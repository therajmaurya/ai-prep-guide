# Causal Inference / AI Explainability / Model Reasoning or Interpretation / Insight Reasoning

Various Techniques to Approach Causal Inference Problems:

* Build Self-Interpretable Models (KNN, Decision Trees, Naive Bayes etc.)
* Model Agnostic Methods: Surrogate Model Based Explanations (LIME, SHAP etc.)
* Example Based Explanations (Counterfactuals etc.)
* Interventional Study (What-If Analysis - Counterfactuals)
* Observational Study (Analysis on Treatment vs Control Group) - NOT USEFUL FOR US YET.

## Approach Classifications:

### Self-interpretable Models
Explains model predictions: regression and classification both.
- Linear Regression and it’s extensions like GLMs and GAMs (regression)
- Logistic Regression (classification)
- Naive Bayes (classification)
- RuleFit (regression, classification)
- Decision Trees (regression, classification)
- K-nearest neighbours - KNN (regression, classification)

### Model Agnostic Methods
Explains models outcomes: regression and classification both.

**Plot based techniques:**
- Partial Dependence Plot (PDP)
- Individual Conditional Expectation (ICE) Plot
- Accumulated Local Effects (ALE) Plot

**Surrogate Model based techniques:**
- Global Surrogate
- Local Surrogate or LIME (by Rebiero, Singh and Guestrin)

**Game-theory based techniques:**
- Shapely Values
- SHAP: SHapely Additive exPlanations (KernelSHAP or TreeSHAP)

**Reinforcement Learning + Graph Search based technique:**
- Anchor or Scoped Rules (by Rebiero, Singh and Guestrin - same as LIME Researchers; the opposite of counterfactuals)

**Feature based techniques:**
- Feature Interaction (Friedman’s H-Statistics)
- Permutation Feature Importance (by Fisher, Rudin and Dominici)

### Example-based Explanations:
“Can explain insights or instance itself for model outcomes.”
- Counterfactual Explanations (USEFUL FOR OUR CURRENT SCOPE)
- Prototypes and Criticisms (TBD: To be done/explored later)
- Adversarial Examples (NA: Not useful for us right now)
- Influential Instances (NA: Not useful for us right now)

### Interventional Causal Inference:
Useful in What-If Analysis.
- Counterfactuals (USEFUL FOR OUR CURRENT SCOPE)
- Extrapolation capable models (simplified models like linear/logistic regression)

### Observational Study (Analysis on Treatment vs Control Group):
Not useful for us.
It include concepts like Inverse Probability of Treatment Weighing (IPTW), Trust Scores, Longitudinal Study, Instrumental Variable Analysis, Principal Stratifications, Regression Discontinuity Analysis, Heckman Correction, Ignorability Concept etc. to design & analyse treatment & control group, fill unobserved data and pre and post processing required for this analysis. Most popular techniques used are:
- RCM: Rubin Causal Model (aka Neyman-Rubin Causal Model)
- Propensity Scores (PSM: Propensity Score Matching)
- DID: Difference in Differences Technique - Median of Differences Technique

