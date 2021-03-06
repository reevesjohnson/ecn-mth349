**Cross Validation**
================

# Quantifying the Quality of a Fit

> How do you measure the performance of your fitted model? That is, what
> mathematical standard should you use to measure how well the model’s
> predictions compare to the data? There are many such measures, but the
> one most commonly-used is the *mean-squared error* (MSE), defined by:
>
> $$
> MSE = \\frac{1}{n} \\sum\_{i=1}^n (y_i - \\hat{y}\_i)^2, 
> $$
>
> where *ŷ*<sub>*i*</sub> is the predicated value of the *i*th
> observation. One way to remember the *M**S**E* formula is to think
> about it backwards: Find the “error” (*E*) or distance between the
> observed value, *y*<sub>*i*</sub> and its predicted value,
> *ŷ*<sub>*i*</sub>; “square” (*S*) this error; then take its “mean”
> (*M*) over all *n* observations.

# Training and Test Data

When you fit a model to data, how confident are you that the model will
generalize? For example, if you gather new data and fit the same model
to the new data, will your results vary significantly? If the predictive
power of your worsens as you fit the model to a different set of data,
then you might want to consider another model. Cross validation is a
powerful technique to investigate, among other things, the degree to
which fitting a model to different data differs and is a necessary
component of any workflow where the objective is predictive performance.
When you use cross-validation techniques you’re in the stage of model
assessment, or perhaps model comparison.

New data are usually not available when you’re at the stage of model
fitting. Unless the dataset you’re working with is very small, however,
you can divide the data into two non-overlapping groups: A training set
and a test set. A model is fit with the *training set*. Observations in
the training set are used to *train*, or teach, the estimation method

An obvious question you might be thinking of is: How should you
partition the data into training and testing sets? Shouldn’t there be a
some sort of decision rule to guide how the data are divided? There are
a few ways to determine the split:

1.  *The Validation Set Approach*:

    -   Step 1: Randomly divide the data into two (usually equal) parts
        to form a training and test set.

    -   Step 2: Fit the model with the training set to obtain results.

2.  *k**-Fold Cross-Validation*:

    -   Step 1: Choose some integer *k* with 1 \< *k* \< *n*, and
        partition the data into *k* approximately equal-sized groups
        called “folds.”

    -   Step 2: Hold out one fold and use the remaining *k* − 1 folds as
        training data to fit the model.

    -   Step 3: Repeat Step 2 *k* − 1 more times, holding out a
        different fold each time you fit a new training set.

3.  *Leave-One-Out Cross-Validation* (LOOCV):

    -   Step 1: Select the first observation,
        (*x*<sub>1</sub>,*y*<sub>1</sub>), to form the test set
        {(*x*<sub>1</sub>,*y*<sub>1</sub>)}, the remaining observations
        forming the training set
        {(*x*<sub>2</sub>,*y*<sub>2</sub>), …, (*x*<sub>*n*</sub>,*y*<sub>*n*</sub>)}.

    -   Step 2: Fit the model with the training set.

    -   Step 3: Repeat Steps 1 and 2, each time holding out a single
        observation (*x*<sub>*i*</sub>,*y*<sub>*i*</sub>) for
        *i* = 2, …, *n*. Note that implementing LOOCV means you’ll have
        *n* training sets, one for each observation left out, and thus
        fit the model *n* times.

We’ll use LOOCV in the following examples as it’s currently the most
widely adopted approach to cross validation. See the following schematic
display of the LOOCV approach that illustrates how the data are
partitioned.

Now, recalling that the *M**S**E* is our choice to measure the quality
of a fit, the *n* estimations generate *n* squared errors for the test
set,
*M**S**E*<sub>1</sub>, *M**S**E*<sub>2</sub>, …, *M**S**E*<sub>*n*</sub>.
Specifically,
*M**S**E*<sub>1</sub> = (*y*<sub>1</sub>−*ŷ*<sub>1</sub>)<sup>2</sup>,
*M**S**E*<sub>2</sub> = (*y*<sub>2</sub>−*ŷ*<sub>2</sub>)<sup>2</sup>,
…,
*M**S**E*<sub>*n*</sub> = (*y*<sub>*n*</sub>−*ŷ*<sub>*n*</sub>)<sup>2</sup>.
Looking back at the definition of the *M**S**E* given above, note that
there’s not need to sum observations because there’s only 1 observation
in each of the *n* test sets.

We can derive the LOOCV estimate for the test *M**S**E* by averaging
these *n* test error estimates:
$$
CV\_{(n)} = \\frac{1}{n} \\sum\_{i=1}^n MSE_i.
$$

Do CV for purposes of model assessment;

Cross-validation techniques can be implemented with the **modelr**
package. The **modelr** package isn’t part of **tidyverse**, so you’ll
need to install and load it into R in the usual way.
