---
title: The Journey to UDP Multiplayer
category: Draft
author: 🗲 Shockk
description: A detailed overview of the work I've done to properly support server/client networking.
---
It's been a long road forward, with seemingly infinite meandering twists and turns along the way, but we're finally at a point where we can say we have working netcode. Well, sort of. But how exactly did we get to this point, and what challenges did we face on the journey? Those questions are what I'm aiming to answer in this blog post.

# Background

Before anything else, I should give a brief overview of how exactly our engine works.

At the core of the engine, we have a sophisticated entity-component-system architecture designed for efficient insertion and lookup. How the core is implemented is outside the scope of this article, but what's important is that we represent game objects as a mapping between a unique ID and a set of discrete components.

If we want to render a cube, we grab a new object ID and map it with a __model__ component encapsulating the cube's vertices. If we want to place that cube somewhere, we give it a __position__ component encapsulating the cube's position in world space. If we want to rotate the cube a little bit, we give it an __orientation__ component. If we want to position and rotate it but not actually render it, we just remove the model component. You get the idea—each component represents some discrete data.

The most important detail here is how we actually represent the data in components like position and orientation. A naïve implementation might use a 3-component vector for the position and a quaternion for the orientation. However, our approach was to implement these as special kinds of values we call __integrables__. At its simplest level, an integrable is just a wrapper around a value, allowing us to give it _another_ integrable to represent its rate of change. That rate of change can be given _another_ integrable to represent _its_ rate of change, and so on, and so on.

Putting this all together, we have an integrator system that passes over every integrable on every component on every object, integrating its value at a fixed timestep. Currently we use Newton's equationsecond law of linear motion for integration, accurately integrating the position/orientation/etc up to its second-order derivative (acceleration in the case of position).

We run the integrator at a fixed timestep to maintain consistency across frames of different time chunks. Any left-over time is carried over to the next frame and also used by other systems to quickly calculate an approximate value at the current time. For example, currently the integrator runs at 50 fps and hence, on a 144 Hz monitor (assuming no frame drops), would only tick once for every 2-3 frames. However, all motion would appear smooth due to the approximation based on the left-over time.

# Basic Networking

I had a decision to make, before writing even a single line of code. TCP or UDP? Let's go over each of them.

## TCP

* Has a concept of connections. You can open a connection and the other side will know your packets are coming from you.
* Reliable. If you send a packet, you know it'll eventually get there.
* Ordered. If you send a bunch of packets, you know they'll arrive in the right order.
* Flow control. You won't send packets too fast for the connection to handle.

## UDP

* Has no concept of connections. You have to implement handshakes and keep track of connection IDs or similar.
* Unreliable. If you send a packet, it might never arrive. The best part about UDP jokes is that I don't care if you get it or not.
* Unordered. Packets might arrive out of order, not at all, or even multiple times.
* No flow control. If you send packets too fast, your connection might not be able to handle them and just drop them.

The clear, obvious winner is UDP. You might be scratching your head right about now, but I'll explain. TCP is designed to ensure that what you send is what the other end receives, including reassembling packets in order and making sure packets are resent if they aren't received. What it's not designed for are real-time applications that don't have time to wait for a single dropped packet to be resent before they can continue.

In a real-time game, we don't actually need packet ordering at a protocol level—we need to be able to get the most recent data to the other end as fast and efficiently as possible. We still need our networking model to be reliable, but we have other options that don't require waiting for an acknowledgement before doing anything else.

The way we implement reliability in our model is by giving each packet a packet ID as well as a list of 32 single-bit acknowledgements for the previous 32 packets the other end sent. Because this is a real-time game, the client and server are going to be exchanging data many times per second, so we can take advantage of this by having them let each other know which of the last 32 packets they received. That way, those dropped packets can be resent if needed, out-of-order, with the correct packet ID.

We also need to have a notion of connections in our protocol if we want to support more than one client/server at a time. For now, the client sends a `connection_request` command to the server along with a unique connection ID with which it will track the server. The server then sends back a `connection_accept` with the packet's connection ID field set to the same one the client just sent, along with _its own_ unique connection ID with which to track the client. The client and server then send these corresponding connection IDs in all further packet headers.

# State Protocol

The first protocol I designed for the game revolved around sending state updates every network tick (currently 10 ticks per second). Let's see the packet header first.

| Field         | Size (bytes) |
| ------------- | ------------:|
| Connection ID |            4 |
| Packet ID     |            1 |
| ACKs          |            4 |
| __Total__     |        __9__ |

And now let's see the state data.

| Field                        | Type   | Size (bytes) |
| ---------------------------- | ------ | ------------:|
| Position                     | point  |   3 x 4 = 12 |
| Velocity                     | point  |   3 x 4 = 12 |
| Velocity Lerp Target         | point  |   3 x 4 = 12 |
| Velocity Lerp Factor         | float  |            4 |
| Acceleration                 | point  |   3 x 4 = 12 |
| Orientation                  | quat   |   4 x 4 = 16 |
| Angular Velocity             | euler  |   3 x 4 = 12 |
| Angular Velocity Lerp Target | euler  |   3 x 4 = 12 |
| Angular Velocity Lerp Factor | float  |            4 |
| Angular Acceleration         | euler  |   3 x 4 = 12 |
| __Total__                    |        |      __108__ |

This data is sufficient to fully represent the current state of a player ship, as it encapsulates all variables representing their position and orientation as well as how those values morph over time. As such, the integrator system would integrate these values in between network frames, providing smooth movement for remote players. To avoid jitter resulting from a lack of time synchronization, we would interpolate towards the new position and orientation rather than directly set them.

The problem arises when you consider how much bandwidth this kind of protocol would require. Including the 16-byte IP header and the 4-byte UDP header, our total packet size adds up to __137__ bytes. This structure also does not include any sort of unique identifier specifying which local object's state should be updated. Assuming we used a 2-byte integer to represent that unique ID, we're now up to __139__ bytes.

Amplifying this up to hundreds of movable and rotatable objects like ships, asteroids, energy weapons fire, missiles, etc, could bring our total data per network frame up to, say, 500 x 139 = __67.87 KiB__. When we take into account how many network frames we're sending per second (10), that's __678.71__ KiB/s. And that doesn't even include _anything_ else we would need to send aside from state. Quite simply, that's an unacceptable bandwidth requirement for us.

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQzNzI4MDUwMl19
-->
