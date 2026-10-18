---
layout: project
title: "robustmargin — failure thresholds with CIs"
description: "robustmargin: model-agnostic dose-response threshold estimation for Python. Failure thresholds with profile-likelihood and Fieller intervals, censoring handled."
permalink: /projects/robustmargin/
software:
  name: robustmargin
  description: "Dose-response threshold and robustness-margin estimation for any severity-outcome dataset. Binomial probit/logit/cloglog fits, thresholds with profile-likelihood or delta intervals, censoring, Fieller and log-delta intervals on threshold ratios, fit diagnostics."
  repo: https://github.com/robustmargin/robustmargin
  license: https://spdx.org/licenses/Apache-2.0.html
  version: "0.1.1"
  status: Alpha
---

# robustmargin

<p class="subtitle">Failure thresholds with confidence intervals, for any system</p>

`robustmargin` takes success counts at increasing levels of stress and returns the severity at which performance crosses a level you choose — with an interval around it. When the data can't support a number, it says so: a threshold beyond the tested range is reported as a censored bound, and a failed fit carries a status flag instead of an estimate.

```bash
pip install robustmargin
```

Python 3.11 or later, Apache-2.0. 0.1.1 is the first release; while the version is below 1.0, a minor release may change the API.

<div class="code-label">Worked example — Finney's (1971) toxicity data</div>

```python
from robustmargin import Axis, Definition, Observations, fit_dose_response, threshold

# Finney (1971): insects exposed to increasing doses; the outcome is survival.
obs = Observations.from_counts(
    Axis(name="dose", unit="mg", scale="log"),
    severity=[0.0, 2.6, 3.8, 5.1, 7.7, 10.2],
    successes=[49, 44, 32, 22, 7, 6],
    trials=[49, 50, 48, 46, 49, 50],
    outcome="survival",
    direction="decreasing",
)

# The link and the asymptotes are chosen up front; nothing is auto-selected.
fit = fit_dose_response(obs, model="binomial", link="probit", upper=1.0, lower=0.0)
t50 = threshold(fit, definition=Definition.absolute(0.5),
                interval_method="profile", level=0.95)
# t50.value, t50.lo, t50.hi: the dose at which survival falls to 50%,
# with a 95% profile-likelihood interval
```

## What 0.1.1 does

- Binomial dose-response fits with a probit, logit or complementary log-log link, each asymptote fixed or estimated.
- Thresholds under three definitions: a fixed performance level, a fraction of control performance, or a fraction of the fitted dynamic range.
- Profile-likelihood or delta-method confidence intervals.
- Censoring, so a system that never breaks in range gets a one-sided bound rather than a silently-reported last grid point, and a failed fit reports exact bounds where the data support them and no estimate.
- **Fieller or log-delta intervals on the ratio of two thresholds**, for testing whether one failure mode has a lower threshold than another.
- Goodness-of-fit, dispersion and link-comparison diagnostics, plus a monotonicity check on the raw counts. They're reported, never acted on: nothing is rescaled, and the link you chose is never swapped for a better-scoring one.
- A grid break-point rule — the first tested severity at which observed performance falls below a criterion — labelled as an observed grid statistic, not an estimated threshold.
- Frozen, self-checking result objects, written to a JSON score card against a versioned JSON Schema that ships in the package.

## What it deliberately doesn't do

It has no GPU dependency, no ML-framework dependency, and no idea what a vehicle is. `numpy`, `scipy`, `statsmodels`, nothing else. Fits, thresholds and ratio intervals are tested against reference results from R's `drc` package.

## Not yet

Continuous outcomes, clustered and paired inference, and design helpers are planned for 0.2. Until clustered inference lands, an overdispersed fit's intervals should be treated as too narrow — the diagnostics flag when that applies.

## Where it came from

The grid break-point rule started in [zeta-bench](https://github.com/badkoubeh/zeta-bench) — a control-theory benchmark whose reference environment is 6-DOF rocket landing — and zeta-bench's per-controller robustness cards call it from the package today. The second consumer, [marginbench]({{ '/projects/marginbench/' | relative_url }}), is being built on it in an unrelated problem domain, which makes it a real test of whether the statistics core is genuinely domain-agnostic. (Both consumers share an author — that's evidence of domain-generality, not of external adoption.)

[GitHub](https://github.com/robustmargin/robustmargin) · [PyPI](https://pypi.org/project/robustmargin/)
