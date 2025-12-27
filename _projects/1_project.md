---
layout: page
title: Lunar Terrain Generator
description: A generation algorithm based on topographic statistics of the moon
img: assets/img/ltg_cover.png
importance: 1
category: work
related_publications: true
---

This project is a procedural lunar terrain heightmap generator for Python. The algorithm is grounded in established models of lunar geology and crater formation, generating realistic terrain at a scale of 1 meter per pixel.

To ensure realism, the generated terrain was validated against real lunar topographic statistics taken from the LROC RDR Data Portal. Note that the highest resolution data provided by the data portal is 2 meters per pixel, which motivated the creation of this generator to have access to data at 1 meter per pixel. The generator can produce distinct landscapes representative of the two key lunar types: smooth, basaltic Mare terrain and rugged, heavily-cratered Highland terrain.

<div class="text-center">
<a href="https://github.com/anderspearson206/LunarTerrainGenerator" target="_blank" rel="noopener noreferrer" class="btn btn-outline-primary rounded-pill">
<i class="fa-brands fa-github"></i> View on GitHub
</a>
</div>

<hr>

<h2 class="mt-5">Real vs. Generated Landscapes</h2>

A primary goal of this project was to generate synthetic terrain that is statistically and visually indistinguishable from real lunar topographies. Note that the terrains are created as 2d images (same
as the LROC DTM data), and then later converted into meshes.

<div class="row">
<div class="col-sm mt-3 mt-md-0">
{% include figure.liquid loading="eager" path="assets/img/real_lunar_terrain_1.png" title="Real Terrain 1" class="img-fluid rounded z-depth-1" %}
</div>
<div class="col-sm mt-3 mt-md-0">
{% include figure.liquid loading="eager" path="assets/img/real_lunar_terrain_2.png" title="Real Terrain 2" class="img-fluid rounded z-depth-1" %}
</div>
<div class="col-sm mt-3 mt-md-0">
{% include figure.liquid loading="eager" path="assets/img/real_lunar_terrain_3.png" title="Real Terrain 3" class="img-fluid rounded z-depth-1" %}
</div>
</div>
<div class="caption">
Examples of <strong>real</strong> lunar topographic data from the LROC RDR Portal, used for statistical validation.
</div>

<div class="row">
<div class="col-sm mt-3 mt-md-0">
{% include figure.liquid loading="eager" path="assets/img/fake_lunar_terrain_1.png" title="Generated Terrain 1" class="img-fluid rounded z-depth-1" %}
</div>
<div class="col-sm mt-3 mt-md-0">
{% include figure.liquid loading="eager" path="assets/img/fake_lunar_terrain_2.png" title="Generated Terrain 2" class="img-fluid rounded z-depth-1" %}
</div>
<div class="col-sm mt-3 mt-md-0">
{% include figure.liquid loading="eager" path="assets/img/fake_lunar_terrain_3.png" title="Generated Terrain 3" class="img-fluid rounded z-depth-1" %}
</div>
</div>
<div class="caption">
Examples of <strong>synthetic</strong> terrain from the generator, matching the statistical properties of real landscapes.
</div>

<hr>

<h2 class="mt-5">How it Works</h2>

The generator is implemented in Python and uses performance optimizations from Numba for parallel processing. The methodology is segmented into three primary stages:

1. Crater Profile Generation

First, a library of precomputed crater profiles is generated based on models derived from high-resolution LROC data. A crater's morphology is a function of its diameter, $D$. The topographic profile is modeled by a piecewise function describing the flat central floor, the cubic polynomial of the inner wall, and the exponential decay of the exterior region.

2. Cratering Simulation

The algorithm simulates cratering events over a specified geological age. The number and size distribution of craters are determined using the Neukum production function and the lunar impact chronology curve. For each time step, crater diameters are sampled from this distribution and added to the heightmap at random locations, allowing the terrain to evolve over time.

3. Topographic Diffusion and Final Touches

Finally, the terrain undergoes a diffusion process to model the effects of long-term erosion and degradation. This is solved numerically by applying a finite-difference method based on the heat diffusion equation, which iteratively smooths sharp features. To enhance realism, the algorithm also adds high-frequency Perlin noise, simulates small rocks, and introduces small, young craters to complete the landscape.
