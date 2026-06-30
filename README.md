# Black-Box Optimisation (BBO) Project
### Imperial College ML/AI Certification 2026

## Section 1: Project Overview

This capstone project focuses on solving **eight unknown black-box optimisation problems** using iterative machine learning and optimisation techniques. Each function represents a maximisation task where only limited observations are initially available, and better solutions are discovered progressively through weekly query submissions and model updates.

The project simulates realistic ML and optimisation scenarios across different domains and dimensionalities:

| Function   | Dimension | Example Real-World Scenario                       |
| ---------- | --------- | ------------------------------------------------- |
| Function 1 | 2D        | Detect contamination sources in a radiation field |
| Function 2 | 2D        | Maximise ML log-likelihood performance            |
| Function 3 | 3D        | Drug discovery with side-effect minimisation      |
| Function 4 | 4D        | Warehouse product placement optimisation          |
| Function 5 | 4D        | Chemical process yield optimisation               |
| Function 6 | 5D        | Recipe optimisation balancing quality and cost    |
| Function 7 | 6D        | ML hyperparameter tuning                          |
| Function 8 | 8D        | High-dimensional ML system optimisation           |

### Project Goal

The high-level goal is to identify near-optimal solutions despite:

* Incomplete information
* Expensive evaluations
* Unknown response surfaces
* Possible local maxima

This project mirrors real-world ML workflows where optimisation must occur under uncertainty and limited computational budgets.

### Professional Relevance

The capstone directly supports my future career by developing practical skills in:

* Bayesian optimisation
* Surrogate modelling
* Hyperparameter tuning
* Uncertainty-aware decision making
* Adaptive experimentation

These skills can be applied to real-world problems such as:

* ML model tuning
* Recommendation systems
* Manufacturing optimisation
* Warehouse logistics
* Pharmaceutical research
* Simulation optimisation
* Resource allocation
* Engineering design
* Scientific experimentation

---

## Section 2: Inputs and Outputs

Initial inputs and outputs data are provided by Imperial College ML/AI programme facilitors for each function which could be noisy and very limited. Each query involves submission of predicted inputs to the unknown function which will return an output.

### Example Input Format

```text
Function 1: [0.838384, 0.767677]

Function 4:
[0.383229, 0.487977, 0.445785, 0.398437]

Function 8:
[0.157347, 0.141981, 0.190491, 0.237306,
 0.376975, 0.691764, 0.478713, 0.771636]
```

### Example Output Format

```text
Function 1: -6.893705068847273e-45

Function 4: -1.590305706105521

Function 8: 9.6902022465459
```

Each query returns a **scalar output value** representing the performance of the hidden function.

### Optimisation Objective

All tasks are framed as **maximisation problems**:

* Higher outputs indicate better solutions
* This remains true even when the original formulation involved negative scoring or minimisation concepts

The dataset grows weekly as new query results are collected and incorporated into the optimisation strategy.

---

## Section 3: Model (Approach and Selection)

This project applies **Bayesian Optimisation (BO)** to maximise eight unknown black-box functions (2D–8D) under a limited-query setting. Since each function evaluation is expensive and only one query is permitted per week, the objective is to identify high-performing inputs by efficiently learning from a small number of observations.

### Optimisation Models

Several complementary models were used throughout the project, with model selection depending on the dimensionality, complexity and observed behaviour of each function.

- **Gaussian Process (GP)** surrogate model with **Radial Basis Function (RBF)** kernel
- **Soft-margin Support Vector Machine (SVM)** for classifying promising regions of the search space
- **Neural Network Deep Ensemble** surrogate models for higher-dimensional functions

Gaussian Processes formed the core optimisation framework because they perform well with small datasets while providing predictive means and uncertainty estimates required for Bayesian Optimisation. Upper Confidence Bound (UCB) was selected as the primary acquisition function due to its explicit control of the exploration–exploitation trade-off through the β (kappa) parameter, while Expected Improvement (EI) was evaluated as an alternative during the early stages.

For higher-dimensional functions where Gaussian Processes became less expressive, Neural Network Deep Ensemble surrogates were adopted to better capture complex response surfaces while maintaining uncertainty-aware optimisation through UCB. SVM classification was incorporated to identify promising regions before acquisition optimisation, improving search efficiency by focusing evaluations where high-performing solutions were more likely.

Acquisition functions were optimised directly as continuous functions rather than over predefined candidate grids, avoiding the curse of dimensionality and improving computational efficiency.

### Model Evolution

The optimisation strategy evolved from a single GP-based framework into a function-specific optimisation pipeline.

Key developments across Weeks 2–13 included:

- Hyperparameter tuning of RBF length scale, observation noise and UCB β (kappa) to balance exploration and exploitation.
- Manual visualisation and heuristic query selection for the 2D problem (Function 1), where plotting helped identify promising regions after noisy observations.
- Adaptive exploration strategies for multimodal functions and progressively exploitative tuning once convergence behaviour became evident.
- Introduction of soft-margin SVM classification for Function 7 to guide Bayesian Optimisation towards promising regions.
- Addition of ARD to Function 2 to better model the contribution of individual input dimensions.
- Hybrid Bayesian Optimisation using Neural Network Deep Ensemble surrogates with UCB for Functions 4, 6 and 8 after sufficient data had accumulated.
- Continuous evaluation of surrogate prediction accuracy and refinement of optimisation strategies based on observed performance.

