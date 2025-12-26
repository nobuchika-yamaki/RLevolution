# RLevolution
Evolutionary Symmetry Breaking Model

This repository contains the full, executable implementation of the evolutionary model described in the Methods section (Sections 2.1–2.10) of the manuscript.
The code reproduces all reported results for Stage 1–Stage 3 without approximation or omitted steps.

The model investigates how hemispheric specialization can emerge, acquire polarity, and remain evolutionarily stable under minimal computational and developmental assumptions.

Overview

The system consists of two initially identical processing channels (L and R) evolving spatial integration scales under an anomaly-detection task.
Exact Z2 symmetry is guaranteed at initialization and in all task environments unless explicitly broken by a developmental gain asymmetry.

Key properties:

No built-in hemispheric differences

Exact left–right exchange symmetry when δ = 0

Evolutionary emergence of specialization via spontaneous symmetry breaking

Directional polarity induced by minimal developmental bias

Stability tested via reseeding under restored symmetry

Model Components
Processing architecture

Each channel is parameterized by:

a_L, a_R: spatial integration scales

λ: evolvable intrinsic bias

Processing consists of Gaussian spatial integration followed by absolute-difference mismatch computation.

Task environment

20 × 20 grid

Pixel values sampled from N(0, 1)

Global structure created by Gaussian smoothing (σ = 1.0)

Local anomaly: 3 × 3 patch with positive offset sampled from U[1.5, 2.0)

New environment generated independently for every trial

Decision and performance

Decision variable:

D = (m_R · M_R − m_L · M_L) + λ

Reaction time:

RT = 1 / (|D| + ε)

Fitness:

fitness = −mean(RT) over 8 trials

Developmental asymmetry

Developmental gain asymmetry is defined as:

δ = m_R − m_L
m_L = 1 − δ/2
m_R = 1 + δ/2

δ = 0 → exact developmental symmetry

δ > 0 → right-favoring bias

δ < 0 → left-favoring bias

The canonical value δ = 0.02 (2%) corresponds to a weak, biologically plausible asymmetry.

Evolutionary algorithm

Population size: 20

Selection: top 50% by fitness

Recombination: arithmetic mean of parents

Mutation: Gaussian noise (σ = 0.05) applied to all parameters

Constraint: a ≥ 0.1

Independent lineages with fixed random seeds

Experimental stages
Stage 1: Symmetric development

δ = 0

a_L = a_R = 1.0 at initialization

λ = 0

40 independent lineages

50 generations

Purpose: test spontaneous symmetry breaking without imposed asymmetry.

Stage 2: Biased development

δ = +0.02 (right-favoring)

20 independent lineages

50 generations

Purpose: test whether a minimal developmental bias fixes polarity.

Stage 2R: Reversed bias

δ = −0.02 (left-favoring)

20 independent lineages

50 generations

Purpose: verify that polarity tracks the sign of δ.

Stage 3: Stability under reseeding

Seeds: best individuals from Stage 2

Gaussian perturbation applied to (a_L, a_R, λ)

δ reset to 0

30 additional generations

Purpose: test evolutionary stability of the broken-symmetry state.

Output metrics

For each lineage, the following quantities are recorded:

a_L, a_R

Δa = a_R − a_L

|Δa|

λ

meanRT_final20 (mean RT over 20 evaluation trials)

Polarity, divergence, and reversals are defined exactly as in Methods 2.9.

Running the simulations

The full pipeline is contained in a single Python script.

Required dependencies:

Python ≥ 3.9

NumPy
SciPy

To run all stages:
stage1 = run_stage("Stage1", 40, 50, delta=0.0)
stage2 = run_stage("Stage2", 20, 50, delta=0.02)
stage2R = run_stage("Stage2R", 20, 50, delta=-0.02)
stage3 = run_stage3_from_stage2(stage2)
All random seeds are lineage-specific and fixed, ensuring full reproducibility.
