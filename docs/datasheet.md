# Datasheet for the Black-Box Optimisation (BBO) Capstone Dataset

## Motivation

This dataset was created as part of the **Black-Box Optimisation (BBO) Capstone Project** to investigate how optimisation algorithms can efficiently locate high-performing solutions when the objective function is unknown and expensive to evaluate.

The dataset supports research and experimentation in:

- Bayesian Optimisation
- Gaussian Process surrogate modelling
- Acquisition function optimisation (UCB and EI)
- Support Vector Machine (SVM) classification of promising regions
- Neural Network Deep Ensemble surrogate models
- Sequential decision-making under limited evaluation budgets

The overall objective is to maximise eight hidden functions that simulate realistic optimisation problems across multiple industries while learning efficiently from a limited number of expensive evaluations.

---

## Composition

### Function Overview

| Function | Dimension | Real-World Scenario | Output Represents |
|----------|-----------|---------------------|-------------------|
| Function 1 | 2D | Detect contamination sources in a radiation field | Contamination reading |
| Function 2 | 2D | Maximise ML model log-likelihood performance | ML score |
| Function 3 | 3D | Drug discovery with side-effect minimisation | Side-effect score |
| Function 4 | 4D | Warehouse product placement optimisation | Performance score |
| Function 5 | 4D | Chemical process yield optimisation | Chemical yield |
| Function 6 | 5D | Recipe optimisation balancing quality, cost and waste | Recipe penalty score |
| Function 7 | 6D | Machine learning hyperparameter tuning | ML performance score |
| Function 8 | 8D | High-dimensional ML system optimisation | ML score |

### Initial Dataset Size

| Function | Initial Samples |
|----------|-----------------|
| Function 1 | 10 |
| Function 2 | 10 |
| Function 3 | 15 |
| Function 4 | 30 |
| Function 5 | 20 |
| Function 6 | 20 |
| Function 7 | 30 |
| Function 8 | 40 |

Each function returns a single scalar objective value that is treated as a **maximisation problem** throughout the project.

---

## Nature of the Data

The initial dataset consists of an input matrix **X** and corresponding scalar output **y**.

- **Input:** N × D numerical array, where **D** represents the dimensionality of the function.
- **Output:** N × 1 scalar objective values.

Each week, one additional query is submitted for every function, causing the dataset to grow sequentially and allowing surrogate models to improve as more observations become available.

Some functions exhibited noisy behaviour, particularly **Function 1**, where aggressive noise filtering was applied after identifying unreliable observations. Other functions appeared relatively smooth, while several higher-dimensional functions displayed evidence of local optima and multimodal behaviour based on surrogate predictions and optimisation performance.

---

## Collection Process

The dataset was collected over **thirteen optimisation rounds** using sequential Bayesian Optimisation.

Stragtegies for query generation evolved throughout the project and included:

- Gaussian Process (RBF and Automatic Relevance Determination (ARD) kernels)
- Upper Confidence Bound (UCB)
- Expected Improvement (EI)
- Manual heuristic point selection
- Continuous optimisation of acquisition functions
- SVM classification of promising regions
- Neural Network Deep Ensemble surrogate models

Each week's query selection was informed by all previous observations, making the optimisation progressively more informed while balancing exploration and exploitation.

---

## Data Handling and Preprocessing

For functions with extremely small output magnitudes, output standardisation was applied before processing.

This improved numerical stability during surrogate model training without changing the optimisation objective.

Additional preprocessing included:

- Feature scaling where appropriate
- Noise filtering for noisy functions
- Hyperparameter tuning of:
  - RBF length scale
  - ARD kernel parameters
  - UCB β (κ)
  - Observation noise assumptions
- Continuous optimisation of acquisition functions instead of predefined search grids
- Evaluation of Neural Network surrogate prediction accuracy before deployment

### Intended Uses

- Bayesian Optimisation research
- Sequential optimisation benchmarking
- Surrogate model comparison
- Exploration–exploitation studies
- Hyperparameter optimisation research

### Inappropriate Uses

This dataset should **not** be used for:

- Supervised learning benchmarks
- Safety-critical decision making
- Generalising properties of unknown optimisation landscapes beyond this project

---

## Weekly Iteration and Learning

The optimisation strategy evolved from a single Gaussian Process framework into function-specific optimisation pipelines.

Early rounds focused on understanding whether each function appeared **unimodal** or **multimodal**. As more observations accumulated, strategies became increasingly specialised.

Key lessons included:

- Different functions required different exploration–exploitation balances.
- Hybrid **Neural Network Deep Ensemble + UCB** models produced the strongest performance on several higher-dimensional problems.
- **SVM classification** effectively narrowed the search to promising regions.
- Manual reasoning and visual inspection remained valuable for low-dimensional noisy problems.
- Hyperparameter tuning became increasingly important as surrogate confidence improved.

---

## Performance and Results

Performance was evaluated using the objective value returned after each weekly submission.

Across thirteen optimisation rounds:

- **Gaussian Process + UCB** consistently performed well on low-dimensional and relatively smooth functions.
- **Neural Network Deep Ensemble + UCB** produced the strongest results for higher-dimensional functions (Functions 4, 6 and 8).
- **SVM-assisted Bayesian Optimisation** significantly improved Function 7.
- Continuous tuning of kernel length scale, ARD, acquisition parameters and noise assumptions progressively improved optimisation performance.

Confidence in the final solutions was highest where repeated exploitative queries consistently produced stable improvements and surrogate uncertainty had substantially reduced.

---

## Ethical, Practical and General Considerations

This dataset reflects realistic optimisation challenges encountered in engineering, manufacturing, healthcare and machine learning, where experiments are expensive, uncertain and sequential.

Because the underlying objective functions are **synthetic and hidden**, conclusions should not be directly generalised to real-world systems. However, the project provides practical insights into surrogate modelling, uncertainty-aware optimisation and adaptive decision-making.

The optimisation strategies developed are applicable to many industrial optimisation problems, although larger-scale applications may require more scalable surrogate models, distributed optimisation and enhanced uncertainty estimation.

Future users should recognise that optimisation performance depends strongly on surrogate model assumptions, preprocessing decisions, hyperparameter tuning and maintaining an appropriate balance between exploration and exploitation.

