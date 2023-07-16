#textbook-intro-to-statistical-learning

## 2.1. What is Statistical Learning?

- We assume there's a relationship between $Y$ and $X = (X_1, X_2, \dots, X_p)$ which can be represented in the general form:
$$Y = f(x) + \epsilon$$
- Where $f$ is a fixed but unknown function of $X_1, \dots, X_p$, and $\epsilon$ is a random error term (independent of $X$ and mean 0). $f$ represents the systematic information that $X$ provides about $Y$, and is something we need to estimate based on observed points.
- Statistical learning refers to the set of approaches for estimating $f$



### 2.1.2 How Do We Estimate f?

- Techniques for estimating $f$ can be characterised as either *parametric* or *non-parametric*.
- *Parametric* techniques require that a functional form be assumed for the data, which provides a set of parameters that one needs to estimate using the data. i.e. it reduces the problem of estimating f down to one of estimating a set of parameters.
	- In general, fitting a more flexible parametric model involves fitting more parameters
- *Non-parametric* techniques do not make assumptions about the functional form of $f$. However, since they don't reduce the problem down to that of fitting a finite set of parameters, they require a large number of observations to obtain an accurate estimate for $f$.

### 2.1.3 The Trade-Off Between Prediction Accuracy and Model Interpretability

- Why would we use a more restrictive method instead of a flexible approach? There is often a trade-off between how restrictive a model is and how interpretable it is.
- If our goal is inference, then we'd likely choose a more restrictive model due to the high interpretability it allows. On the other hand, some methods are so flexible that they lead to complicated estimates of $f$ that are difficult to understand. In this case, it may be difficult to understand how any individual predictor is associated with the response. 
- However, if our goal is prediction accuracy, then we'd choose the method that provides the greatest performance over our dataset. This may not necessarily be the most flexible model type, due to the possibility that the flexibility allows the model to [[overfit]].

### 2.1.4 Supervised Versus Unsupervised Learning

- *Supervised learning* refers to the problem set where for each observation of the predictor measurements ($x_i$, $i=1,\dots,n$) there is an associated response measurement ($y_i$). We aim to fit a model that relates the response to the predictors with either prediction or inference as the goal. ^31a009
	- Many statistical learning methods fall into this domain: linear/logistic regression, Generalised Additive Models, boosting, and support vector machines.
- *Unsupervised learning* describes the situation where for every observation ($i=1,\dots,n$), we have a vector of measurements ($x_i$) but no associated response ($y_i$). We do not have response data to supervise our analysis, which limits what we're able to achieve. ^0ebfb2
	- We can instead seek to understand the relationships between the variables or the observations using:
		- [[Cluster analysis|Cluster analysis/clustering]]: where the goal is to determine whether observations fall into distinct groups (i.e. categorising potential customers)
- There are also *semi-supervised* situations where we have responses for some of the $n$ observations, but no responses for the rest of them. This may arise if it is expensive to obtain response data. We would want to use learning methods that can incorporate the observations for which there are responses, but also the ones without responses - however this is beyond the scope of this book.

### 2.1.5 Regression Versus Classification Problems

- Variables are either *quantitative* (numerical) or *qualitative* (categorical).
- We refer to problems with a quantitative response as *regression* problems; whereas those with qualitative responses are referred to as *classification* problems.
	- Some techniques step into both definitions such as logistic regression which estimates class probabilities (regression), but then turns these into a binary response (classification).
	- Some techniques can be used in either quantitative or qualitative situations such as [[K-nearest neighbours]] and [[boosting]].

## 2.2. Assessing Model Accuracy

