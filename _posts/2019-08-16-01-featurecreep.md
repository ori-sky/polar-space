---
title: Feature Creep
category: Draft
author: 🦊 The Fox
description: Designing a game comes with a huge problem, "Feature Creep". In this post I detail how we are trying to avoid it, whilst still getting everything we want in the game.
---

# Concepting and Design

When you're building anything creative, you have two routes you can take.  
You can jump in head first and see where the wind blows you, or you can sit down and carefully plan out what you want to build.  
Of course, we do things the awkward way, and have ended up with a strange hybrid of these methods.  
The design document itself, when it was originally put up on Google Docs is just an incoherent string of thoughts spewed out onto a digital page. Weapon concepts, features we wanted to see, game modes, maps...  
I'm doing my best to port this over to this site, and as close to verbatim as possible so you can see what our "ultimate" desires are, however it's unlikely that we will implement everything we have laid out in [that document](https://shockkolate.github.io/polar-space/design).  
That's where "Feature Creep" comes in.



# What is "Feature Creep"?

Put simply, at least for us, "Feature Creep" is where we started with a simple concept (deathmatch style FPS in space) and expanded into huge numbers of game modes, ship classes and concepts. Each new concept appearing as we go "Hey! That would be awesome to have!" without considering how it would impact our development process.  
Our original scope has bloated way beyond what we originally intended, so we've had to do something about it.



# Fixing "Feature Creep"
## Now
For us, we still want our ultimate goals to be reached, so we've decided to split development into "Expansions".  
The "Base Game", at least initially, will be the [Technical Demonstration](https://shockkolate.github.io/polar-space/designs/prototype) which will have the following features:

- A single pilotable ship, with "final game" manoeuvrability.
- A single projectile weapon, with simple damage.
- A single health bar, a more advanced damage model will follow.
- Collision Detection, seems simple, but isn't.
- "Basic" netcode, allow people to play the demo together.
- "Asteroid Field" Map, got to have something to fly around.
- A basic "AI", something to hone your skills on, may or may not shoot back.

We will not deviate from these features at the technical demo stage, this keeps us on track, and stops us from wasting time building features that are superfluous to our goal.

## Next

The "Base Game" will then be updated to be more fleshed out:

- Multi Stage Health System, complete with a dynamic damage model.
- Multiple Weapons, with a variety of damage types and "feels".
- Updated Textures and Models, "Programmer Graphics" will only take you so far.
- More Maps!
- "Advanced" netcode, allowing people to spin up custom games.
- Sound, full dynamic musical score, sound effects and (maybe) voice lines.
- "Map Development Kit", this one is a maybe, but would make development easier, and allow players to make maps too!
- Enemy AI ships, something to shoot that definitely shoots back.
- Scoring system, counting frags.
- Deathmatch and Team Deathmatch game modes.

## Future

Our first "Expansion" is rather ambitious, but is a big deal:

- Class based game modes, multiple ships, new maps, team strategy!
- Matchmaking, complete with an ELO/Ranking system.
- Ongoing balance patches, community engagement.



# Keeping Focus

Staying on track and keeping development focussed on each set of specific features for that "expansion" isn't going to be easy. What is easy is getting side-tracked, or overambitious.  
But if we can ensure that we stay focussed on our feature goals for each "expansion" we should be able to keep development going at a reasonable pace.

[The Design Document](https://shockkolate.github.io/polar-space/design) is a "living" document and will be updated and polished as we continue development. Some features will be added, some features will be removed.  
Once the Google Doc has been fully ported over, I plan to keep a [record](https://shockkolate.github.io/polar-space/designs/cutting-room) of stuff that was cut from development and why.

-{{ page.author }}

