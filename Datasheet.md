# Dataset Datasheet: Bayesian Optimisation Capstone Experiments

## Motivation

This dataset was created to support a practical exploration of **Bayesian Optimisation (BO)** under tight evaluation budgets. The primary goal is to study how different surrogate models, acquisition functions, and exploration–exploitation strategies behave across a range of synthetic black-box functions with varying dimensionality, noise levels, and structural properties.

The dataset supports tasks such as:

* analysing optimisation trajectories over time,
* comparing acquisition strategies under limited data,
* diagnosing failure modes such as local trapping or overconfidence,
* and reflecting on decision-making under uncertainty.

More broadly, it is intended as a learning and research artefact illustrating how optimisation strategies evolve when interacting iteratively with unknown systems.

## Composition

The dataset consists of **query–response histories** for eight benchmark black-box functions, collected over ten iterative optimisation rounds.

For each function, the dataset includes:

* input vectors `X` (ranging from 2D to 8D),
* corresponding scalar outputs `y`,
* and metadata recording the order and context in which queries were made.

The data is stored in NumPy array format (`.npy`) for numerical efficiency, alongside JSON Lines files (`.jsonl`) that log submissions and decisions chronologically. Each function has its own directory containing:

* initial inputs and outputs provided at the start of the task,
* accumulated optimisation history,
* and a submission log capturing weekly queries.

The total dataset size is intentionally small: each function contains approximately 16–20 evaluated points, reflecting the constrained-budget setting. Gaps are inherent to the task, with large regions of the input space remaining unexplored, especially in higher-dimensional functions.

## Collection Process

The data was collected iteratively through a **human-in-the-loop Bayesian Optimisation process**. For each function, one new query was generated per round using a Gaussian Process surrogate model and a selected acquisition function (e.g. UCB, EI, or Noisy Expected Improvement).

Query generation strategies varied by function and evolved over time, informed by:

* observed outputs from previous rounds,
* qualitative assumptions about the underlying landscape (e.g. sparse, unimodal, noisy, multi-modal),
* and the remaining evaluation budget.

In some rounds, deliberate interventions were introduced—such as switching acquisition functions or injecting Sobol sequence points—to counteract local trapping or overconfidence. The data was collected over several weeks, with each round representing a discrete optimisation iteration rather than continuous sampling.

## Preprocessing and Uses

Minimal preprocessing was applied to the raw data. Inputs and outputs are stored in their native numerical form, with standardisation and normalisation applied **internally within the optimisation models**, not permanently to the dataset.

**Intended uses** include:

* educational analysis of Bayesian Optimisation behaviour,
* reproduction of optimisation trajectories,
* benchmarking alternative acquisition or surrogate strategies on the same historical data,
* reflective study of decision-making under uncertainty.

**Inappropriate uses** include:

* treating the dataset as a dense or representative sampling of the underlying functions,
* training machine learning models that assume independent and identically distributed data,
* claiming globally optimal solutions based on this dataset.

The dataset is not suitable for conventional supervised learning.

## Distribution and Maintenance

The dataset is distributed publicly via the project’s GitHub repository, alongside the accompanying optimisation code and documentation. It is provided for educational and research purposes.

There are no usage restrictions beyond standard open-source norms; however, users are expected to cite or acknowledge the project if the dataset is reused. The dataset is maintained by the project author and will remain stable unless future iterations of the capstone add additional rounds or functions, in which case versioning and updates will be clearly documented.

