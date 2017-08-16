\begin{figure}[htbp] %  figure placement: here, top, bottom, or page
   \centering
   \includegraphics[width=2in]{example.jpg} 
   \caption{example caption}
   \label{fig:example}
\end{figure}\documentclass{beamer}

% \usepackage{beamerthemesplit} // Activate for custom appearance

\title{Example Presentation Created with the Beamer Package}
\author{Till Tantau}
\date{\today}

\begin{document}

\frame{\titlepage}

\section[Outline]{}
\frame{\tableofcontents}

\section{Introduction}
\subsection{Overview of the Beamer Class}
\frame
{
  \frametitle{Features of the Beamer Class}

  \begin{itemize}
  \item<1-> Normal LaTeX class.
  \item<2-> Easy overlays.
  \item<3-> No external programs needed.      
  \end{itemize}
}
\end{document}
Practical Business Applications for Linear Regression
================
Paul Newton

Most business people have encountered **Linear Regression** at some point in their lives - but rarely use it in practice. In a world where very few things have a linear relationship, it can be deemed irrelevant or too simplistic for practical use.

However, my experience has been that business performance improvement is usually about detecting beneficial or detrimental patterns and suitably adapting for them. While linear regression is the simplest of models, when used appropriately, it can still detect insightful patterns and convey them to business line managers in a manner that they understand and (most importantly) can act upon.

Accuracy vs Simplicity in Models
--------------------------------

If the purpose of a model is to identify **general** rules and behavior to explain business mechanisms, then the accuracy of predictions is less important than if the intent is to predict a quantitative outcome, given a set of characteristics. While the natural desire is to build accurate models, they invariably come with the price of additional complexity. Unless there is a compelling reason for accuracy, I usually opt for the simpler model because:

1.  The consumer of the insight is often a business line manager (rather than an analytics peer or a machine) who needs to buy into the logic of the conclusions. Therefore, it is usually more productive to convey a simple, but relevant model than a more accurate, but complex one.

2.  The cost of being "wrong" on any given prediction is frequently low when compared to the status quo, or the actionable improvements that a more general model facilitates. Being right on the general pattern, even allowing for errors on the specifics, provides a clear pathway for improvements.

3.  Performance gains are made when things are incrementally improved rather than striving for perfection. Therefore, it is desirable to accept less accuracy if the model is intuitive, and the actions can be speedily adopted by the wider company.

If accuracy is critical, linear regression is unlikely to produce an optimal model and a more sophisticated technique should be considered. However, if we are looking for patterns to subsequently change behaviour, linear regression can be an additional tool - as I hope the use-case below, taken from my own experiences, demonstrates.

Our Business Problem
--------------------

#### *(Adapted from an Actual Use-Case)*

Suppose we sell a perishable widget. When fresh (which is most of the time), we achieve our full Gross Margin (GM) on sales. But as our unit of widget ages, it requires ever increasing price reductions to ensure we are not left with a product that needs to be thrown out (at a corresponding GM of -Cost).

<img src="PN_Linear_Regression_Utility_v1.0_test_files/figure-markdown_github-ascii_identifiers/plot-1.png" style="display: block; margin: auto;" />

The above data is comprised of just two variables and `Days (on-Hand)` our independent or predictor variable and `Gross Margin $` our dependent or response variable. The dataset has 746 transactions/records totaling $18,882 in net Gross Margin at an average GM of ~$25 per transaction.

A casual observer of the above graph would note that the relationship between GM and Days is unlikely to be linear. The business intuitively understands that margins drop off rapidly after a specific (or vague) amount of days.

Below we show how linear regression helps clarify the mechanics of what is a non-linear relationship?

### Simple Linear Regression

First of all, let's look at how well a simple linear model (of GM$ on Days) represents the situation. This simple linear model is expressed as *G**M* = *β*<sub>0</sub> + *β*<sub>1</sub> \* *D**a**y**s*

