# Model Card: Bayesian Optimisation Capstone Approach

## Overview

**Model name:** Iterative Gaussian-Process–Based Bayesian Optimisation  
**Model type:** Surrogate-based black-box optimisation (Gaussian Process + acquisition functions)  
**Version:** v1.0 (Ten-round capstone submission)

This project implements an iterative Bayesian Optimisation (BO) framework using Gaussian Process (GP) surrogate models combined with principled acquisition functions. The approach is human-in-the-loop: decisions about kernels, acquisition strategies, and exploration–exploitation balance are informed by observed behaviour across rounds rather than being fully automated.

## Intended Use

This approach is suitable for:

* Optimising expensive or slow-to-evaluate black-box functions.
* Problems with small evaluation budgets (tens of queries).
* Educational or research settings where understanding optimisation behaviour matters as much as final performance.
* Comparative studies of acquisition strategies, kernels, and optimisation dynamics.

Use cases that should be avoided include:

* Large-scale optimisation with thousands of evaluations.
* High-dimensional problems (>10–15 dimensions) without additional structure or dimensionality reduction.
* Fully automated production systems where manual intervention is not acceptable.
* Scenarios requiring guaranteed global optimality.

## Details

Across ten rounds of optimisation, the approach evolved from broad exploration toward increasingly selective exploitation, guided by patterns observed in each function.

Early rounds prioritised exploration to build a coarse understanding of the search spaces. High-exploration strategies such as UCB with elevated κ values were used for sparse or poorly understood landscapes (e.g. Function 1). As data accumulated, the strategy became more differentiated by function type. Noisy, multi-modal problems (Functions 2, 3, 4, 7, and 8) relied primarily on Noisy Expected Improvement (NEI) with ARD-enabled Matérn kernels, allowing uncertainty-aware local refinement while retaining some global search capability.

Midway through the project, the strategy incorporated deliberate interventions when failure modes emerged. These included switching acquisition functions, increasing exploration parameters, injecting Sobol sequence points to reset coverage, and adjusting noise assumptions when models became overconfident or stuck. Later rounds reflected tighter budgets and clearer hypotheses: for example, Function 1 switched entirely to Sobol sampling when no signal had been detected, while Function 5 shifted decisively toward Expected Improvement to exploit the best-known region.

Throughout, kernel choice (Matérn 3/2 vs 5/2), ARD usage, and inner-loop optimisation settings (candidates, restarts, Monte Carlo samples) were adapted based on dimensionality, noise, and observed behaviour rather than fixed in advance.

## Performance

Performance was evaluated function-by-function rather than via a single aggregate metric. The primary metric was the **best observed objective value** over time, tracked across rounds. Secondary indicators included:

* consistency of improvement,
* ability to escape local optima,
* stability of optimisation behaviour,
* and qualitative alignment with expected function structure.

Across the eight functions:

* Low-dimensional and moderately structured problems showed steady improvements.
* Noisy and high-dimensional problems demonstrated ridge-following behaviour with occasional jumps.
* Sparse and deceptive landscapes highlighted the limits of GP-based BO under tight budgets.
* In later rounds, performance gains diminished, reflecting budget saturation rather than optimiser failure.

The results are best interpreted comparatively and diagnostically rather than as claims of optimality.

## Assumptions and Limitations

The approach assumes that:

* The objective functions are reasonably smooth or locally smoothable by a GP surrogate.
* Past observations are informative about nearby regions.
* Qualitative assumptions about function structure (e.g. sparsity, unimodality) are broadly correct.

Key limitations include:

* Susceptibility to local trapping and overconfidence with small data sets.
* Weakly identified ARD lengthscales in higher dimensions.
* Diminishing returns from increased inner-loop optimisation when data is sparse.
* Dependence on manual judgment to correct failure modes.

These constraints limit scalability and generalisability without further automation or model changes.

## Ethical Considerations

Transparency is central to this project. All decisions, configurations, and deviations from default behaviour are documented alongside the data. This supports reproducibility, critical review, and adaptation to real-world settings where assumptions must be scrutinised.

By explicitly logging strategy changes and acknowledging limitations, the project avoids overstating performance or masking uncertainty. This is particularly important when translating optimisation techniques to applied domains—such as engineering or policy—where hidden assumptions can lead to misleading conclusions.

