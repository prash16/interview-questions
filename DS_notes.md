

#### Statistics

* Type 1: False positive, Test says disease when u dont have one

* Type 2: False negative: Test says no disease when you do have one.

* Confidence interval:  A 95% confidence interval will contain the true parameter with probability 0.95.


* Prediction interval:  Range that is likely to contain the response value of an individual new observation under specified settings of your predictors


* Prediction interval is wider than the confidence interval of the prediction. This will always be true, because additional uncertainty is involved when we want to predict a single response rather than a mean response.



### Machine Learning

**Machine learning** is a form of AI that automates data analysis to enable computers to learn and adapt through experience to do specific tasks without explicit programming.

**Deep Learning** is a subfield of machine learning concerned with algorithms inspired by the structure and function of the brain called artificial neural networks.

**Deductive machine learning** starts with a conclusion, then learns by deducing what is right or wrong about that conclusion. **Inductive machine learning** starts with examples from which to draw conclusions.

##### Data normalization

*  The goal of normalization is to change the values of numeric columns in the dataset to use a common scale, without distorting differences in the ranges of values or losing information


* If we don’t do this then some of the features (those with high magnitude) will be weighted more in the cost function 


* data normalization makes all features weighted equally.

##### Tradeoff between bias and variance

* Underfitting: Model that is not complex enough to capture important features. Eg: Linear model when quadratic is necessary. High training and test error.


* Overfitting: Model that follows the training data too closely. Low training error and high test error.


* Avoid overfitting by using simpler model, cross validation or regularization.

* **Bias**:  Error from erroneous assumptions in the learning algorithm (Underfitting)
	* High bias: Underfitting (Too many assumptions)
	* Low bias: Overfitting

* **Variance**: Error from sensitivity to small fluctuations in training set (Overfitting)
	* High variance: Overfitting (Too much adaptation)
	* Low variance: Underfitting

* A model with high variance is overfitting and has low bias because it makes little to no assumption about the data. It adapts too much to the data.

* **Low complexity** --> High bias & Low variance
* **High complexity** --> Low bias & High variance
* Reduce high bias --> choose more complex model
* Reduce high variance: regularization

##### Gradient descent

* Optimization algorithm, based on a convex function.
* Used to minimize some function by iteratively moving in the direction of steepest descent.
* make use of the magnitude of the gradient, not just its direction. When the magnitude is small, we are in a flatter area and want to take smaller steps.
* **Batch gradient descent:** :run through ALL the samples in your training set to do a single update for a parameter in a particular iteration.
* Adv: computationally efficient and produces more stable error gradient. Disadv: Can result in convergence which isnt best and requires entire training data set in memory and available to the algorithm.
* **SGD**:  you use ONLY ONE or SUBSET of training sample from your training set to do the update for a parameter in a particular iteration.
* Adv: faster and efficient in case if a really large dataset, converges faster. Disadv: noisy gradients, which may cause the error rate to jump around, instead of slowly decreasing.

##### Dimensionality reduction
* Curse of dimensionality:  many features --> slow training and prone to overfitting
* Reducing dimensionality increases speed, reduced overfitting, better visualization and clustering.
* **PCA**: Mapping data into a lower dimensional hyperplane which preserves maximum amount of variance.
* PCA1: captures maximum amount of variance within data
* PCA2: largest remaining variance

##### KNN
* Non parametric:  it means that it does not make any assumptions on the underlying data distribution
* the model structure is determined from the data.
* first choices for a classification study when there is little or no prior knowledge about the distribution data.
* feature similarity
* all (or most) the training data is needed during the testing phase
* data points are separated into several classes to predict the classification of a new sample point.
* pro: no assumption about data, simple, versatile(classification or regression)
* cons: computationally expensive, prediction slow with big n, sensitive to irrelevant features.


##### Linear regression
* **Single Variable Linear Regression** is a technique used to model the relationship between a single input independent variable (feature variable) and an output dependent variable using a linear model i.e a line.

*  **Multi Variable Linear Regression** where a model is created for the relationship between multiple independent input variables (feature variables) and an output dependent variable. The model remains linear in that the output is a linear combination of the input variables.

*  **Polynomial Regression** where the model now becomes a non-linear combination of the feature variables i.e there can be exponential variables, sine and cosine, etc.

##### Decision tree & Random Forest

* **Decision tree**: It subdivides learning data into regions having similar features. Descending the tree allows the prediction of the class or value of the new input data point.


* **Random Forest** : train several decision trees. The original learning dataset is randomly divided into several subsets of equal size. A decision tree is trained for each subset. Note that a random subset of features is selected for the learning of each decision tree. During the prediction, all decision trees are descended and an average is performed on all predictions, for the regression, or a majority vote is performed, for the classification.

##### Support Vector Machine
* It finds the separation (hyperplane in a n-dimensions space) that maximizes the margin between two data populations.
* By maximizing this marge, we mathematically reduce the tendency to overfit the learning data. 
* These support vectors are the data closest to the separation and defining the marge . Once the hyperplane is trained, you only need to store the support vectors for the prediction. This saves a lot of memory when storing the model.
* During prediction, you only need to know if your new input data point is “below” or “above” your hyperplane.

