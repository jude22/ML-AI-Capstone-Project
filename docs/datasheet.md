# Datasheet for the Black-Box Optimisation (BBO) Capstone Dataset

## 1. Motivation

This dataset was created as part of a Black-Box Optimisation (BBO) capstone project designed to investigate optimisation strategies for unknown functions under limited evaluation budgets. The project simulates real-world optimisation problems where the internal structure of the objective function is unavailable and each function evaluation is expensive.

### Supported Tasks

The dataset supports the development, evaluation and comparison of optimisation methods including:

* Bayesian Optimisation
* Gaussian Process surrogate modelling
* Acquisition function optimisation (UCB and EI)
* Support Vector Machine (SVM) classification of promising regions
* Neural Network Deep Ensemble surrogate models

The objective is to identify input configurations that maximise the output of eight unknown functions with increasing dimensionality.

---

## 2. Composition

The dataset consists of sequential optimisation observations collected across multiple weekly rounds.

Each observation contains:

* Function identifier (Function 1–8)
* Input vector
* Output value returned by the hidden function

### Data Dimensions and Size

| Function   | Input Dimensions | Initial Dataset Size | Output |
| ---------- | ---------------- | -------------------- | ------ |
| Function 1 | 2D               | 10                   | Scalar |
| Function 2 | 2D               | 10                   | Scalar |
| Function 3 | 3D               | 15                   | Scalar |
| Function 4 | 4D               | 30                   | Scalar |
| Function 5 | 4D               | 20                   | Scalar |
| Function 6 | 5D               | 20                   | Scalar |
| Function 7 | 6D               | 30                   | Scalar |
| Function 8 | 8D               | 40                   | Scalar |


### Known Gaps

* The true functions are unknown.
* Sampling is not uniformly distributed across the search space.
* Later rounds contain more exploitative samples than exploratory samples.
* Some regions remain unexplored.

---

## 3. Collection Process

Queries were generated using an iterative optimisation workflow. Each week's query selection was informed by all previous observations and outcomes.

Strategies evolved over time and included:

* Gaussian Process Bayesian Optimisation
* UCB acquisition function
* Expected Improvement (EI)
* Manual expert-guided exploration
* SVM-based promising-region classification
* Neural Network Deep Ensemble surrogates

### Assumptions

The collection process assumes:

* Nearby points may contain improved solutions.
* Historical observations provide useful information for future searches.
* Surrogate models can approximate the hidden objective function.

---

## 4. Preprocessing and Intended Uses

### Preprocessing Applied

The following preprocessing and modelling decisions were used:

* Feature scaling where required by optimisation models
* Noise filtering in selected functions
* Continuous optimisation of acquisition functions
* Hyperparameter tuning of RBF length scales and UCB parameters

### Intended Uses

Appropriate uses include:

* Research into Bayesian Optimisation
* Surrogate model evaluation
* Exploration–exploitation studies
* Hyperparameter optimisation research
* Sequential decision-making experiments

### Inappropriate Uses

This dataset should not be used:

* As a benchmark for supervised prediction tasks
* To estimate processes outside this Capstone project
* To make safety-critical decisions
* To claim generalisable properties of unknown optimisation landscapes

---

## 5. Distribution and Maintenance

The dataset is provided and maintained by Imperial Executive Education Programme.

### Terms of Use

The dataset is intended for educational, research and reproducibility purposes within the context of the capstone project.

Users should acknowledge that the underlying functions remain unknown and that observations are generated through sequential optimisation rather than random sampling.

This documentation supports responsible and transparent use of the dataset throughout the lifecycle of the project.
