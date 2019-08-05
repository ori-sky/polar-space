---
title: The Journey to UDP Multiplayer
category: Draft
author: 🗲 Shockk
description: A detailed overview of the work I've done to properly support server/client networking.
---
It's been a long road forward, with seemingly infinite meandering twists and turns along the way, but we're finally at a point where we can say we have working netcode. Well, sort of. But how exactly did we get to this point, and what challenges did we face on the journey? Those questions are what I'm aiming to answer in this blog post.

# Background

Before anything else, I should give a brief overview of how exactly our engine works.

At the core of the engine, we have a sophisticated entity-component-system architecture designed for efficient insertion and lookup. How the core is implemented is outside the scope of this article, but what's important is that we represent game objects as a mapping between a unique ID and a set of discrete components. If we want to render a cube, we grab a new object ID and map it with a __model__ component encapsulating the cube's vertices. If we want to place that cube somewhere, we give it a __position__ component encapsulating the cube's position in world space. If we want to rotate the cube a little bit, we give it an __orientation__ component. If we want to position and rotate it but not actually render it, we just remove the __model__ component. You get the idea—each component represents some discrete data.

The most important detail here is how we actually represent the data in components like __position__ and __orientation__. A naïve implementation might use a 3-component vector for the position and a quaternion for the orientation. However, our approach was to implement these as special kinds of values we call __integrables__. At its simplest level, an integrable is just a wrapper around a value, allowing us to give it _another_ integrable to represent its rate of change. That rate of change can be given _another_ integrable to represent _its_ rate of change, and so on, and so on.

Putting this all together, we have an integrator system that passes over every integrable on every component on every object, integrating its value at a fixed timestep. Currently we use Newton's second law of linear motion for integration, accurately integrating the position/orientation/etc up to its second-order derivative (acceleration in the case of position). We run the integrator at a fixed timestep to maintain consistency across frames of different time chunks. Any left-over time is carried over to the next frame and also used by other systems to quickly calculate an approximate value at the current time. For example, currently the integrator runs at 50 fps and hence, on a 144 Hz monitor (assuming no frame drops), would only tick once for every 2-3 frames. However, all motion would appear smooth due to the approximation based on the left-over time.

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