##### Neural Networks
* Neural networks are a set of algorithms, modeled loosely after the human brain, that are designed to recognize patterns.
* **Neurons**: these are functions which contain weights and biases in them and wait for the data to come them. After the data arrives, they, perform some computations and then use an activation function to restrict the data to a range(mostly).
* **Weights/Parameters**: are the numbers the NN has to learn in order to generalize to a problem.
* **Bias**: These numbers represent what the NN “thinks” it should add after multiplying the weights with the data.
* **Activation function**These are also known as mapping functions. They take some input on the x-axis and output a value in a restricted range(mostly). 
	* **Sigmoid**: The main reason why we use sigmoid function is because it exists between (0 to 1). Therefore, it is especially used for models where we have to predict the probability as an output.
	* **Tanh**: The range of the tanh function is from (-1 to 1)
	* **Relu**:  Range 0 to infinity


* Convolutional Neural Networks take advantage of local coherence in the input (often image) to cut down on the number of weights.
* Recurrent Neural Networks are used to process sequential data (often with LSTM cells)

##### Regularization
* Both approaches reduce overfitting by penalizing features with large coefficients and minimizing the difference between predicted value and observation
* Lasso adds a penalty term equivalent to the absolute value of the magnitude of coefficients. so that it zeros out target variables’ coefficients and eliminates them from the model. 
* Ridge assigns a penalty equivalent to square of the magnitudes of the coefficients. 
* L2 (Ridge) shrinks all the coefficient by the same proportions but eliminates none, while L1 (Lasso) can shrink some coefficients to zero, performing variable selection

##### ROC Curve
*  The ROC curve is a graphical representation of the contrast between true positive rates and the false positive rate at various thresholds. 
*  It’s often used as a proxy for the trade-off between the sensitivity of the model (true positives) vs the fall-out or the probability it will trigger a false alarm (false positives).

##### F1
* Recall: True positive rate: the amount of positives your model claims compared to the actual number of positives there are throughout the data. (completeness of results)
* Precision:  Positive predictive value, and it is a measure of the amount of accurate positives your model claims compared to the number of positives it actually claims (usefulness of results)

##### Evaluation metrics

* MAE: average of the absolute difference between the predicted values and observed value
*
* RMSE: represents the sample standard deviation of the differences between predicted values and observed values (called residuals)
* R2 shows how well terms (data points) fit a curve or line.R2 assumes that every single variable explains the variation in the dependent variable. 
* The adjusted R2 tells you the percentage of variation explained by only the independent variables that actually affect the dependent variable.

##### Bootstrap
The bootstrap method is a resampling technique used to estimate statistics on a population by sampling a dataset with replacement.


##### Regularization 

* L1 - lasso - adds absolute magnitude - Less important feature to zero 

* L2 - Ridge regression - added square term - Penalty term- avoid overfitting 


 

Some interview questions to prepare:

Technical Questions


* Gradient descent. Do gradient descent methods at all times converge to a similar point?
* Favorite algorithm
* Type 1 and type 2 error -- “I don’t have a stats background”
* Multiple tests correction - Bonferroni and BH tests
* Explain a complex algorithm to a layman
* Backpropagation and chain rule
* LSTM, RNNs and how do they work?
* Bias-variance trade off for various algorithms. Explain over and under fitting
* Explain PCA
* Map reduce
* Linear regression assumptions
* Interpreting logistic regression output
* What are GLMs? When would you use it?
* What is regularization? How can it be useful?
* Explain precision and recall? What is an ROC curve?
* What is bootstrapping?
* Define statistical power of a test.
* Explain ANOVA and where you would use it
* What is selection bias, why is it important and how can you avoid it?
* How would you fit a model on an imbalanced dataset?
* Explain cross-validation.
* Explain star schema for a database
* What are Eigenvalue and Eigenvector?
* Explain dimensionality reduction, where it’s used, and its benefits?
* Which machine learning model would you use? Ans. Talk about “No free lunch theorem here”
* What is Max pooling?
* What is batch normalization?
* SQL joins --- gotto know this!
* Why is naive Bayes so ‘naive’ ?
* What is multicollinearity? 
* Correlation does not imply Causation. Give examples preferably
* What is collaborative filtering? (recommender systems)
* What is machine learning? Explain like i’m 5
* What is the curse of dimensionality and how should one deal with it?
* determine “k” for k-means clustering?
* How do you identify outliers in -- response and input features? (talk about quantile regression?)
* How would you import and clean a json file to get it ready for an sklearn model/machine learning model? (Python and R) For python, refer to json_normalize function in pandas. For R, jsonlite package maybe?



* He asked me a lot about my capstone project so I guess you should be able to say things about your work.

* My favorite algorithm, why and how does it work?

* How do you evaluate your models

* Linear regression assumptions

* Bias variance tradeoff in Machine learning

* Regularization (L1 and L2)

* How do you deal with unbalanced data

* How neural networks work?

* What is backpropogation (neural networks)

* Underfitting vs overfitting model

* Dimensionality reduction

* Type 1 and type 2 error

* Gradient descent and how does it work

* Bootstrapping

* Bias Variance Trade off 