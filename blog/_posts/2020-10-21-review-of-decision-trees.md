---
layout: post
title: "Review of Decision Trees"
category: Blog
---
A year or so ago, I received my introduction to machine learning thanks to my supervisor. My first model was a decision tree since it is the most popular machine learning algorithm due to simplicity and easy to realize. Now I'm back in machine learning, and this post is a brief review of Decision Trees.

## Introduction

As the name implies, it is a tree that assists us in making decisions. The algorithm belongs to the family of [supervised learning algorithms](https://en.wikipedia.org/wiki/Supervised_learning). However, unlike other supervised learning algorithms, it is applicable for both [classification](https://en.wikipedia.org/wiki/Classification_(general_theory)) and [regression](https://en.wikipedia.org/wiki/Regression_analysis).

![By Gilgoldm - Own work, CC BY-SA 4.0, https://commons.wikimedia.org/w/index.php?curid=90405437](/blog/assets/images/Decision_Tree.jpg)

As the famous [Titanic Decision tree](https://en.wikipedia.org/wiki/Decision_tree_learning#/media/File:Decision_Tree.jpg) shows above ("sibsp" is the number of spouses or siblings aboard), the basic structure of a decision tree contains a *root node*, several *interior nodes*, and the final *leaf nodes*.

### Terminology
- **Root node**: The entire dataset that is further divided
- **Splitting**: The process of dividing a node into two or more sub-nodes
- **Interior node**: A sub-node that split into further sub-nodes
- **Leaf node**: Node that does not split
- **Pruning**: Removal of sub-nodes, opposite to splitting
- **Branch**: Sub-section of the entire tree
- **Entropy**: The impurity of a dataset
- **Gini impurity**: A variation of the usual entropy measure

### Process
1. __Present__ a dataset containing several training instances characterized by several input features and target features.
2. __Train__ the decision tree model by continuously splitting target features along the values of input features using a measure of information gain during the training process.
3. __Grow__ the tree until we reach the stop condition. Create leaf nodes for new query instances.
4. __Show__ the query instance and run it through the tree until we reach the leaf node.

## Principle

Consider an example dataset.

```python
import pandas as pd

data = pd.DataFrame({"Temperature":["hot","hot","hot","cool","cool","mild","cool","mild","mild","mild","hot","mild","cool","mild"],
                    "Outlook":["sunny","sunny","overcast","rain","overcast","sunny","sunny","rain","sunny","overcast","overcast","rain","rain","rain"],
                    "Humidity":["high","high","high","normal","normal","high","normal","normal","normal","high","normal","high","normal","high"],
                    "Windy":["false","true","false","false","true","false","false","false","true","true","false","true","true","false"],
                    "Play Golf?":["no","no","yes","yes","yes","no","yes","yes","yes","yes","yes","no","no","yes"]},
                   columns=["Temperature","Outlook","Humidity","Windy","Play Golf?"])
features = data[["Temperature","Outlook","Humidity","Windy"]]
target = data["Play Golf?"]

data
```

|    | Temperature | Outlook  | Humidity | Windy | Play Golf? |
| -  | ----------- | -------- | -------- | ----- | ---------- |
| 0  | hot         | sunny    | high     | false | no         |
| 1  | hot         | sunny    | high     | true  | no         |
| 2  | hot         | overcast | high     | false | yes        |
| 3  | cool        | rain     | normal   | false | yes        |
| 4  | cool        | overcast | normal   | true  | yes        |
| 5  | mild        | sunny    | high     | false | no         |
| 6  | cool        | sunny    | normal   | false | yes        |
| 7  | mild        | rain     | normal   | false | yes        |
| 8  | mild        | sunny    | normal   | true  | yes        |
| 9  | mild        | overcast | high     | true  | yes        |
| 10 | hot         | overcast | normal   | false | yes        |
| 11 | mild        | rain     | high     | true  | no         |
| 12 | cool        | rain     | normal   | true  | no         |
| 13 | mild        | rain     | high     | false | yes        |

Shannon's entropy model uses the logarithm function $log_{2}(P(x))$ to measure the entropy. The logarithm is to make it growing linearly with system size and "behaving like information".

To sum up the entropies of each possible target value and weight it by the probability, we have the baseline for the calculation:

$$H(x) = -\sum_{for \ k \ \in target}(P(x=k)*log_2(P(x=k)))$$

where $P(x=k)$ is the probability, that the target feature takes a specific value k.

Hence we applying this formula to calculate the information in the data which contained nine yes's and five no's.

$$H(x) = -\frac{9}{14}*log_2(\frac{9}{14})-\frac{5}{14}*log_2(\frac{5}{14}) = 0.94$$

We use the input feature which occupies the most information about the target feature to split the dataset. From now on, we use the **information gain** as a measure of the feature "informativeness". To construct a decision tree on this data, we will take the split with the most information gain as the first. The process will continue until all leaf nodes are pure, or until the information gain is 0. The information gain of a feature is calculated with:

$$InfoGain(feature_{d}) = Entropy(D)-Entropy(feature_{d})$$

The formula for the information gain calculation per feature is:

$$InfoGain(feature_{d},D) = Entropy(D)-\sum_{t \ \in \ feature}(\frac{|feature_{d} = t|}{|D|}*H(feature_{d} = t))$$

$$=$$

$$Entropy(D)-\sum_{t \ \in \ feature}(\frac{|feature_{d} = t|}{|D|}*(-\sum_{k \ \in \ target}(P(target=k,feature_{d} = t)*log_{2}P(target=k,feature_{d} = t))))$$

Now we will calculate the information gain for the feature _temperature_.

In this dataset, there are 4 data points with a _hot_ value, 2 of which have a target variable value of yes and 2 with a value of no. The information of the _temperature=hot_ is calculated using the entropy equation above:

$$H(temperature=hot) = -\frac{2}{4}*log_2(\frac{2}{4})-\frac{2}{4}*log_2(\frac{2}{4}) = 1$$

The data points with a _temperature_ value of _cool_ contain 3 yes's and 1 no's, we have:

$$H(temperature=cool) = -\frac{3}{4}*log_2(\frac{3}{4})-\frac{1}{4}*log_2(\frac{1}{4}) = 0.81$$

For the node where _temperature=mild_ there were 6 data points, 4 yes's and 2 no's. Thus we have:

$$H(temperature=mild) = -\frac{4}{6}*log_2(\frac{4}{6})-\frac{2}{6}*log_2(\frac{2}{6}) = 0.92$$

To find the information of the split, we take the weighted average of these three numbers based on how many observations fell into which node.

$$H(temperature) = \frac{4}{14}*1+\frac{4}{14}*0.81+\frac{6}{14}*0.92 = 0.91$$

Now we can calculate the information gain achieved by splitting on the _temperature_ feature.

$$InfoGain(temperature) = 0.94-0.91 = 0.03$$

To build the tree, we need to calculate the information gain of each possible first split and choose the best that provides the most information gain. The process is repeated for each impure node until the tree is complete.

## Algorithm

Invented by Ross Quinlan in 1986, the ID3 (Iterative Dichotomiser 3) is an algorithm used to generate a decision tree from a dataset. Besides the ID3 algorithm, there are other popular algorithms like the [C4.5](https://en.wikipedia.org/wiki/C4.5_algorithm), the [C5.0](https://en.wikipedia.org/wiki/C5.0_algorithm), and the [CART](https://en.wikipedia.org/wiki/Predictive_analytics#Classification_and_regression_trees_.28CART.29) algorithm. We won't go into details here.

We now introduce the ID3 algorithm through pseudocode:

```pseudocode
ID3 (Examples, Target_Attribute, Attributes)
    Create a root node for the tree
    If all examples are positive, Return the single-node tree Root, with label = +.
    If all examples are negative, Return the single-node tree Root, with label = -.
    If number of predicting attributes is empty, then Return the single node tree Root,
    with label = most common value of the target attribute in the examples.
    Otherwise Begin
        A ← The Attribute that best classifies examples.
        Decision Tree attribute for Root = A.
        For each possible value, vi, of A,
            Add a new tree branch below Root, corresponding to the test A = vi.
            Let Examples(vi) be the subset of examples that have the value vi for A
            If Examples(vi) is empty
                Then below this new branch add a leaf node with label = most common target value in the examples
            Else below this new branch add the subtree ID3 (Examples(vi), Target_Attribute, Attributes – {A})
    End
    Return Root
```

For R users, there are multiple packages available to implement a decision tree such as ctree and rpart.

```R
> library(rpart)
> x <- cbind(x_train,y_train)
# grow tree 
> fit <- rpart(y_train ~ ., data = x,method="class")
> summary(fit)
#Predict Output 
> predicted= predict(fit,x_test)
```

For Python users, below is the code:

```python
#Import Library
#Import other necessary libraries like pandas, numpy...
from sklearn import tree
#Assumed you have, X (predictor) and Y (target) for training data set and x_test(predictor) of test_dataset
# Create tree object 
model = tree.DecisionTreeClassifier(criterion='gini') # for classification, here you can change the algorithm as gini or entropy (information gain) by default it is gini  
# model = tree.DecisionTreeRegressor() for regression
# Train the model using the training sets and check score
model.fit(X, y)
model.score(X, y)
#Predict Output
predicted= model.predict(x_test)
```

## Extension

In this post, we have discovered decision trees for machine learning. Owing to its length, we've only briefly reviewed the basic principles. To improve the model performance, we should adjust the hyperparameters for optimization, here are [more details](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html).

Decision trees have many advantages, such as simple to understand and interpret, able to handle both numerical and categorical data. While the major disadvantage is overfitting, especially when a tree is particularly deep. Fortunately, this issue can be addressed using [pruning](https://en.wikipedia.org/wiki/Decision_tree_pruning). Another approach to increase accuracy is to use an ensemble approach such as [bagging](https://en.wikipedia.org/wiki/Bootstrap_aggregating) and [boosting](https://en.wikipedia.org/wiki/Boosting_(machine_learning)).

Despite decision tree learning is an old method, the more recent tree-based models including [Random forest](https://en.wikipedia.org/wiki/Random_forest) (bagging), [Gradient boosting](https://en.wikipedia.org/wiki/Gradient_boosting) (boosting), and [XGBoost](https://en.wikipedia.org/wiki/XGBoost) (boosting), are built on the top of decision tree algorithms. Such ensemble models have proven themselves to be more powerful. Therefore, a thorough understanding of decision trees is very helpful in building a good foundation for learning machine learning and data science.

## References:
- A. Renyi (1961), [On Measures of Entropy and Information](http://projecteuclid.org/euclid.bsmsp/1200512181), *Proc. of the Fourth Berkeley Symposium on Mathematical Statistics and Probability*, vol. 1, 547-561.
- <https://en.wikipedia.org/wiki/Decision_tree_learning>
- <https://www.python-course.eu/Decision_Trees.php>
- <https://en.wikipedia.org/wiki/ID3_algorithm>