<img src="PN_Linear_Regression_Utility_v1.0_test_files/figure-markdown_github-ascii_identifiers/lm1a-1.png" style="display: block; margin: auto;" /> The line above represents the predicted values from the model for a given day, and by observing the actual GM values, it can be expected to underpredict GM$ for the newer widgets, while overpredicting for the older ones.

#### Model Statistics

    ## 
    ## Call:
    ## lm(formula = GM ~ Days, data = days)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -51.774  -4.541   1.235   4.973  23.820 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  42.6077     0.6687   63.72   <2e-16 ***
    ## Days         -3.6803     0.1218  -30.21   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 9.437 on 744 degrees of freedom
    ## Multiple R-squared:  0.5509, Adjusted R-squared:  0.5503 
    ## F-statistic: 912.7 on 1 and 744 DF,  p-value: < 2.2e-16

##### Call:

This is the formula for the linear regression model. `GM` being the dependent variable, `Days` the independent variable, and `data = days` being the dataset used.

##### Residuals:

This shows the distribution of the differences between actual and predicted values (residuals). In this case, the Min of -51.774 indicates that in one observation, the actual GM was ~$52 less than the predicted. Similarly, the max of 23.82 indicates that one record's actual GM was ~$24 higher than predicted.

These are fairly large residuals, with the mean GM being $25.3 - so the predictive *accuracy* of the model is in question. Furthermore, this distribution does not appear to be normal, showing a long left tail.

Further information on the Residuals is shown in the plots below.

<img src="PN_Linear_Regression_Utility_v1.0_test_files/figure-markdown_github-ascii_identifiers/lm1c-1.png" style="display: block; margin: auto;" />

