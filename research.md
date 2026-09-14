---
title: Research interests
description: "Babak Badkoubeh's research: dose-response failure thresholds for ML policies, physically-calibrated perturbations, and whether RL post-training widens margins."
permalink: /research/
---

# Research interests

<p class="subtitle">Babak Badkoubeh · RL post-training, robustness margins, threshold estimation</p>

## Measuring where systems break, not how well they behave {#measuring-where-systems-break}

My background is control systems, and that shapes what I think is missing from machine learning evaluation. A control engineer asked "is this system robust?" doesn't answer with an average-case score. They answer with a **margin**: the smallest perturbation that destabilises the loop. Gain margin, phase margin, the µ-analysis structured singular value — different tools, same shape of answer. The number is actionable because it tells you how much room you have left.

ML robustness evaluation reports something different. It reports performance at a conventionally-chosen perturbation budget — [RobustBench](https://robustbench.github.io/)'s ε = 8/255 is the canonical example — or it plots a degradation curve and leaves the reader to eyeball where the interesting part is. Neither gives you a margin, and neither gives you an uncertainty estimate on one. My work is an attempt to close that gap with statistics that already exist in other fields.

## Dose-response estimation as the missing import {#dose-response-estimation}

The method is dose-response quantile estimation, borrowed from toxicology and psychophysics. Those fields spent decades on exactly this problem: you have a graded stimulus, a binary or continuous response, and you want the dose at which the response crosses a threshold — plus honest uncertainty on that dose. The machinery is mature: probit and logit GLMs, four-parameter log-logistic fits for continuous outcomes, ED50 and EDx estimation, profile-likelihood intervals, right-censoring when the subject never responds in range, and Fieller's theorem for intervals on the *ratio* of two thresholds.

That last one is the piece I find most interesting, because it turns a vague qualitative claim into a testable one. "Capability A degrades before capability B" is untestable as usually stated. "The ratio σ\*(A)/σ\*(B) has a 95% Fieller interval lying below 1" is a claim a reviewer can check.

I'm careful about the framing: dose-response is the *method*, and it comes from toxicology. "Margin" is what it *estimates*, and that word is chosen so control engineers recognise the output. The method didn't come from control theory and I don't claim it did.

## Physically-calibrated perturbations {#physically-calibrated-perturbations}

A threshold is only meaningful if its units are. Most robustness work perturbs inputs by arbitrary multiples of some baseline, or inside an ε-ball chosen by convention. I parameterise degradation axes against real hardware: angle random walk and bias instability figures from named automotive IMU grades, position drift rates and heading RMS from published dead-reckoning field studies. A severity level should map to a sentence like "tunnel GNSS outage at 30 seconds," not to "3× nominal noise." Each axis is designed to carry its datasheet citation in code, and that citation flows through into the report card.

## Reasoning–action consistency in driving VLAs {#reasoning-action-consistency}

The first system I'm measuring is NVIDIA's Alpamayo, an open-weight vision-language-action model for driving that produces a chain-of-causation reasoning trace alongside its planned trajectory. Reasoning traces are increasingly offered as an interpretability affordance — the model tells you *why*. My question is whether that affordance is robust: does the reasoning stop matching the action at a lower input-degradation severity than the trajectory quality itself degrades?

If reasoning–action consistency has the lower threshold, the explanation becomes untrustworthy precisely in the degraded conditions where a human operator would most want to consult it. That's a hypothesis I'm testing, not a finding I'm reporting, and I'll publish the answer either way.

The consistency measure I use is a deterministic, rule-based re-implementation in spirit — it is not NVIDIA's internal reward.

## The question: does RL post-training actually widen the margin? {#does-rl-post-training-widen-the-margin}

I'm an RL engineer, and this is the question I keep running into without being able to answer it.

There's a plausible mechanism for why it should. Supervised fine-tuning optimises agreement with demonstrations that were, almost by construction, collected under nominal conditions — there's no gradient signal for what to do when the state estimate is wrong, because nothing in the demonstration set has a wrong state estimate. RL optimises a return under a distribution the policy itself visits, which at least opens the door to the tails. Add a reward term targeting the specific property you care about — agreement between a model's stated reasoning and its executed action, say — and you have direct pressure on something SFT can only reach indirectly.

That's a mechanism story, not evidence. And when I went looking for the evidence, I found that the question is harder to answer than I expected, for two reasons worth stating up front.

{% comment %}cite: Alpamayo 1 vs 1.5 model cards / release notes (backbone, reasoning-trace data, capabilities){% endcomment %}
**The published checkpoints can't settle it.** The open-weight pairs that look like an SFT-versus-RL ablation aren't one. NVIDIA's Alpamayo 1 and 1.5 differ in more than RL post-training — the release also changes the backbone, the training data and the model's capabilities. Any robustness difference between them has several candidate causes at once. De-confounding it means training a matched pair on a multi-node cluster, which isn't something an independent researcher does on a weekend. So I treat that comparison as confounded and report it that way, rather than dressing it up as an ablation.

{% comment %}cite: published work on RL post-training and adversarial tail risk for this model family{% endcomment %}
**The evidence that does exist is more interesting than "RL helps."** Early published results on this model family suggest RL post-training can improve robustness in the benign regime while *worsening* worst-case behaviour under adversarial input — the mean gets better and the worst case gets worse. If that pattern generalises, then "RL improves robustness" and "RL widens the robustness margin" are two different claims, and the second is the one that matters for deployment. A method that lifts average-case performance while thinning the tail is exactly what a threshold-with-an-interval catches and a headline score hides.

So I don't have a settled answer, and I'm suspicious of the confident versions in either direction. What I can do is build something that would let the question be asked properly — which is most of what I'm currently doing.

## The second half: a harness other people can run {#a-harness-other-people-can-run}

The tooling isn't scaffolding for one paper. It's the half of this I expect to outlast the paper.

The practical artifact I want to exist is a **robustness regression harness for anyone doing post-training**: run it before the RL stage, run it after, and see whether the margin moved — in metres, degrees, and milliseconds, with a confidence interval, on the degradation axes your deployment actually faces. Today a team finishing an RL run has no way to answer "did that buy us robustness, or did it push the failure somewhere we aren't looking?" beyond watching aggregate scores go up.

That's why the statistics live in a separate, model-agnostic package — [marginkit]({{ '/projects/marginkit/' | relative_url }}) — with no ML dependency at all. If the only thing that survives from this project is that other people can estimate a threshold with an honest interval on their own systems — driving, robotics, or something I haven't thought of — that's a good outcome. The Alpamayo study is the first real use of the tool and the thing that will prove it works end to end. It isn't the point of building it.

## Scope, stated honestly {#scope}

This work is **open-loop screening**, not a closed-loop safety argument and not a certification claim. The reason is specific: I'm not aware of published fidelity evidence for neural-rendering driving simulators under off-nominal conditions, and measuring where a policy breaks inside a simulator that may itself break at comparable severities is circular. I'd rather report a narrower result I can defend than a broader one I can't.

## Where this goes {#where-this-goes}

The long-term interest is whether one measurement protocol can characterise systems built on completely different principles — a hand-tuned PID controller on a rocket in [zeta-bench](https://github.com/badkoubeh/zeta-bench) and a large learned driving policy — and return comparable numbers. The statistics package is currently the shared piece between those two, which is at least a real test of whether the core is domain-agnostic. Whether the *interpretation* transfers as cleanly is a more interesting and less settled question.

## Topics I'd be glad to talk about {#topics}

<ul class="tags">
  <li>RL post-training and the tails</li>
  <li>Robustness margins for learned policies</li>
  <li>Reward design for reasoning–action consistency</li>
  <li>Dose-response &amp; threshold estimation in ML evaluation</li>
  <li>Open-weight vision-language-action models</li>
  <li>Sensor-degradation modelling</li>
  <li>Interpretability under distribution shift</li>
  <li>Evaluation methodology &amp; benchmark saturation</li>
  <li>Single-GPU research infrastructure</li>
</ul>

See what I'm working on [this month]({{ '/now/' | relative_url }}), or find me on [GitHub](https://github.com/badkoubeh) and [LinkedIn](https://www.linkedin.com/in/babakbadkoubeh).
