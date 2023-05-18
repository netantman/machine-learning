# Problem Formulation/Definition

- What is the **core business problem**/**dependent variable** we are trying to model: determine potentially which ML models we need.
    - Remember to ask **clarifying questions**
    - For instance, 'Predicting stock returns': regression; 'Predicting stock price change directions': classification. 
    - Does it make sense to predict **another, related variable**?
- What are the right **performance metrics** to satisfy business goals?
    - What value is added for the business?
    - If a wrong prediction is made, how does it impact the business?
- [Scope](problem-definition/scoping.ipynb) of the project. 
    - Are there **benchmarks**? Human-level performance?
    - Are there reasons (economically or common-sense) that the data has informative information about the dependent variable?
    - How good does it need to be so as to considered a success? What is the **hurdle rate**?
- System Limitations
    - What is the **budget**?
    - What is the **latency** needed?
    - At what **frequencies** do you need the predictions?
    - **Throughput**: what is the total number of predictions throughout the day?
    - Where is the model **deployed**?
- Other considerations
    - Is ML even needed? Can we use a simple heurestics or rules?
    - Are there legal or regulatory risks associated with the project?
- Draw a diagram similar to [this one](problem-definition/MLOps-overview.ipynb), setting the groundwork for discussion.

# Data

### Sourcing data

- **What data fields** can be useful? (Probably need inspirations from **ChatGPT**)
- **Where do you source the data** and **how costly** is the data?
- At what **frequency** do you need to source the data?
- How long should you spend sourcing data？
    - Unless there is prior experience, there is no golden rule of how much data is enough. Try to **get into the loop** of obtaining data - traing models - error analysis **as soon as possible**.
    - Instead of asking 'how long can we collect $m$ data'. Ask 'how much data can we collect in $k$ days'.
    - See this [graph](data/time-getting-data.ipynb).
- Stress the importance of **data pipelines**, **data lineage** and **data provinance**, as well as the idea of **versioning data**.
- What are the **advanced labeling techniques**: [advanced-labeling](data/advanced-labeling.ipynb).
    - Semi-supervised learning
    - Active learning: margin sampling, clustered-based sampling, query-by-commitee, region-based sampling
    - Weak supervision
    - Note: advanced labeling is to deal with missing $y$, not missing $X$.

### Cleaning and checking Data

#### What is good, clean data

- **No missing, duplicated or erroneous** values: what counts as erroneous or outliers could be debatable.
- **Consistent labeling**: there is no ambiguities or noise in $p(y|x)$. 
    - Eliminating labeling ambiguities sometimes is more effective in improving data quality and algo effectiveness, than obtaining more data.
- $p(x)$ **covers the whole feature space**, or the desired region.
- There is **no uninformative data**: watch out for **sparseness** and **low variance** columns; see the discussion of [degenerate distributions](data/feature-selection.ipynb) in the feature selection notebook.
- **Size appropriately**: sufficiently big for models to learn but not too much for models to train; further notes on [dataset size](data/data-size.ipynb)

#### Some other things to [check](data/data-checks.ipynb)

- **Leakage**: $y$ accidentally a column in $X$; this is different than the type of leakage encountered in cross-validation.
- **Distributional bias** between training and serving data.
- **Non-stationary** in data
    - What are the ways to check?
    - If non-stationary data is not avoidable, determine the best frequency to recalibrate/retrain the model.

    
### EDA

The **motivation** is to better understand both $p(x)$ and $p(y|x)$.

- **Summary statistics**: sample mean, median, mode, standard deviations, quantiles.
- **Data visualization**: 
    - histograms or density plots: on $X$ 
    - box plots (for outlier detection): on $X$ and/or $y$
    - [scatter plots](data/scatter-further-note.ipynb) between $X$ and $y$
        - works particularly well for low-dimensional $X$;
        - for high-dimensional $X$, maybe a slice of data), .
- **Correlations**: among $X$ and between $X$ and $y$; good to also show scatter plots.
- **Outlier detection**: z-score, interquartile range (IQR)
- **Dimension reduction**: Inspect principle directions and principle components for insights - [PCA](../unsupervised-learning/PCA.ipynb)
- **Cluster analysis**: [K-Means](../unsupervised-learning/Kmeans.ipynb)

### [Feature Engineering](data/data-preprocessing-feature-engineering.ipynb)

- **From economic intuitions**: probably need inspirations from **ChatGPT**

- **Scaling**: min-max, normalization and standardization - just be very careful to do consistently across training and testing.

- **Encoding**: one-hot encoding (embedding), ordinal encoding, and binary encoding.

- **Transformation**: log transformation, square root transformation, power transformation and quantile transformation.

- **Non-linearity and higher-order**: hinge transform, cross products of features; see [MARS](../supervised_learning/MARS.ipynb)

- **Dealing with Missing Data**: mean/median/mode interpolation, linear interpolation, [kNN](../supervised_learning/kNN.ipynb), random sampling; note that sometimes missing data can simply be dropped.

- **Dimensionality Reduction**: [PCA](../unsupervised-learning/PCA.ipynb)

- [**Feature Selection**](data/feature-selection.ipynb): what are the **advantages** of feature selections?
 

# Modeling

## Model selection

- Heuristics -> simple model -> more complex model -> ensemble of models; see [model-tips](models/models-tips.ipynb)
    - Pros and cons, and decision
    - The benefit of trying at least two kinds of models: a **most baseline model** and a **most complex model**
    - Always start as simple as possible (KISS) and iterate over: **do not prematurely statistical optimization**.

- Typical models
    - [Linear regression](../supervised-learning/linear-regression.ipynb)
    - [kNN](../supervised-learning/kNN.ipynb)
    - [Logistic Regression](../supervised-learning/logistic-regression.ipynb)
    - [Decision tree](../supervised-learning/CART.ipynb) and variants 
        - [bagging](../supervised-learning/boosting.ipynb) and [random forest](../supervised-learning/random-forest.ipynb)
            - What are the two ways random forest employ to reduce overfitting?
        - [boosting](../supervised-learning/boosting.ipynb)
    - [SVM](../supervised-learning/SVM.ipynb)
    - [MARS](../supervised-learning/MARS.ipynb)
    - Neural networks
        - [MLP](../supervised-learning/MLP.ipynb)
        - [CNN](../supervised-learning/CNN.ipynb)
        - [RNN](../supervised-learning/RNN.ipynb)
        - Transformers
    - [emsemble](ensemble.ipynb)
    - [Gaussian processes](../supervised-learning/gaussian-process.ipynb)
    - [PCA](../unsupervised-learning/PCA.ipynb)
        - What is the mathematical formulation?
        - Is PCA scale invariant? What if we do not scale $X$ before we do PCA?
        - Limitation of PCA?
    - [Kmeans](../unsupervised-learning/Kmeans.ipynb)
        - Describe the algorithm
        - How to choose $K$?
- [Things to consider](../supervised-learning/pros-n-cons.ipynb)
    - Complexity of the task
    - Data: Type of data (structured, unstructured), amount of data, complexity of data
    - Training time
    - Inference requirements: compute, latency, memory
    - Continual learning/online learning
    - [Interpretability](models/interpretability-explanability.ipynb)


## Data aspect of modeling

- Sampling: when you have too much data
    - Non-probabilistic sampling
    - random, stratified, reservoir, importance sampling

- Class Imbalance
    - Resampling
    - weighted loss function: focal loss
    - combining classes

- Input/Output Representation
    
## Tuning ML models

- Loss functions
    - MSE, MAE, Huber loss: which one is most sensitive to outliers?
    - log likelihood
    - Binary/Categorical cross entropy (or KL Divergence)
    - Focal loss: the loss function for classification that deals with class imbalance
    - Hinge loss: see [SVM](../supervised-learning/SVM.ipynb)
    
- [Optimizers](models/optimizers.ipynb): make sure you know their formulae or the rational
    - SGD and BGD: motivation of random samples and batch?
    - Further variants: AdaGrad, RMSProp, Adam
    
- [Cross-validation](models/cross-validation-and-backtesting.ipynb)
    - What is the purpose of doing cv? What could be the pitfalls?
        - What is the one-standard-deviation rule?
    - How would you relate cv and backtesting?
    - Portions
    - Splitting time-correlated data (split by time): seasonality, trend, embargo
        - How to solve the lack-of-data problem when doing time-series cv or backtesting?
    - Data leakage hazard: improper pre-scaling, duplicates, temporal data
    - Hyperparameter searching: grid search, random search, Bayesian optimization

- Debugging or curating ML models
    - [Error analysis](models/error-analysis.ipynb)
    - [Curating ML models](models/curating-ML-models.ipynb)
    - [Regularization](models/regularization.ipynb)
    - [Early stopping](models/early-stopping.ipynb)
    - [Experiment Tracking](models/experiment-tracking.ipynb)
    - [Performance Auditing and Sensitivity Analysis](models/performance-audit-sensitivity-analysis.ipynb)


- [Model offline evaluation](models/evaluation-metrics-and-information-criterions.ipynb)

- Offline vs online training: what should be the frequency to recalibrate the model?


# Domain-Specific Performance Metrics/Evaluation

- [Investment metrics](domain-specific-metrics/investment-metrics.ipynb)
    - General characteristics
    - Performance
    - Runs
    - Implementation Shortfall
    - Efficiency Ratios
    

# [Deployment](deployment/MLOps-deployment.ipynb)