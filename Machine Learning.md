
Task - Performance - Experience

Experience is gained through dataset and classification of it.

dataset: collection of examples or data points or instances 

features: a number or a string representing a category

performance can be measured by accuracy and error

Training set: examples used to train the ML algorithm

Testing set: examples used to evaluate the ML algorithm

  

Types of ML systems:

### Supervised learning
- [[Linear Regression]]
- labeled data, has the solutions
- this produces predictions for a new label    

### Unsupervised learning
- receive unlabeled data, no "correct" data
- clustering
- anomaly detection

### Semi-supervised learning
- dataset is partially labeled
- clusters and finds label of the cluster
### Reinforcement learning
-   observe
-   select action using policy, usually guesses at the beginning stages
-   action
-   get reward or penalty
-   update policy
-   iterate until an optimal policy is found

Batch vs Online
-   Batch: trained all at once, if new data available, have to start from scratch
-   Online: trained incrementally, new data is incorporated


Instance vs Model based
-   Instance: analyzing training data, kept in memory to make the predictions
-   k nearest neighbors, new instance is added, it measures nearest neighbor to other training data
-   Model: uses the model to make predictions, training data can be discarded
    

the performance of typically simple algorithms will eventually reach the accuracy of complex algorithms given enough data. Ex: Naive Bayes classifier 

Challenge: 

-   insufficient quantity of training data
    
-   non representative training data
    
-   poor quality data
    
-   irrelevant features overfitting training  data
    
-   underfitting training data
    

Use the np functions instead of the math ones

# Neural Networks
*activation functions*

![[Drawing 2023-02-23 09.41.05.excalidraw]]

[[Convolutional Neural Networks]]