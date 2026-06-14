The idea of this acticle is to show that Bayes theorem can be used to obtain ML metrics from confusion matrix. That main metrics are related to eachover with the same equation as we have in Bayes theorem.

Let's start with the same illustration as we have in bayesian_mushrooms/article.md
but instead of mushrooms use "You work as a salesman and know that only 10% of marketing leads for the particular product will buy it. ML team in you company developed a new prediction model, but you collegue tells you -- among all prediction of this model only 50% are correct. Shoud you use the model?"

Next as in the previous article we should say that if we know only that 10% of leads buy (Prevalence=0.1) and that the model predicts only 50% of them correctly (Recall=0.5) it is now enough to decide is the model good or not. We need to know how many positive prediction it give us (What Precision it has).

Next we need to write about the fact that both articles looks similar, and this is not coincidence. Bayes theorem good for any kind of additional knowledge. And knowledge about red caps of mushrooms is good as knowledge of trained ML model. They both give us additional information and can be used the same formal way the theorem give us.

Next we need to illustrate this with -- lets prove the folmula for Recall, Prevalence, and Precision with the same geometrical approach. Take confusion matrix, define metrics, and show that Bayes theorem unite them by deffinition. as it was with mushrooms.

In the conclusion summarize the results of both articles.
