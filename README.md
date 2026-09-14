# Babak Badkoubeh — RL engineer · robustness evaluation for open-weight policies

Source for **[badkoubeh.github.io](https://badkoubeh.github.io)**, the research site of Babak Badkoubeh.

I work on reinforcement learning post-training, and I build open measurement tools that answer a question the field doesn't currently report: **where does this model break, and how confident are we in that number?** Not an accuracy-versus-severity curve — a failure threshold in real sensor units (metres, degrees, milliseconds), with a confidence interval, designed to run on a single 24 GB GPU.

The method is dose-response estimation borrowed from toxicology and psychophysics — ED50/EDx thresholds, profile-likelihood intervals, and Fieller intervals on the ratio of two thresholds — applied to robustness evaluation of driving and robotics vision-language-action models.

## Projects

| Project | What it is | Status |
|---|---|---|
| [marginkit](https://github.com/badkoubeh/marginkit) · [page](https://badkoubeh.github.io/projects/marginkit/) | Model-agnostic dose-response threshold estimation: σ\* with confidence intervals, right-censoring, Fieller ratio intervals. No GPU, no ML dependency. | In development |
| [marginbench](https://badkoubeh.github.io/projects/marginbench/) | Open-loop robustness report card for open-weight driving policies, with degradation axes calibrated to IMU and GNSS datasheets. | In development |
| [zeta-bench](https://github.com/badkoubeh/zeta-bench) | Reproducible robustness benchmark for RL, MPC, LQR, world-model, and PID controllers across a physics-grounded disturbance matrix. | Public |

**First application:** reasoning–action consistency in NVIDIA Alpamayo — does the model's explanation stop tracking its trajectory at a lower input-degradation severity than trajectory quality itself degrades? An open question, not a reported result.

## Research interests

RL post-training and what it does to the tails · robustness margins for learned policies · reward design for reasoning–action consistency · dose-response and threshold estimation in ML evaluation · open-weight vision-language-action models · sensor-degradation modelling · interpretability under distribution shift · evaluation methodology · control systems and robust control

More on the [research page](https://badkoubeh.github.io/research/) and what I'm doing [now](https://badkoubeh.github.io/now/).

## Background

- **EE – Robotics, BSc + MSc** — model-based robust RL, PDE dynamic models, Lyapunov stability, H∞ control.
- **CS – ML, MSc** — modern RL, world models, model-based RL, transformer policies.
- **Production ML** — led data/AI at a fintech under strict SLAs with Fortune 500 clients.

## Links

[Website](https://badkoubeh.github.io) · [GitHub](https://github.com/badkoubeh) · [LinkedIn](https://www.linkedin.com/in/babakbadkoubeh)

---

## About this repo

A Jekyll site built and served by GitHub Pages from `main` (with `jekyll-seo-tag` and `jekyll-sitemap`). Pages live in `index.html`, `research.md`, `now.md`, and `projects/`; shared markup is in `_layouts/` and `_includes/`; styles are in `assets/css/site.css`.

Local preview (requires Docker):

```bash
docker run --rm -p 4000:4000 -v "$PWD":/srv -w /srv ruby:3.3 \
  sh -c "bundle install && bundle exec jekyll serve --host 0.0.0.0"
```

Then open http://localhost:4000.
