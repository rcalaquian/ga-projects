### ***Project 3 Feedback***

***Nico Van de Bovenkamp***

***

**Overall:**  
Awesome, awesome job on this assignment! You worked through the entire workflow and got some solid models out (there was some lift above your base rate!). You also effectively built in your categorical variables too!

Below are some comments and suggestions for improvement on your modeling process. Overall, I think you have a solid, solid grasp on how to use these methods (which makes sense), but not always *why*. I would encourage you to just sit and focus on these types of questions:

* Why this model versus others?
* What are these evaluation metrics?
* What does it mean if I tweak my model a bit?

**Please let me know if any of these comments are unclear or if you have any questions at all! My goal is to provide you with effective feedback so that you get the most out of these assignments, so let me know if something doesn't sit right with you.**

**Some notes**  

* **Train/test Split earlier** As we discussed in our review session, the first thing that you want to do is perform your train/test split (once you have your data). You don't want any information about your test set to "leak" into what you are learning about your training set. All preprocessing, transformations, handling of missing values, etc. should be done on your train set as the goal is to have it generalize onto your test set. Again, the concept is: "We don't have the test yet, that comes tomorrow." Also, note that your test size is a bit large. I would stick to values closer to 0.1 - 0.2. Otherwise, you are just losing out on data that you could be using to train your model.
* **Mapping**  Nice use of a list comprehension in `bank.y = [tc_flag[item] for item in bank.y]`. This is more than fine, but I wanted to point out that we do have some handy pandas functions that you can use, like the `map()` function!
* **Get Dummies**  A quick note on using `pd.get_dummies()`: you can actually perform this without looping!

```python
# Columns = columns that you selected to dummify.
bank = pd.get_dummes(bank, columns=columns, drop_first=True)
```

* **Scaling your data**  Again, as we discussed in our review session, it's pretty much always a good idea.
* **Precision vs. Recall vs. F1** Which should you maximize? This is a key question in any modeling task. If you want to avoid False Negatives, cases when you predict class 0 but it is actually class 1, then you want to maximize Recall. The reverse is true for Precision (avoid False Positives). I hope I am understanding this correctly problem correctly, but is this about if a person defaults on a loan or not? If it is, then you probably want to be sensitive to saying someone won't default when they actually do. This means you really want to avoid False Negatives because being wrong about about defaults can be super costly (my apologies if I don't properly understand the problem). So, how do we shift our balance of recall and precision:
    - Try different models, with different hyperparameters, features, or totally different all together, and pick the one that gives you the best recall (without losing too many False Positives).
    - With logistic regression, because we have probabilities, you can actually change a threshold for making the classification. Instead of making it a majority vote (probability > 0.5 in binary), you can cut it off at a higher threshold. If you make the cut-off to say it is going to rain (class 1) at probability 0.4, you are being more flexible to saying "Hey, it may rain." This will lower your chance of making a False Negative prediction!
    - The last option is by weighting... You can actually give more weight to a certain class if it has more cost: https://stackoverflow.com/questions/30972029/how-does-the-class-weight-parameter-in-scikit-learn-work
* **Cross Validation!** You always want to cross validate your models! That is how we ensure that we are building a solid model. If you do not cross validate, then there is not much assurance that you didn't overfit your training set. You should always be building your models with an eye on those cross val scores. If this is unclear, let's chat after class!
* **Hyperparameter optimization**  As we discussed in our review session, once you have decided on a solid set of features, you want to do some level of hyper parameter tuning. You currently aren't adding any penalty to your model, which could help with some over-fitting.
    - Logistic regression:
        - C : Amount of penalty you apply to your weights! Remember that in this implementation, it is actually 1/C. Thus, a smaller C means more penalty as you are adding MORE of the weights to the penalty.
        - penalty: 'l1' vs. 'l2'
        - fit_intercept : (this is a parameter, but you likely always use this term)
        - solver : {‘newton-cg’, ‘lbfgs’, ‘liblinear’, ‘sag’, ‘saga’} this is your term to change the optimization procedure! These can be interesting to experiment with. On a smaller dataset, this may not be so important, but when datasets become huge... This is very important.
    - KNN:
        - n_neighbors : number of neighbors.
* **Evaluation Metrics**  As we discussed in class and our review session, you want to look at tons of different evaluation metrics. You did take a look at the classification report, which is good, but spend sometime digging in to what those metrics mean and what your models are telling you. Let me know if you have questions on this!
* **Scoring functions**  A quick note on passing values into the scoring functions: you want to pass the **TRUE** labels in first and then the **PREDICTED** in second: `accuracy_score(y_train, preds)`
* **precision_recall_cure and roc_curve**  One of the greatest things to do is to plot out those roc_curves and precision_recall_cure to see how we can threshold our models. What the precision recall curve is doing is it is taking a sweep of different cut-off points, or **thresholds**, and then showing you the precision score and recall score that you get from that threshold. **Also**, note that you may want to look at a precision recall curve at the top line (the average over each class), but you might also want to look at the percision_recall_curve of just ONE class. This will always be the case for AUC_ROC as it only computes the curve on single classes.
* **Using different models**  So, why would you want to use LogisticRegression vs. KNN? I hope you have a better idea after our review session, but here are some points:
    - **Model structure**  Logistic regression is modeling the probability of a class occurring, given some data, to make a linear classification boundary. KNN does not have a model form, it is just empirically taking the structure of your training data and hoping that there is some local information given your data that will have once class be closer to another class (distance).
    - **Model output**  Logistic regression output model probabilities. This means that we can rank the outcomes of things (based on probability) and we can see a band of different outcomes! You have more information in there. When you use KNN, we are just applying a label to a new datapoint.
    - **Efficiency and performance**  If you have tons of features, KNN is going to be SLOW. At times, it will even be totally useless as distance becomes meaningless in high dimensions. Logistic Regression are very, very fast.
