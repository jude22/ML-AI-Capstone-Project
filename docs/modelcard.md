# Model Card: Black-Box Optimisation (BBO) Capstone Strategy

## Overview

### Model Name

**Hybrid Bayesian Optimisation Strategy for Black-Box Function Maximisation**

### Type

Sequential Black-Box Optimisation Framework

### Version

**v1.1 (Weeks 1–13 Capstone Submission)**

### Summary

This approach was developed to optimise eight unknown functions with dimensions ranging from 2D to 8D under a limited evaluation budget. The strategy combines Bayesian Optimisation, Gaussian Process (GP) surrogate modelling, acquisition function optimisation, Support Vector Machine (SVM) classification and Neural Network (NN) Deep Ensemble surrogates to iteratively identify high-performing query points.

---

## Intended Use

### Suitable Tasks

This approach is intended for:

* Expensive black-box optimisation problems
* Hyperparameter tuning
* Engineering design optimisation
* Scientific experimentation with costly evaluations
* Sequential decision-making under uncertainty
* Small-to-medium sized datasets where uncertainty estimation is important

Example applications include:

* Machine learning hyperparameter optimisation
* Drug discovery experiments
* Manufacturing process optimisation
* Supply chain and warehouse configuration
* Resource allocation problems

### Inappropriate Uses

This approach should not be used for:

* Real-time optimisation requiring millisecond decisions
* Large-scale supervised learning tasks
* Safety-critical systems without extensive validation
* Problems requiring guarantees of global optimality
* Domains where uncertainty estimation is unreliable or unavailable

---

## Details

### Weeks 1–3: Exploration and Problem Understanding

The initial phase focused on understanding function behaviour and identifying whether functions appeared unimodal or multimodal.

Methods included:

* Gaussian Process surrogate models
* UCB acquisition function
* Expected Improvement (EI) comparison
* Exploration-focused tuning

During this phase, important lessons included correcting Function 6 from minimisation to maximisation and recognising the impact of RBF length scales on exploration behaviour.

### Weeks 4–6: Hyperparameter Refinement

The strategy evolved toward function-specific tuning.

Key techniques:

* RBF length-scale optimisation
* UCB beta (κ) tuning
* Noise assumption adjustment
* Manual intervention for low-dimensional functions

The relationship between kernel parameters and acquisition behaviour became a major focus.

### Weeks 6–8: Hybrid Methods

Additional techniques were introduced for higher-dimensional functions.

#### SVM Classification

Applied to Functions 7 and 8:

* Soft-margin SVM with RBF kernel
* Identification of promising regions
* Combined with Bayesian Optimisation for focused search

#### Neural Network Deep Ensemble Surrogates

Applied to Functions 6 and 8:

* Deep ensemble surrogate models
* UCB acquisition optimisation
* Improved uncertainty estimation in higher dimensions

### Weeks 9–13: Controlled Exploitation

This phase emphasised exploitation of identified promising regions while maintaining targeted exploration where performance plateaued.

Manual guidance was occasionally used for Function 1 where visual inspection of 2D landscapes provided additional insight.

---

## Performance

### Evaluation Metric

The primary metric was:

**Function output value (maximisation objective)**

Performance was assessed by:

* Best observed outcome
* Improvement over previous rounds
* Ability to identify promising regions
* Stability of optimisation behaviour

### Summary Across Functions

| Function | Key Outcome                                                                      |
| -------- | -------------------------------------------------------------------------------- |
| F1       | Best result achieved through manual exploration combined with GP guidance        |
| F2       | Progressive improvement through exploration before shifting to exploitation      |
| F3       | Mixed performance with evidence of local optima challenges                       |
| F4       | Significant gains from exploitation after exploratory phases                     |
| F5       | Consistently strong results using GP + UCB exploitation                          |
| F6       | Best outcome achieved using NN Deep Ensemble + UCB                               |
| F7       | Best outcome achieved using SVM-assisted Bayesian Optimisation                   |
| F8       | Best outcome achieved using NN Deep Ensemble + UCB with SVM-informed exploration |

---

## Assumptions and Limitations

### Assumptions

The strategy assumes:

* Nearby points may contain improved solutions.
* Surrogate models adequately approximate unknown functions.
* Historical observations are informative for future searches.
* Acquisition functions can effectively balance exploration and exploitation.

### Limitations

Several limitations exist:

* Small datasets throughout optimisation.
* Potential trapping in local optima.
* High sensitivity to kernel and acquisition hyperparameters.
* Uneven sampling of the search space.
* Limited guarantees of finding the global optimum.
* Neural network surrogates may still be data-constrained in higher dimensions.

### Known Failure Modes

Potential failure modes include:

* Over-exploitation of local maxima.
* Poor uncertainty estimation.
* Excessive sensitivity to noise.
* Misleading surrogate predictions in sparsely sampled regions.

---

## Ethical Considerations

### Transparency and Reproducibility

Transparency was maintained through:

* Weekly reflections documenting decisions and outcomes
* Version-controlled query submissions
* Explicit recording of hyperparameter changes
* Documentation of modelling assumptions

This level of documentation enables other researchers to understand, reproduce and critique the optimisation process.

### Responsible Use

Users should recognise that optimisation outcomes are influenced by:

* Sequential sampling decisions
* Limited observations
* Surrogate model assumptions
* Exploration–exploitation trade-offs

Results should therefore be interpreted as evidence-based approximations rather than guaranteed optima.

### Real-World Adaptation

The project demonstrates how transparent optimisation workflows can support responsible deployment in real-world applications. By documenting assumptions, limitations and decision processes, practitioners can better assess when optimisation recommendations are reliable and when additional validation is required. This is particularly important in domains such as healthcare, manufacturing and machine learning system design, where decisions may have significant operational or societal consequences.
