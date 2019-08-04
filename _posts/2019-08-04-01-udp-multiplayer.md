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

The most important detail here is how we actually represent the data in components like __position__ and __orientation__. A naïve implementation might use a 3-component vector for the position and a quaternion for the orientation.

TBC
