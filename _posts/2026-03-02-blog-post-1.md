---
title: 'noise and growth in blueprint'
date: 2026-03-02
permalink: /posts/2026/03/noise-and-growth-in-blueprint/
tags:
  - procedural generation
  - geometry script
---

A Blueprint implementation of 1D Perlin-like noise for procedural vegetation growth
======

Vegetation is one of the biggest challenge that can be encountered while creating scenes in Unreal. Along the design process, scenes change, assets get rebuilt and placing ready-made vegetation assets in a way that feels natural can become a thorny task.

As a consequence, the possibility to dress a set with vegetation with realistic natural growth and adapted to the assets placement and that can be re-generated in a couple seconds at will is a considerable advantage. 
It is flexible, time-saving and an art-based process.

The Geometry Script framework in Unreal Engine offers a comprehensive solution to create and edit procedural assets through Blueprint and C++ logic. While there might be one node that does exactly this, I have not found a node that could output Perlin-like values 

From there, it became high-reward to explore how to create these natural patterns natively in Unreal and utilize the technique for a Vine Asset Creation tool. 

Once the foundation of the tool is laid down, the user input and the branch creation handled,   it is time to look for ways to randomize our spline branches' shapes.

We want it to look like Perlin noise – soft, contained between maximum and minimum value range and continuous.

**Perlin Noise**
In a nutshell, Perlin Noise is a smooth and organic looking noise that was developped by Ken Perlin in 1982 to simulate surfaces or volume such as terrain, rocks or clouds. 
For our vine growth example, we will only need a one-dimensional Perlin noise: a wiggly line that we will be slightly pointing toward the sky.

<img src='https://en.wikipedia.org/wiki/File:Perlin_noise_example.png'>


We can obtain a Perlin noise in two simple steps:
creating random values contained in a range
smoothing – or interpolating –  the values in between

**Implementation**

Initializing our arrays:

<img src='/images/Screenshot%202026-03-10%20040639.png'>

Our starting point before we apply the perlin noise is the world coordinates of given points along a straight spline branch.

We know that to get the wavy look we will have to randomize some of the values at a regular interval and  interpolate those in between. So we start by creating a parameter at the level of our Scriptable Tool to decide how far apart those random values will be : the noise resolution parameter.
As it significantly influences the look of the vine thus it makes sense to keep it accessible.

In our function, we add those selected indices = boundaries to a first array “Res Indices”:

<img src='/images/Screenshot%202026-03-10%20040738.png'>

Note: an additional random value as to what was expected is needed here as we also have to interpolate the far-out values that might be there

Now to assign random values to the indices we selected, we create a simple hash function:

Frac(Seed * InputVariable) = RandomValue

Seed being a pre-determined 1D parameter (any). 
InputVariable being the Y coordinate of our point index.

For each given point along the vine branch, the function multiplies its initial Y position with a preset seed and keeps the fraction part of the result.

We store these values into a second array called Random Ys. The additional index added in the previous step will also need a corresponding random value.

<img src='/images/Screenshot%202026-03-10%20040832.png'>

Now let's get the smoothed values in between. Let's create a third array “Interpolated Ys” that will contain both our random values and the interpolated ones. 

For each of these indices, we find out whether they are a random set value or if they are an in-between one
--> if set value we add it to the array as is
--> otherwise, by using the closest “Res Indices” and associated noise value “Random Ys” above and below it, we are able to interpolate it and to choose the degree of interpolation. In our case 2 is a fitting choice. The calculated value is added to “Interpolated Ys”.

<img src='/images/Screenshot%202026-03-10%20041157.png'>

Unreal provides a node to interpolate values “Finterp Ease In Out”. In our case, we use it as follows. 

<img src='/images/Screenshot%202026-03-10%20041237.png'>

The gist of the idea is that we first interpolate the indices of the points then use that result to get the interpolated noise values.

Lastly, we output the noise values from “Interpolated Ys” using our minimum and maximum parameters. We finish by re-attaching the branch to the stem by using the original un-displaced coordinate for the very beginning of the vine branch:

<img src='/images/Screenshot%202026-03-10%20041656.png'>