### Model Selection Justification

Rather than adopting a single optimisation method for all eight functions, the final strategy selected models according to the characteristics of each optimisation problem.

- **GP + UCB** remained the preferred solution for lower-dimensional problems because of its sample efficiency, uncertainty estimation and interpretability.
- **GP + ARD + UCB** improved modelling of feature importance for Function 2.
- **SVM + GP + UCB** improved optimisation efficiency for Function 7 by restricting search to promising regions.
- **Neural Network Deep Ensemble + UCB** produced the strongest results for Functions 4, 6 and 8, where higher dimensionality justified a more expressive surrogate model.
- Manual visualisation and heuristic guidance complemented Bayesian Optimisation for Function 1 when the search space could be inspected directly.

By the final submission (Week 13), most functions had either converged or identified highly promising regions.

## Section 4: Hyperparameter Optimisation

Hyperparameter optimisation played a central role throughout the project. Rather than using fixed settings, hyperparameters were iteratively refined after each weekly submission based on the observed optimisation outcomes. The objective was to improve the surrogate model's ability to identify promising regions while maintaining an appropriate balance between exploration and exploitation.

The main hyperparameters tuned were:

| Hyperparameter | Purpose | Optimisation Strategy |
|----------------|---------|-----------------------|
| **RBF Length Scale** | Controls how quickly the surrogate model assumes the objective function changes. | Increased to encourage broader exploration when local optima were suspected, and reduced to focus exploitation around promising regions. |
| **UCB β (Kappa)** | Controls the exploration–exploitation balance of the acquisition function. | Higher values promoted exploration during early rounds, while progressively lower values encouraged exploitation as confidence increased. |
| **Observation Noise (Alpha)** | Represents uncertainty in observations and influences GP fitting. | Adjusted when noisy outcomes were suspected, particularly for Function 1, to improve model robustness. |
| **ARD Length Scales** | Learns feature relevance independently for each input dimension. | Introduced for Function 2 to better model the contribution of individual variables. |
| **Neural Network Learning Rate** | Controls optimisation speed for neural network ensemble surrogates. | Tuned to produce smoother surrogate predictions and improve optimisation stability. |

---

## Section 5: Results

The optimisation strategy evolved from a single Gaussian Process Bayesian Optimisation framework into a function-specific optimisation pipeline. Different functions exhibited different response surface characteristics, making tailored optimisation strategies consistently outperform a single generic approach.

### Best Performing Strategies

| Function | Final Strategy |
|----------|----------------|
| **Function 1** | GP + UCB with heuristic point selection, visualisation and noise filtering |
| **Function 2** | GP + ARD + UCB with moderate exploration for multimodal search |
| **Function 3** | GP + UCB with balanced exploration and exploitation |
| **Function 4** | Neural Network Deep Ensemble + UCB with exploitative tuning |
| **Function 5** | GP + UCB with balanced exploration and exploitation |
| **Function 6** | Neural Network Deep Ensemble + UCB with increased exploitation |
| **Function 7** | Soft-margin SVM + GP + UCB targeting promising regions |
| **Function 8** | Neural Network Deep Ensemble + UCB with exploitative tuning |

### Best Query Inous and Outputs
F1
Input: [0.425000, 0.429545]
Output: 0.8286823864031674

F2
Input: [0.684524, 0.309524]
Output: 0.6835811441905567

F3
Input: [0.463099, 0.496023, 0.473688]
Output: 0.008818623635985325

F4
Input: [0.348314, 0.382797, 0.401724, 0.362093]
Output: 0.4908152329746014

F5
Input: [1.000000, 0.985038, 0.893541, 0.908000]
Output: 5438.448014038112

F6
Input: [0.511005, 0.370966, 0.636673, 0.835488, 0.009609]
Output: -0.2237771238234831

F7
Input: [0.151315, 0.120420, 0.430978, 0.281602, 0.315301, 0.692483]
Output: 3.2264809068553113

F8
Input: [0.267350, 0.192449, 0.043722, 0.045149, 0.560404, 0.584412, 0.035214, 0.981305]
Output: 9.8041839160995


### Key Findings

- Bayesian Optimisation using Gaussian Processes proved highly effective for low-dimensional problems with limited observations.
- Neural Network Deep Ensemble surrogates significantly improved performance on higher-dimensional functions once sufficient data had been collected.
- SVM classification successfully narrowed the search space by identifying promising regions before Bayesian Optimisation.
- Careful tuning of RBF length scale and UCB β (kappa) had a greater impact on performance than changing acquisition functions.
- Treating each function independently produced stronger results than applying a single optimisation strategy across all problems.
- Poor-performing queries were valuable because they revealed unpromising regions, improving subsequent decisions.

Overall, the project demonstrated that successful black-box optimisation depends not only on selecting an appropriate surrogate model, but also on continuously refining hyperparameters, adapting strategies to the characteristics of each problem, and learning iteratively from every function evaluation.

## Section 5: Documentation
* [Project Data Sheet](https://github.com/jude22/ML-AI-Capstone-Project/blob/main/docs/datasheet.md)
* [Project Model Card](https://github.com/jude22/ML-AI-Capstone-Project/blob/main/docs/modelcard.md)

