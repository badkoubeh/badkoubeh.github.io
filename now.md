---
title: Now
description: "What Babak Badkoubeh is working on now: running NVIDIA Alpamayo on one 24 GB GPU, extracting marginkit, and building a reasoning–action consistency measure."
permalink: /now/
---

# Now

<p class="updated">Updated September 2026.</p>

- Standing up the released Alpamayo inference path on a single 24 GB GPU and measuring real per-inference wall-clock. (Published latency figures reflect an optimised runtime; I expect the released PyTorch path to be considerably slower.)
- Extracting [`marginkit`]({{ '/projects/marginkit/' | relative_url }}) from [`zeta-bench`](https://github.com/badkoubeh/zeta-bench) with history preserved, and generalising anything that still assumes a rocket.
- Building the rule-based reasoning–action consistency extractor (a re-implementation in spirit, not NVIDIA's internal reward) and hand-labelling its ~100-pair calibration set.
- Running a standing literature watch on reasoning–trajectory consistency for this model family — it's an active area and I'd rather cite concurrent work than collide with it.

## Open to conversations

About RL post-training and what it does to the tails, robustness-evaluation methodology, collaborators for the first preprint, and anyone running post-training who wants a regression harness for "did that RL run actually buy robustness, or just move the failure somewhere we aren't looking?"

Reach me on [LinkedIn](https://www.linkedin.com/in/babakbadkoubeh) or [GitHub](https://github.com/badkoubeh). The longer version is on the [research page]({{ '/research/' | relative_url }}).
