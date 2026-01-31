### 1. Counterfactual Explanations:
(Python Implementation using “alibi” library)

A counterfactual explanation describes a causal situation in the form: "If X had not occurred, Y would not have occurred". For example: "If I hadn't taken a sip of this hot coffee, I wouldn't have burned my tongue". Event Y is that I burned my tongue; cause X is that I had a hot coffee.

Given this simple graph, it is easy to see how we can simulate counterfactuals for predictions of machine learning models: We simply change the feature values of an instance before making the predictions and we analyse how the prediction changes. We are interested in scenarios in which the prediction changes in a relevant way, like a flip in predicted class (e.g. credit application accepted or rejected) or in which the prediction reaches a certain threshold (e.g. the probability for cancer reaches 10%). A counterfactual explanation of a prediction describes the smallest change to the feature values that changes the prediction to a predefined output.

**Simplest Implementation:**
The basic idea is that for the selected datapoint, we look at all other datapoints classified as a different class as the selected one. We then calculate the distance from the selected datapoint to each of those others, to find the closest. The distance between examples is calculated on a feature by feature basis, and then those feature distances are combined using either L1 (sum of absolute values) or L2 (sum of squares) distance. For numeric features, the distance between two examples is the number of std deviations apart the two values are. For categorical features, the distance is 0 if they are identical, and if they are different, the distance score is the probability that any two randomly drawn examples will have the same value for that feature. Features with fully unique values in all examples (like images or text strings) are currently ignored for the distance measure.

**Advantages:**
- The interpretation of counterfactual explanations is very clear. It is not as dangerous as methods like LIME, where it is unclear how far we can extrapolate the local model for the interpretation.
- The counterfactual method does not require access to the data or the model.
- The method works also with systems that do not use machine learning.
- The counterfactual explanation method is relatively easy to implement, since it is essentially a loss function that can be optimised with standard optimiser libraries.

**Disadvantages:**
- For each instance you will usually find multiple counterfactual explanations (Rashomon effect). It might not be a big issue for us but we want only one explanation then choosing one becomes complex.
- There is no guarantee that for a given tolerance (ϵ) a counterfactual instance is found. That is not necessarily the fault of the method, but rather depends on the data.
- This method does not handle categorical features with many different levels well. A solution that handles both numerical and categorical variables with a principled way of generating perturbations for categorical variables is implemented in the Python package Alibi.
