---
layout: project
title: "marginkit — failure thresholds with CIs"
description: "marginkit: model-agnostic dose-response threshold estimation. Failure thresholds with profile-likelihood and Fieller intervals, right-censoring handled."
permalink: /projects/marginkit/
software:
  name: marginkit
  description: "Dose-response threshold and robustness-margin estimation for any severity-outcome dataset. Probit/logit GLMs, EDx thresholds, profile-likelihood intervals, Fieller intervals on threshold ratios, right-censoring."
  repo: https://github.com/badkoubeh/marginkit
  status: In development
---

# marginkit

<p class="subtitle">Failure thresholds with confidence intervals, for any system</p>

`marginkit` takes a table of `(severity, outcome)` observations and returns the severity at which the system fails — with an interval around it.

<div class="code-label">Interface sketch — may change during extraction</div>

```python
from marginkit import fit_dose_response, threshold, ratio_interval

fit = fit_dose_response(severity, outcome, family="binomial",
                        link="probit", severity_scale="log")
t   = threshold(fit, p=0.5, ci="profile")
# Threshold(value=…, lo=…, hi=…, method='profile', censored=False)
```

## What it's built to do

- Binomial GLM fits (probit, logit, cloglog) and four-parameter log-logistic fits for continuous outcomes.
- EDx / σ\* estimation at arbitrary *p*.
- Profile-likelihood intervals as the default, the delta method as a cross-check, parametric bootstrap as a fallback.
- **Fieller intervals on the ratio of two thresholds**, for testing whether one failure mode has a lower threshold than another.
- Right-censoring, so a system that never breaks in range gets a one-sided lower bound rather than a silently-reported last grid point.
- Monotonicity and goodness-of-fit diagnostics.
- Serialisable result objects and a report-card schema.

## What it deliberately doesn't do

It has no GPU dependency, no ML-framework dependency, and no idea what a vehicle is. `numpy`, `scipy`, `statsmodels`, nothing else in the core. Planned: threshold and Fieller values validated against R `drc` reference results on published datasets.

## Where it came from

`marginkit` is being extracted, with history intact, from [zeta-bench](https://github.com/badkoubeh/zeta-bench) — a control-theory benchmark where it estimates robustness margins for a PID controller on a 6-DOF rocket. Its second consumer is [marginbench]({{ '/projects/marginbench/' | relative_url }}), in an unrelated problem domain, which makes it a real test of whether the statistics core is genuinely domain-agnostic. (Both consumers share an author — that's evidence of domain-generality, not of external adoption.)

It isn't on PyPI yet. Follow progress on [GitHub](https://github.com/badkoubeh/marginkit).