For a good explanation on interpreting these plots, please see [here](http://data.library.virginia.edu/diagnostic-plots/).

In short:

1.  Residuals vs Fitted - The difference between actuals and predicted (residuals) are large when predicting negative GM
2.  Normal Q-Q - Residuals are not normally distributed, particularly for negative predictions.
3.  Scale-Location - Variances are not evenly spread across the predictive range
4.  Residuals vs Leverage - The outliers do not influence the regression model too much.

This is not a good predictive model. However, if we accept that we are not particularly interested in predicting a specific outcome, the model does explain the general workings of the business.

Given that the point is to show the usefulness of imperfect, linear models, let's take it as given that the following two models also exhibit residuals consistent with a poor predictive model. Therefore I will omitt the corresponding residual plots for the models below, although anyone interested can run them from the provided data and code.

##### Coefficients:

###### Estimate

The Estimate represents the intercept and the slope values for linear regression - and for the above model, the intercept is $42.61 with a slope of $3.68. For practical business application this means that an average unit of widgets starts out with $42.61 in margin and loses $3.68 for every day.

While perhaps not a particularly accurate prediction as seen from the residuals, as a concept, it is simple and useful for the business to know that each passing day has a -$3.68 impact on GM. Even if not entirely accurate, this is something tangible and actionable.

###### Std Error, t-Value & Pr(&gt;|t|)

These three statistics combine to provide a probability that the co-efficient is in fact zero. The `Estimate / Std. Error` gives you the t-Value which, is the number of std errors that the co-efficient value is away from zero and using a t-distribution to calculate Pr(&gt;|t|), this is this probability that the coeffient is zero.

For our model above, both the intercept and slope have a tiny probability of being zero, and are therefore statistically significant (we would use a 0.05 or 5% threshold to determine significance).

Thus the slope and the coefficient both provide information about the true relationship between Days and GM.

##### Residual Standard Error

This is the average variation from the regression line, that the actuals exhibit. Thus, on our mean GM of $25.3, we can expect an average variation of $9.44 or 37.3%, which is rather high.

##### Adjusted R-Squared

R-Squared is always between zero and one and idicates how well the model fits the actual data. Adjusted R-Squared takes into account the number of variables used in the model, and is the more appropriate measure to review.

With an Adjusted R-Squared of 0.55, roughly 55% of the variance found in the response variable `GM` can be explained by the predictor variable `Days`. This means that `Days` contains some amount of information that is useful to predict `GM`, but there is something else that contains information about `GM` that we are not picking up in this model.

##### F-Statistic

The F-statistic asks if there is a relationship between the dependent and independent variables - with a larger F indicating more evidence of such, based on the number of data points and variables. In this case, with F = 912.7, and a correspondingly small p-value, we can conclude that there is (statistically) certainly a relationship between the two.

#### Summary

The simple linear model contains some useful information and the fact that we know the business loses ~$3.68 for every day that the widget is held, is a useful reference point. However, we also know that the model is missing some information, so next we look at whether we can make changes to our linear model to gain information.

### Polynomial Regression

It is clear from the plot above that Days-GM is not a linear-relationship but that doesn't mean we can't use linear modelling to understand the business better. A simple way to extend the linear model to a non-linear relationship is to transform the independent variable `Days` to `log(Days)`, `square-root(Days)` or `(Days)^n` and perform a linear regression of the transformed independent variable.

After some (off-line) analysis, I decided to add the square of the days as a second independent variable. So, if a widget is sold on the 6th day, the independent variables are 6 `Days` and 36 `Days2`.

**Note: This is still a linear regression as the model is now: *G**M* = *β*<sub>0</sub> + *β*<sub>1</sub> \* *D**a**y**s* + *β*<sub>2</sub> \* *D**a**y**s*<sup>2</sup> **

<img src="PN_Linear_Regression_Utility_v1.0_test_files/figure-markdown_github-ascii_identifiers/lm2a-1.png" style="display: block; margin: auto;" />

The non-linear curve (from a linear regression) is clearly evident now and appears to better match the actuals, although for 20+ days does seem to over predict the losses.

    ## 
    ## Call:
    ## lm(formula = GM ~ Days + Days2, data = days)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -34.908  -3.014  -0.819   3.136  50.728 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 27.35774    0.60847   44.96   <2e-16 ***
    ## Days         2.36469    0.19164   12.34   <2e-16 ***
    ## Days2       -0.43675    0.01272  -34.34   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 5.871 on 743 degrees of freedom
    ## Multiple R-squared:  0.8264, Adjusted R-squared:  0.826 
    ## F-statistic:  1769 on 2 and 743 DF,  p-value: < 2.2e-16

##### Residuals

The residuals are still fairly high and non-normal - with errors ranging up to $52 for the longer aged widgets.

##### Coefficients

The Co-efficients are all significant, although interpreting them becomes a little harder. The `Intercept` means that at Day 0, the expected GM is $27.36, but for every day that passes, GM increases by $2.36 - which is counter-intuitive given that in the simple linear model, it was a negative relationship between `Days` and `GM`.

This is explained with the Days-Squared co-efficient `Days2`, which states that for every Day-Squared, the margin decreases by 44c and has the effect of installing exponentially larger decreases as the number of days increases - thereby ofsetting the effect of the positive `Days` coefficient. But what does Days-Squared actually mean in a business context?

<strong> Accuracy vs Simplicity:</strong>
While the Polynomial Regression model better predicts the likely GM, *D**a**y**s*<sup>2</sup> is not something that is easily understood and highlights the trade-off between accuracy and simplicity. For accurate predictions, Polynomial Regression is preferred, but for pattern communication, the Simple Linear Model would be better.

##### Residual Standard Error

With a RSE of 5.87 on our mean GM of $25.3, we can expect an average variation of $5.87 or 23.2%, which is considerably lower than the simple linear model's RSE of 9.44.

##### Adjusted R-Squared & F-Statistic

The Adjusted R-Squared has risen to 0.826 which compares very favourably with the simple regression's 0.55. The F-statistic of 1769 shows there to be a significant relationship between the independent and dependent variables.

##### Summary

The exponential effect of *D**a**y**s*<sup>2</sup> had a positive prediction impact on GM, but at the expense of simplicity. So is there a way to keep the model simple enough to explain and action, while also improving accuracy over the simple linear model? We turn to Qualitative or Categorical predictors to see if that helps.

### Linear Regression with Qualitative Predictors

Up to this point, we have used `Days` as a predictor, which implicitly assumes that Day 10 is "one more"" than Day 9 and tries to a model an equal effect for each additional day.

To incorporate a Qualitative (or Categorical) variable, I have assigned each day to one of three logical groups in a new variable `Age` - such that *New* = &lt;6 Days old, *Medium* = 6-10 Days old and *Old* = &gt;10 Days. (I selected the ranges to optimize R-squared, but if the company had a logical grouping, that would be more relevant).

The linear expression is: *G**M* = *β*<sub>0</sub> + *β*<sub>1</sub> \* 1(*i**f**M**e**d**i**u**m*)+*β*<sub>2</sub> \* 1(*i**f**O**l**d*)

<img src="PN_Linear_Regression_Utility_v1.0_test_files/figure-markdown_github-ascii_identifiers/lm3a-1.png" style="display: block; margin: auto;" /> Note that a linear regression on a Categorical variable with three categories will produce what is essentially three flat lines, with breaks at the group cut-off points.

    ## 
    ## Call:
    ## lm(formula = GM ~ Age, data = days)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -35.352  -3.027  -1.089   3.496  42.777 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  29.1303     0.2874  101.37   <2e-16 ***
    ## AgeMedium    -6.3007     0.5444  -11.57   <2e-16 ***
    ## AgeOld      -75.0564     1.4627  -51.31   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 6.572 on 743 degrees of freedom
    ## Multiple R-squared:  0.7825, Adjusted R-squared:  0.7819 
    ## F-statistic:  1336 on 2 and 743 DF,  p-value: < 2.2e-16

##### Residuals:

The residuals look to be more normally distributed, although are still over a wide range, indicating our model misses quite badly at times.

##### Coefficients:

With Categorical variables, there is a *Baseline* factor from which the other groups are referenced. So in our case, `New` is the baseline and the Intercept relates to `Age = New` and means that *New* widgets expect to generate GM of $29.13. `Age = Medium` is expected to generate $6.30 less than *New* and `Age = Old` $75.06 less than *New*. All co-efficients are statistically significant.

This is an intuitive model and can be easily communicated to the wider business. $29.13 is a baseline by which all widget margins can be measured. $6.30 is a fair sized drop in margin for days 6-10 and also becomes an actionable point of improvement. Finally, quantifying the $75 loss (on average) for units over 10 days would be unacceptable to most people and crystalizes the need for action.

##### Residual Standard Error & Adjusted R-Squared

RSE & R-squared of 6.57 and 0.78 respectively are much improved over the simple linear model (9.44 and 0.55), but somewhat worse than the Polynomial Regression (5.87 and 0.826).

##### Summary

The Categorical model retains most of the information gains of the Polynomial Model, but with a significant reduction in complexity. This is an easily explainable model that is sufficiently accurate to get the point across. I feel it is a good balance between Accuracy and Simplicity and in this situation, is the model that I would use to drive the reduction in our aged widgets.

### Summary of Models

Below are the three **Linear** models from above plotted against the hypothetical example's data-points. None of the three models are particularly good at predicting the really old widgets, partly due to the actual decline flattening out as it approaches the limit to GM losses - which is the initial cost of the widget.

<img src="PN_Linear_Regression_Utility_v1.0_test_files/figure-markdown_github-ascii_identifiers/allmodels-1.png" style="display: block; margin: auto;" />

Drastically mis-predicting the handful of sales over ~15 days may not be a problem if the intuitive understanding that margin declines with age has now been quantified and made tangible (and probably more urgent).

### Conclusion

For me, the conclusion that there needs to be a strong initiative to reduce the age of **all** sold widgets has been formalized and quantified with a linear model. While the model lacks accuracy, it's simplicity is its strength as I work to convince the business line managers to change their processes to adress this avoidable margin leakeage.

------------------------------------------------------------------------

<strong>Zimtoti Analytics</strong>  |     ![](logo3.png)
