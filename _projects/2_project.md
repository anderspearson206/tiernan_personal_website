---
layout: page
title: RadioLunaDiff
description: Estimation of wireless network signal strength in lunar terrain
img: assets/img/arch7.png
importance: 2
category: work
giscus_comments: false
published: true
---

This work was done in the [UW Sensor Systems Lab](https://sensor.cs.washington.edu/) in collaboration with several other talented researchers, namely [Paolo Torrado](https://paolotorrado.com/). As an undergraduate, I built a data generation pipeline, using my own custom terrain generation algorithm and [Sionna-RT](https://github.com/NVlabs/sionna-rt). I also developed and implemented the NN prediction architecture. In September of 2025 we submitted a paper to [ICASSP](https://2026.ieeeicassp.org/).

This project introduces a novel physics-informed deep learning architecture for predicting radio maps (wireless signal strength) over complex lunar terrain. This work supports future communication-aware mission planning for NASA's proposed LunaNet framework.

- [**Project Website**](https://radiolunadiff.github.io/)
- [**Paper (arXiv)**](https://arxiv.org/pdf/2509.14559)
- [**GitHub Repository**](https://github.com/anderspearson206/RadioLunaDiff)

---

## Abstract

In this paper, we propose a novel physics-informed deep learning architecture for predicting radio maps over lunar terrain. Our approach integrates a physics-based lunar terrain generator, which produces realistic topography informed by publicly available NASA data, with a ray-tracing engine to create a high-fidelity dataset of radio propagation scenarios. Building on this dataset, we introduce a triplet-UNet architecture, consisting of two standard UNets and a diffusion network, to model complex propagation effects. Experimental results demonstrate that our method outperforms existing deep learning approaches on our terrain dataset across various metrics.

---

## System Architecture

Our approach uses a pipeline of three cascaded models—two UNets followed by a diffusion network—to predict a radio map. The first UNet estimates the binary square wave number map ($k^2$). The second UNet produces an initial radio map estimate. Finally, a Denoising Diffusion Probabilistic Model (DDPM) refines the initial map by predicting the residual.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/arch7.png" title="Proposed system architecture (Fig. 2 from paper)" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

---

## Example Inputs & Outputs

The model takes several inputs, including the terrain height map, a high-pass filtered version of the terrain, and a one-hot map of the transmitter's location.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sample_700_details/input_terrain.png" title="Input Terrain" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sample_700_details/input_filtered_terrain.png" title="Input Filtered Terrain" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sample_700_details/input_tx_map.png" title="Input Transmitter Location" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Example inputs to the model pipeline.
</div>

Below are the intermediate outputs from the model (at 415 MHz), showing the predicted $k^2$ map, the initial coarse radio map, and the predicted residual used for refinement.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sample_700_details/predicted_k2_map_415.png" title="Predicted k² Map (415 MHz)" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sample_700_details/initial_pred_radiomap_415.png" title="Initial Radio Map (415 MHz)" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sample_700_details/predicted_residual_415.png" title="Predicted Residual (415 MHz)" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Intermediate outputs from the model pipeline (415 MHz).
</div>

Finally, here is a comparison of the model's final refined prediction against the ground truth (from the Sionna ray-tracer) for both 415 MHz and 5.8 GHz frequencies.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sample_700_details/final_rm_415.png" title="Final Predicted RM (415 MHz)" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sample_700_details/ground_truth_415.png" title="Ground Truth RM (415 MHz)" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Final model prediction vs. ground truth (415 MHz).
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sample_700_details/final_rm_58.png" title="Final Predicted RM (5.8 GHz)" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sample_700_details/ground_truth_58.png" title="Ground Truth RM (5.8 GHz)" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Final model prediction vs. ground truth (5.8 GHz).
</div>
