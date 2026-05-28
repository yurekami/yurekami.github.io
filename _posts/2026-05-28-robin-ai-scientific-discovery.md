---
layout: post
title: "Robin: The First AI System to Discover a Drug Candidate in a Lab-in-the-Loop"
slug: robin-ai-scientific-discovery
date: 2026-05-28
description: A reading of the Nature paper introducing Robin, FutureHouse's multi-agent system that autonomously generated hypotheses, proposed experiments, analyzed data, and identified ripasudil as a novel therapeutic candidate for dry age-related macular degeneration — the first AI-driven drug repurposing through an iterative lab-in-the-loop cycle.
tags: [ai, biology, drug-discovery, multi-agent-systems]
categories: [research]
giscus_comments: true
related_posts: true
toc:
  sidebar: left
---

A system called Robin just did something no AI has done before: it proposed a disease mechanism, suggested drugs to test, analyzed the experimental results, proposed follow-up experiments, analyzed _those_ results, and identified a clinically approved drug as a novel therapeutic candidate for a major disease — all with the intellectual steps automated by language model agents. The results are published in Nature.[^paper]

[^paper]: Ali Essam Ghareeb, Benjamin Chang, Ludovico Mitchener, Angela Yiu, et al., "A multi-agent system for automating scientific discovery," _Nature_ (2026). [doi:10.1038/s41586-026-10652-y](https://doi.org/10.1038/s41586-026-10652-y). Accelerated Article Preview. FutureHouse, San Francisco.

The drug is **ripasudil**, a Rho kinase (ROCK) inhibitor already approved in Japan for glaucoma. The disease is **dry age-related macular degeneration (dAMD)** — the leading cause of irreversible blindness in the developed world, with 1.5 million Americans having vision-threatening dAMD and a figure projected to nearly triple by 2050. There are currently limited treatment options.

Robin is not a single model. It is a **multi-agent architecture** built from three specialized agents: Crow and Falcon (literature search), and Finch (experimental data analysis). The system synthesized 825 scientific papers in 30 minutes — an estimated 540 hours of human reading — and reduced the total cognitive labor of a discovery cycle from an estimated 872–937 human hours to under two hours, at a cost of roughly $11.

## The Discovery Cycle

The paper walks through two complete rounds of a lab-in-the-loop cycle:

### Round 1: Hypothesis generation and initial screen

Given only the prompt "dry age-related macular degeneration," Robin:

1. **Identified disease mechanisms.** Called Crow to review 151 papers and proposed 10 biologically relevant dAMD mechanisms to assay. After ranking, Robin selected **enhancing RPE (retinal pigment epithelium) phagocytosis** as the top strategy.

2. **Selected an assay.** Proposed testing how well drugs increase the phagocytic capacity of RPE cells using flow cytometry. (The human scientists substituted pHrodo beads for the photoreceptor outer segments Robin suggested, and used ARPE-19 cells instead of primary/stem cell-derived RPE, for expediency.)

3. **Proposed drug candidates.** Reviewed ~400 papers and proposed 30 drug candidates for the phagocytosis assay. Falcon generated detailed evaluation reports on each candidate. An LLM-judged tournament ranked them.

4. **Top 5 tested.** Scientists tested: Exendin-4, Fingolimod, MFGE8, Y-27632, and AICAR+TUDCA. MFGE8 (known to enhance RPE phagocytosis) served as a positive control.

5. **Autonomous data analysis.** Raw flow cytometry data was uploaded to Robin. Finch launched 8 independent analysis trajectories, each writing its own Jupyter notebook to gate the flow cytometry data and run statistical tests. A meta-analysis synthesized the trajectories into consensus conclusions. **Y-27632**, a ROCK inhibitor, was the top hit — consistent with preclinical literature Robin had identified.

### Round 1.5: Follow-up experiment

Robin recommended RNA sequencing of Y-27632-treated RPE cells. Scientists ran the experiment. Finch performed differential gene expression analysis and GO enrichment analysis, revealing:

- Rapid transcriptional changes in actin filament organization, small GTPase signaling, and autophagy
- A **3-fold upregulation of ABCA1** (adjusted $$p = 2.13 \times 10^{-83}$$), a critical lipid efflux pump linked to AMD genetic susceptibility via ApoE

This ABCA1 finding was a novel mechanistic insight: ROCK inhibition doesn't just enhance phagocytosis through cytoskeletal rearrangement — it also upregulates a lipid transport gene implicated in AMD pathology.

### Round 2: Iterative refinement

Robin generated a second round of drug candidates informed by the Round 1 results. Scientists tested 10 candidates. Finch's analysis identified **ripasudil** — a ROCK inhibitor already approved for ocular use in Japan — as outperforming Y-27632, with a 1.89-fold increase in RPE phagocytosis vs. controls.

### Validation in primary human RPE

The hits were re-screened using primary RPE stem cells (RPE-SC) from a >60-year-old patient, with fluorescently labeled bovine rod outer segments as a more physiologically relevant substrate. Ripasudil and Y-27632 were again hits, with ripasudil showing higher potency. Dose-response confirmed ripasudil's superiority. Surprisingly, ripasudil showed a _negative_ relationship with cytotoxicity — higher doses _reduced_ LDH release.

An additional hit emerged: **KL001**, a circadian clock modulator. Robin had suggested it based on the circadian control of RPE phagocytosis. To the authors' knowledge, KL001 has never previously been proposed as an enhancer of phagocytosis.

RNA-seq on ripasudil-treated RPE-SC confirmed the ABCA1 upregulation finding from the cell line experiments.

## The Architecture

Robin coordinates three agents:

- **Crow**: A literature search agent based on PaperQA2, performing concise literature summaries. Used for initial disease mechanism identification and quick literature queries.
- **Falcon**: A deep literature search agent, also PaperQA2-based, generating comprehensive evaluation reports on individual drug candidates. Access to scientific literature, clinical trial reports, and the Open Targets Platform.
- **Finch**: A scientific data analysis agent that executes analysis code in Jupyter notebooks. Given raw data (flow cytometry .FCS files, RNA-seq gene counts), Finch independently develops analysis pipelines. 8 trajectories run in parallel, with a meta-analysis for consensus.

The workflow is orchestrated by a coordinator that passes information between agents. The tournament ranking uses an LLM judge (Claude Sonnet 3.7 in the ablation studies) for pairwise comparisons, with Bradley-Terry-Luce (BTL) scoring.

## Ablation Studies

The paper includes careful ablations:

- **Removing Falcon** (or both Crow and Falcon): dramatic increase in hallucinated references. Without Falcon, o4-mini produced 44.5% hallucinated references; wild-type Robin produced zero.
- **Removing Crow alone**: minimal impact on reference quality (Falcon corrects for Crow's errors), but reduced quality of final drug proposals.
- **Deep Research comparison**: OpenAI's Deep Research was given the same prompt and asked to generate 19 drug candidates. None were hits in the RPE-SC phagocytosis assay. Deep Research did not suggest ROCK inhibition as a strategy. This confirms Robin's discovery was non-trivial.

Finch's performance: 100% adherence to expert rubrics on flow cytometry analysis, 86% on RNA-seq. On a harder benchmark (170 question-answer pairs from BixBench), Finch scored 22.8% vs. 1.6% for the base LLM alone — useful but with substantial room for improvement, especially on multi-step bioinformatics pipelines.

## What Makes This Different

Previous AI systems for scientific discovery (Coscientist, TxGemma, AI Co-Scientist) have automated _parts_ of the discovery process — hypothesis generation, or property prediction, or safety profiling. Robin is the first to automate all key intellectual steps in a continuous cycle:

1. Literature review → 2. Hypothesis generation → 3. Experiment proposal → 4. Data analysis → 5. Hypothesis refinement → repeat

The human role is narrowed to executing experiments (following Robin's proposed assays) and making go/no-go decisions on which candidates to test. All hypotheses, experimental directions, data analyses, and data figures in the main text were produced by Robin.

The practical impact: Robin analyzed 551 papers in 30 minutes. The entire discovery cycle cost ~$11 in LLM API calls. A human team would need an estimated 872–937 hours. The 200-fold reduction in cognitive labor, if it generalizes, changes the economics of early-stage drug discovery.

## Limitations and Caveats

The paper is honest about what Robin cannot do:

- **No executable protocols.** Robin generates experimental outlines, not step-by-step protocols. Human scientists translate these into lab procedures.
- **Finch requires prompt engineering.** The data analysis agent needs task-specific prompting from domain experts. It cannot yet independently decide what analysis to run.
- **In vitro only.** Ripasudil's efficacy for dAMD has been shown only in cell culture. In vivo validation and clinical trials are necessary.
- **Stochastic analysis.** Finch's conclusions vary between runs. The 8-trajectory consensus mechanism mitigates this, but it's a feature of the architecture, not a guarantee.
- **No guarantee of novelty.** While ROCK inhibitors had never been proposed for _dry_ AMD via phagocytosis enhancement, ROCK inhibitors are known to enhance RPE phagocytosis, and ripasudil is a known ROCK inhibitor. The insight is the _combination_ — connecting these known facts to a specific disease indication — which is exactly what drug repurposing requires.

## What It Means

The most important sentence in the paper may be this one: "By focusing on 'combinatorial synthesis' (identifying non-obvious connections between disparate fields), Robin effectively targets 'low-hanging fruit' that human experts may overlook due to the compartmentalization of scientific knowledge."

This is a precise characterization of what LLM systems are good at: breadth over depth, connection over creation. Robin didn't discover a new drug mechanism. It connected an existing mechanism (ROCK inhibition enhances RPE phagocytosis) to an unmet clinical need (dry AMD) by reading 825 papers that no single human researcher would have synthesized. The result — ripasudil as a dAMD candidate — was logically deducible from the existing literature. Robin deduced it.

The drug repurposing examples in the introduction set the scale of the problem: dabrafenib took 10 years to connect known BRAF inhibition to otoprotection. Ketamine took 22 years from the pharmacology to the antidepressant application. If Robin-like systems can systematically collapse these lag times, the impact on human health would be substantial.

Whether ripasudil works for dAMD is still unknown. But the question "can an AI system autonomously identify a clinically plausible drug repurposing candidate through iterative hypothesis generation and experimental validation?" now has an answer. The answer is yes.

---

_The paper is available as an Accelerated Article Preview at [doi:10.1038/s41586-026-10652-y](https://doi.org/10.1038/s41586-026-10652-y). FutureHouse's tools — PaperQA2 (Crow/Falcon) and Finch — are described in [Skarlinski et al. (2024)](https://arxiv.org/abs/2312.07559) and [Laurent et al. (2025)](https://arxiv.org/abs/2506.xxxxx)._
