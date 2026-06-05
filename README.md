# Black-Box Optimisation (BBO) CProject

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

The project receives numerical query vectors as inputs. Each function increases in dimensionality from **2D to 8D**, where each dimension represents a controllable parameter.

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

## Section 3: Challenge Objectives

The primary objective is to maximise the unknown functions while operating under several constraints:

* Unknown mathematical structure
* Limited query opportunities
* Expensive evaluations
* Increasing dimensionality
* Possible multimodal landscapes

### Exploration vs Exploitation

The optimisation process requires balancing:

* **Exploration** of uncertain regions
* **Exploitation** of regions predicted to perform well

Some functions appear close to unimodal and favour exploitative strategies, while others exhibit behaviour suggesting local maxima, requiring more exploratory adjustments.

### Learning Through Iteration

The project also requires careful interpretation of objectives. For example, one function was initially treated incorrectly as a minimisation problem before being corrected to maximisation in later rounds.

---

## Section 4: Technical Approach

This project uses a sequential Black-Box Optimisation (BBO) framework to identify high-performing inputs for eight unknown functions ranging from 2D to 8D. Since function evaluations are expensive and only one new query can be submitted per week, the focus is on learning efficiently from limited observations while balancing exploration and exploitation.

The optimisation strategy evolved significantly throughout the project as additional data became available and deeper insights were gained from previous outcomes.

### Core Optimisation Method

The primary optimisation framework is **Bayesian Optimisation (BO)** using:

* Gaussian Process (GP) surrogate models
* Radial Basis Function (RBF) kernels
* Upper Confidence Bound (UCB) acquisition function
* Expected Improvement (EI) acquisition function for comparison

The Gaussian Process surrogate was selected because it performs well on small datasets and provides both mean predictions and uncertainty estimates, allowing informed exploration of unknown regions.

Rather than relying on large predefined candidate grids, acquisition functions were directly optimised as continuous functions for higher-dimensional problems. This avoided the curse of dimensionality and significantly improved computational efficiency.

### Evolution of the Strategy

#### Weeks 1–3: Exploration and Problem Understanding

The initial focus was on understanding the behaviour of each function and identifying whether they appeared unimodal or multimodal.

During this phase:

* UCB was tuned to encourage exploration.
* RBF length scales were adjusted to influence the correlation structure of the surrogate model.
* Several functions revealed that excessive exploration produced poor outcomes.
* Function 6 exposed an early modelling mistake where a maximisation task was incorrectly treated as minimisation.

As more outcomes became available, the optimisation strategy shifted towards function-specific tuning rather than applying a common configuration across all functions.

#### Weeks 4–6: Hyperparameter Refinement

The next phase focused on improving acquisition behaviour through:

* RBF length-scale tuning
* UCB beta (κ) adjustment
* Noise assumption tuning
* Exploration–exploitation balancing

This period highlighted the importance of understanding the interaction between kernel parameters and acquisition functions rather than tuning them independently.

For low-dimensional problems, manual inspection of plots was occasionally used to guide exploration when model recommendations appeared unreliable.

#### Weeks 6–10: Hybrid Models and Advanced Search

As dataset sizes increased, additional techniques were introduced.

##### SVM-Based Region Classification

For Functions 7 and 8, a soft-margin Support Vector Machine (SVM) with an RBF kernel was used to classify promising and non-promising regions of the search space.

The SVM predictions were combined with Bayesian Optimisation to focus acquisition optimisation within regions more likely to contain high-performing solutions.

##### Neural Network Deep Ensemble Surrogates

For Functions 6 and 8, a hybrid Bayesian Optimisation framework was introduced using:

* Neural Network Deep Ensemble surrogate models
* UCB acquisition function

This approach was adopted because:

* Input dimensionality was higher (5D and 8D)
* Dataset sizes had grown sufficiently to support neural networks
* Evaluations remained expensive
* Improved uncertainty estimation was desirable

These hybrid models produced the best outcomes observed for both functions.

### Exploration vs Exploitation Strategy

A key lesson throughout the project was that each function required a different balance between exploration and exploitation.

* Functions suspected to be unimodal generally benefited from aggressive exploitation.
* Functions displaying multimodal characteristics required longer exploratory phases.
* Several functions initially became trapped in local optima, requiring deliberate increases in uncertainty or kernel length scales to encourage broader search.

By Weeks 8–10, most functions had transitioned toward exploitative search as evidence suggested that strong-performing regions had already been identified.

### Key Themes

Overall, the project emphasises:

* Iterative learning
* Model interpretation
* Hyperparameter tuning
* Uncertainty-aware optimisation
* Decision-making under incomplete knowledge
* Adaptive search strategies
* Surrogate model assumptions
* Acquisition function behaviour
* Dimensionality challenges

## Section 5: Documentation
* [Project Data Sheet](https://github.com/jude22/ML-AI-Capstone-Project/blob/main/docs/datasheet.md)
* [Project Model Card](https://github.com/jude22/ML-AI-Capstone-Project/blob/main/docs/modelcard.md)

The iterative nature of the project reinforced the importance of evidence-based tuning, continuous reflection and adapting optimisation strategies as new information becomes available.
