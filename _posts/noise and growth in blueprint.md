---
title: 'noise and growth in blueprint'
date: 2026-03-02
permalink: /posts/2026/03/noise-and-growth-in-blueprint/
tags:
  - procedural generation
  - geometry script
---

A Blueprint implementation of 1D Perlin-like noise for procedural vegetation growth

Vegetation is one of the biggest challenge that can be encountered while creating scenes in Unreal. Along the design process, scenes change, assets get rebuilt and placing ready-made vegetation assets in a way that feels natural can become a thorny task.

As a consequence, the possibility to dress a set with vegetation with realistic natural growth and adapted to the assets placement and that can be re-generated in a couple seconds at will is a considerable advantage. 
It is flexible, time-saving and an art-based process.

The Geometry Script framework in Unreal Engine offers a comprehensive solution to create and edit procedural assets through Blueprint and C++ logic. While there might be one node that does exactly this, I have not found a node that could output Perlin-like values 

From there, it became high-reward to explore how to create these natural patterns natively in Unreal and utilize the technique for a Vine Asset Creation tool. 

Once the foundation of the tool is laid down, the user input and the branch creation handled,   it is time to look for ways to randomize our spline branches' shapes.

We want it to look like Perlin noise – soft, contained between maximum and minimum value range and continuous.

Perlin Noise 
In a nutshell, Perlin Noise is a smooth and organic looking noise that was developped by Ken Perlin in 1982 to simulate surfaces or volume such as terrain, rocks or clouds. 
For our vine growth example, we will only need a one-dimensional Perlin noise: a wiggly line that we will be slightly pointing toward the sky.

https://en.wikipedia.org/wiki/File:Perlin_noise_example.png

We can obtain a Perlin noise in two simple steps:
creating random values contained in a range
smoothing – or interpolating –  the values in between

Implementation

Initializing our arrays:

Headings are cool
======

You can have many headings
======

Aren't headings cool?
