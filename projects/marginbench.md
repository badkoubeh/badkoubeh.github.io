---
layout: project
title: "marginbench — robustness report cards"
description: "marginbench: an open-loop robustness report card for open-weight driving policies, with degradation axes calibrated to real IMU and GNSS sensor datasheets."
permalink: /projects/marginbench/
software:
  name: marginbench
  description: "Open-loop robustness evaluation harness for open-weight driving policies: physically-calibrated degradation axes, model adapters, and an auto-generated robustness report card built on marginkit."
  status: In development
---

# marginbench

<p class="subtitle">A robustness report card for open-weight driving policies</p>

<div class="callout"><p>In development. This page describes the design; there's no public release yet.</p></div>

<div class="code-label">Planned CLI</div>

```bash
marginbench run --model alpamayo-1.5 --axis ego-noise --scenarios ./data --out card.html
# → threshold σ* = … m, 95% CI […, …]
```

## Degradation axes, in physical units

- **Ego-state additive noise** — σ in metres and degrees, grounded in named automotive IMU grades.
- **Ego-state bias and drift** — percent-of-distance position drift and heading RMS, grounded in published dead-reckoning field data.
- **Observation staleness** — τ in milliseconds, in multiples of the sensor frame period.

Every axis carries its citation in code, and that citation appears on the report card — not arbitrary σ multiples or conventional ε-balls.

## Two measures, one grid

A trajectory-quality measure and a deterministic, rule-based reasoning–action consistency measure are evaluated on the same fixed severity grid, so their thresholds are directly comparable and the Fieller ratio interval from [marginkit]({{ '/projects/marginkit/' | relative_url }}) means something. Adaptive dosing would put the two curves under different conditions and destroy exactly the comparison the harness exists to make.

The consistency measure is a re-implementation in spirit — it is not NVIDIA's internal reward.

## Built to take more models

The adapter interface comes before the first adapter. Bring-your-own-data is a first-class path, and an ungated synthetic demo is planned so that installing it gives you a working example without waiting on a dataset approval queue.

## Scope

Open-loop evaluation, inference only, on one 24 GB GPU. A full three-axis sweep is estimated at tens of GPU-hours of embarrassingly parallel work. See [why it's open-loop]({{ '/research/#scope' | relative_url }}).
