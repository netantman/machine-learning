# Problem Formulation/Definition

- What is the **core business problem**/**dependent variable** we are trying to model: determine potentially which ML models we need.
    - 'Predicting stock returns': regreesion; 'Predicting stock price change directions': classification. 
    - Remember to ask clarifying questions.
- What are the right **performance metrics** to satisfy business goals?
    - What value is added for the business?
    - If a wrong prediction is made, how does it impact the business?
- [Scope](problem-definition/scoping.ipynb) of the project. 
    - How good does it need to be so as to considered a success?
    - Are there **benchmarks**? Human-level performance?
    - Are there reasons (economically or common-sense) that the data has informative information about the dependent variable?
- System Limitations
    - What is the **budget**?
    - What is the **latency** needed?
    - At what **frequencies** do you need the predictions?
    - **Throughput**: what is the total number of predictions throughout the day?
    - Where is the model **deployed**?
- Other considerations
    - Is ML even needed? Can we use a simple heurestics or rules?
    - Are there legal or regulatory risks associated with the project?

# Data

### Sourcing data
- **What data fields** can be useful? (Probably need inspirations from <font color=red> ChatGPT </font>)
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

### Cleaning and checking Data

#### What is good data
- **No missing, duplicated or erroneous** values: what counts as erroneous or outliers could be debatable.
- **Consistent labeling**: there is no ambiguities or noise in $p(y|x)$. 
    - Eliminating labeling ambiguities sometimes is more effective in improving data quality and algo effectiveness, than obtaining more data.
- $p(x)$ covers the whole feature space.
- **Size appropriately**: there is **no uninformative data**, yet sufficiently big for models to learn.

#### Some other things to check
See [data-checks](data/data-checks.ipynb).
- Leakage
- Distributional bias between training and serving data.
- Nonstationary in data
    - What are the ways to check?
    - If non-stationary data is not avoidable, what are other remediations?

    
### EDA

- **Summary statistics**: sample mean, median, mode, standard deviations, quantiles.
- **Data visualization**: histograms, box plots (for outlier detection), scatter plots (for high-dimensional $x$, maybe a slice of data), density plots.
- **Correlations**: among $x$ and between $x$ and $y$; good to also show scatter plots.
- **Outlier detection**: z-score, interquartile range (IQR)
- **Dimension reduction**: Inspect what the principle directions and principle components are - [PCA](../unsupervised-learning/PCA.ipynb)
- **Cluster analysis**: [K-Means](unsupervised-learning/Kmeans.ipynb)

### Feature Engineering

- **Economic intuitions**: probably need inspirations from <font color=red> ChatGPT </font>

- **Scaling**: min-max, normalization and standardization.

- **Encoding**: one-hot encoding (embedding), label encoding, and binary encoding.

- **Imputation**: mean/median/mode interpolation, linear interpolation, [kNN](../../supervised_learning/kNN.ipynb); note that sometimes missing data can simply be dropped.

- **Transformation**: log transformation, square root transformation, power transformation and quantile transformation.

- **Non-linearity and higher-order**: hinge transform, cross products of features; see [MARS](../../supervised_learning/MARS.ipynb)

- **Dimensionality Reduction**: [PCA](../unsupervised-learning/PCA.ipynb)

- **Feature Selection**: 
    - **Correlation analysis**
    - **Mutual information**
    - **Chi-square test**: used only for categorical features, and measures the dependence between feature and target variable
    - **Recursive feature elimination**: see examples in [linear-regression](../../supervised-learning/linear-regression.ipynb)
    - **L1 regularization, or lasso**
 

# Modeling

### Models

#### Supervised Learning

#### Unsupervised Learning

### Model Selection

(bullet points in Ace Book)

### Tuning Machine Learning Models

### Input/Output Format and Representation (Probably part of model)

# Performance Metrics/Evaluation

# Deployment
