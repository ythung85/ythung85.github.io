---
layout: post-single
title: "Optimal Transport: A Practitioner's Guide"
date: 2025-03-15
tags: [stat, ml]
excerpt: "From theory to implementation — understanding how optimal transport reshapes probability distributions for machine learning applications."
permalink: /post/optimal-transport/
---

## What Is Optimal Transport?

Imagine you have a pile of dirt in one configuration and you want to reshape it into a target configuration. **Optimal transport (OT)** asks: what is the most efficient way to move the dirt, minimizing the total work done?

Formally, given two probability distributions $\mu$ and $\nu$ over spaces $\mathcal{X}$ and $\mathcal{Y}$, OT finds a **transport plan** $\pi \in \Pi(\mu, \nu)$ that minimizes the expected cost:

$$W_c(\mu, \nu) = \inf_{\pi \in \Pi(\mu, \nu)} \int_{\mathcal{X} \times \mathcal{Y}} c(x, y) \, d\pi(x, y)$$

When $c(x,y) = \|x - y\|^2$, this gives the squared **Wasserstein-2 distance** — a geometrically meaningful metric on the space of distributions.

## Why OT for Machine Learning?

Unlike KL divergence, the Wasserstein distance respects the geometry of the underlying space. This makes it especially useful when:

- Distributions have **non-overlapping support** (where KL divergence is infinite)
- You need **gradient flow** through distribution comparisons (e.g., GANs, VAEs)
- The task involves **matching** samples across domains

## Entropic Regularization and the Sinkhorn Algorithm

Exact OT is computationally expensive ($O(n^3)$ for $n$ samples). The standard remedy is to add an entropy regularization term:

$$W_\varepsilon(\mu, \nu) = \inf_{\pi \in \Pi(\mu, \nu)} \left[ \int c \, d\pi + \varepsilon \, \text{KL}(\pi \| \mu \otimes \nu) \right]$$

This regularized problem has a unique solution with a nice structure: the optimal $\pi$ is a **Gibbs kernel** that can be computed via the **Sinkhorn algorithm** — an iterative matrix scaling that converges in $O(n^2 / \varepsilon)$ time.

## Application: Preference Alignment Auditing

In my work on [LLM preference alignment](/post/), OT plays a central role. We use it to transport difference embeddings between a labeled (positive) pool and an unlabeled pool, uncovering misaligned human preference labels in large-scale RLHF datasets.

The transport plan $\pi$ here captures how "positive" evidence from labeled pairs can be attributed to unlabeled pairs — a novel use of OT in the NLP evaluation pipeline.

## Key Libraries

- **POT** (Python Optimal Transport): full-featured, supports Sinkhorn and exact solvers
- **`ot.emd`**: exact transport via network simplex
- **`ot.sinkhorn`**: regularized transport, GPU-friendly

## Further Reading

- Villani, *Optimal Transport: Old and New* (2009) — the canonical reference
- Peyré & Cuturi, *Computational Optimal Transport* (2019) — practitioner-focused, free online
