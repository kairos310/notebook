**Regression** because it predicts numbers as the output.

**Classification** is when we predict one of a finite number of categories rather than a number, like predicting if a picture is of a dog or a cat, or if a patient has a disease or not, or one of many diseases.

```
	TrainingSet
		⬇
learning algorithm 
		⬇
	x ➡ f ➡ y
features of sqft
				price in thousands
```
y = true y
𝑦̂ = predicted y

𝑓 is called the _model_. A model takes features as inputs and produces predictions as outputs.
## **Model**
$$f_{w,b}(x)=wx+b$$


**Overfitting** -> model is too complex, fits training data perfectly
**Underfitting** -> model is too simple
**Univariate** means we only have one input variable here, 𝑥x.


## **Squared error cost function**
**Cost function** measures how well a possible line fits the training data
exagerates larger errors, penalizes these points
$$J(w,b)= \frac{1}{2m} \sum_{i=1}^{m}(\hat{y}^i-y^i)^2 $$
2 - for differentiation 
m - number of points
J - error (cost) function

$$J(w,b)= \frac{1}{2m} \sum_{i=1}^{m}(f_{w,b}(x^i)-y^i)^2 $$
