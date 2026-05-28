---
layout: post
title: "Epicure: What 4 Million Recipes Reveal About the Hidden Geometry of Flavor"
date: 2026-05-27
categories: research
tags: computational-modeling embeddings
description: A paper dissection of Radzikowski & Chen's Epicure, which trains skip-gram embeddings on a multilingual recipe corpus and discovers that centuries of culinary tradition have been silently encoding molecular chemistry.
toc:
  beginning: true
giscus_comments: true
related_posts: false
---

What if grandma's cookbook was doing chemistry all along?

That is the provocation at the heart of **Epicure** (Radzikowski & Chen, 2026), a paper that trains three flavors of ingredient embeddings on 4.14 million recipes spanning seven languages --- and discovers that the geometric structure of culinary co-occurrence mirrors the geometric structure of molecular flavor compounds. Recipes, it turns out, are implicit experiments. Embeddings make the results legible.

## The Setup: From Raw Recipes to a Canonical Ingredient Space

The first contribution is unglamorous but load-bearing: building a clean corpus. Eleven data sources --- Recipe1M+, FooDB, FlavorDB, Xiachufang (Chinese), ChefKoch (German), SomosNLP Recetas (Spanish), and several South/Southeast Asian collections --- are funneled through an LLM-augmented standardization pipeline that maps the wild diversity of ingredient names ("cilantro" / "coriander" / "koriander" / "xiang cai") down to **1,790 canonical entries**. Without this step, the embeddings would learn that German and English cuisines share nothing, which is wrong in precisely the interesting ways the paper wants to explore.

## Three Embeddings, One Spectrum

Epicure does not train a single model. It trains three **Metapath2Vec** variants that differ in their random-walk schemas over a heterogeneous ingredient graph:

| Model    | Walk Schema                                     | What It Captures                              |
| -------- | ----------------------------------------------- | --------------------------------------------- |
| **Cooc** | Traverses recipe co-occurrence edges only       | Culinary tradition --- what _is_ paired       |
| **Chem** | Traverses FlavorDB chemical compound edges only | Molecular affinity --- what _could_ be paired |
| **Core** | Balanced traversal across both edge types       | The intersection of tradition and chemistry   |

This is the paper's central conceptual device: a **chemistry-vs-recipe-context spectrum**. Cooc lives at the cultural end (soy sauce near dashi, because Japanese cuisine pairs them). Chem lives at the molecular end (soy sauce near parmesan, because both are rich in glutamates). Core occupies the middle ground.

The distinction matters because it disentangles two kinds of knowledge that are normally confounded:

- **Empirical knowledge**: "These ingredients work together" (recipes)
- **Mechanistic knowledge**: "These ingredients share volatile compounds" (chemistry)

When the two agree, you have a well-understood pairing. When they disagree --- when Chem says two ingredients should work but Cooc says nobody has tried --- you have a **discovery opportunity**.

## What the Geometry Reveals

The most striking results come from nearest-neighbor analysis and UMAP projections across the three embedding spaces.

**Cooc clusters by cuisine.** Spices group by geographic tradition: cumin-coriander-turmeric (South Asian), oregano-basil-thyme (Mediterranean), star anise-five spice-sesame oil (East Asian). This is expected but validates the embeddings.

**Chem clusters by molecular family.** Here things get interesting. Umami-rich ingredients --- soy sauce, miso, parmesan, tomato paste --- cluster together regardless of cuisine. Acidic ingredients (citrus, vinegar, yogurt) form their own neighborhood. The embedding has rediscovered flavor chemistry from molecular structure alone.

**Core reveals cross-cultural bridges.** The combined embedding surfaces ingredients that are close in chemistry _and_ appear in overlapping culinary contexts, but across different traditions. These are the "why does this work?" moments that molecular gastronomy lives for.

The paper also applies **WEAT** (Word Embedding Association Tests) adapted for food, probing whether the embeddings encode cuisine stereotypes or category biases. They find that embeddings do reflect training-data imbalances --- English-language recipe dominance distorts representation of non-Western cuisines --- which is honest and useful to report.

## The Deeper Point: Recipes as Distributed Experiments

The result I find most thought-provoking is not any single cluster or nearest-neighbor list. It is the meta-observation: **culinary traditions encode chemical knowledge empirically.**

Nobody in 14th-century Sichuan was measuring glutamate concentrations. Yet Sichuan cuisine converged on ingredient combinations that maximize umami synergy, because the feedback loop --- cook, taste, iterate over generations --- is a distributed optimization process. The Cooc embedding is, in a sense, the fixed point of that process. That it aligns with the Chem embedding is evidence that cultural selection pressure on recipes is real and chemically grounded.

This connects to a broader principle in computational modeling: **emergent geometry is evidence of latent structure.** When you embed objects in a vector space and meaningful clusters appear, those clusters are not artifacts of the method --- they reflect genuine statistical regularities in the data-generating process. The ingredient embeddings are meaningful because recipes are not random.

## Limitations Worth Noting

The paper is candid about its gaps:

- **Language imbalance.** English sources dominate the corpus. Cuisines documented primarily in non-English languages (West African, Central Asian) are underrepresented, and the embeddings inherit this bias.
- **FlavorDB coverage.** The chemical compound graph is incomplete for many non-Western ingredients, which means the Chem embedding has blind spots precisely where discovery would be most valuable.
- **No temporal dynamics.** Cuisines evolve. Ingredients that were exotic become staples (chili peppers in Thai cuisine post-Columbian exchange). The static embedding captures a snapshot, not a trajectory.

## Why This Paper Matters Beyond Food

Epicure is a food science paper, but the methodology generalizes. Whenever you have two independent sources of similarity --- one empirical (co-occurrence in practice) and one mechanistic (shared structural properties) --- you can construct a spectrum of embeddings and ask where the two agree, where they diverge, and what the divergences mean.

Drug repurposing. Materials science. Music composition. Any domain where tradition has accumulated implicit knowledge that mechanistic models can now make explicit.

The recipe corpus is the proof of concept. The geometry is the point.

---

**Paper:** Radzikowski, J. & Chen, J. (2026). _Epicure: Navigating the Emergent Geometry of Food Ingredient Embeddings._ [arXiv:2605.22391](https://arxiv.org/abs/2605.22391)
