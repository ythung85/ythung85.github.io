---
layout: post-single
title: "PU Learning: When You Only Have Positives"
date: 2025-01-10
tags: [stat, ml]
excerpt: "An intuitive guide to Positive-Unlabeled learning — what it is, when to use it, and how it connects to classical binary classification."
permalink: /post/pu-learning/
---

## The Problem Setup

In classical binary classification, you have labeled positive and negative examples. But what if you only have **positives** and a pool of **unlabeled** data — where some unlabeled points are actually positives but you don't know which?

This is **Positive-Unlabeled (PU) learning**, and it comes up more often than you might think:

- Identifying fraudulent transactions (confirmed fraud vs. unknown)
- Drug discovery (known active compounds vs. untested molecules)
- Preference learning from implicit feedback (clicked vs. not-clicked-but-maybe-relevant)

## The Key Insight: Risk Reformulation

Let $p(x)$ be the positive class density and $u(x)$ the unlabeled density. The unlabeled set is a mixture:

$$u(x) = \pi \cdot p(x) + (1 - \pi) \cdot n(x)$$

where $\pi = P(Y=1)$ is the class prior and $n(x)$ is the negative density.

The standard binary risk can be rewritten as:

$$R(f) = \pi \, \mathbb{E}_p[\ell(f(x), +1)] + (1-\pi) \, \mathbb{E}_n[\ell(f(x), -1)]$$

Since we can estimate $\mathbb{E}_n[\ell]$ from the unlabeled set (after subtracting the positive contribution), PU learning becomes tractable without ever observing negative labels directly.

## Non-Negative Risk Estimator

A practical challenge: the estimated risk can go negative during training, causing overfitting. The **nnPU estimator** (Kiryo et al., 2017) clips the negative part:

$$\hat{R}_{nnPU}(f) = \pi \, \hat{\mathbb{E}}_p[\ell_+] + \max\!\left(0,\, \hat{\mathbb{E}}_u[\ell_-] - \pi \, \hat{\mathbb{E}}_p[\ell_-]\right)$$

This simple fix dramatically stabilizes training and is now the default in most PU implementations.

## Connection to My Research

In our work on [LLM preference auditing](/post/optimal-transport/), we model the RLHF annotation process as a PU problem: gold-standard pairwise comparisons form the positive pool, and a large collection of model-generated preferences forms the unlabeled pool. OT then bridges the two, yielding a principled confidence score for each unlabeled preference annotation.

## When to Use PU Learning

PU learning is the right tool when:
1. Collecting negative labels is **expensive or impossible**
2. Your unlabeled data is a realistic mixture of both classes
3. You have at least a rough estimate of the class prior $\pi$

A common pitfall: treating unlabeled as negative. This introduces systematic label noise that biases the classifier toward over-predicting negatives. PU methods avoid this by design.

## Libraries

- **`pulearn`** (Python): sklearn-compatible PU classifiers
- **`pu-learning`** on GitHub: nnPU and unbiased PU implementations in PyTorch
