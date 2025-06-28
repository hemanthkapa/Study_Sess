A dimensionality technique used in data anand machine learning. It helps to reduce the number of features in a dataset while keeping the most important information.

## 1. Standardize the Data 

While doing PCA we often deal with features that have different units and scales. If we dont standardize, features with larger scales will dominate the analysis and PCA will will mostly capture the variance from those features, ignoring the smaller scaled ones.

$$
Z = \frac{X - μ}{σ} 
$$
where, 
μ is mean of the independent features
σ is the standard deviation of independent features
X is the original value 

## 2. Calculate the Covariance Matrix 

 Covariance matrix is a table that shows how much each pair of features in the data changes together. If two features increase of decrease together, their covariance is positive, if one increases while the other decreases, its negative. if unrelated then close to zero. 
$$
cov(x1, x2) = \sum_{i=1}^{n}(x1_i- \overline{x1})(x2_i- \overline{x2})/n-1
$$
Where 
$\overline x_1$ and  $\overline x_2$ are the mean values of features $x_1$ and $x_2$
n is the number of data points

## 3.  Find the Principal Components

PCA identifies new axes where the data spreads out the most: 

- **PC1 (1st Principal component):** The direction of maximum variance (most spread)
- **PC2 (2nd Principal component):**** The next best direction, perpendicular to PC1 and so on
These directions come from the [[eigenvectors]] of the covariance matrix and their importance is measured by eigenvalues. 
- For a square matrix A an eigenvector X (a non-zero vector) and its corresponding eigenvalue λ satisfy: 
$$
AX = λX
$$
Which means: 
- When A acts on X it only stretches or shrinks X by the scalar λ. 
- The direction of X remains unchanged hence eigen vectors define "stable directions" of A

## 4. Pick the Top Directions & Transform Data 

Once we calculate the eigenvalues and eigenvectors. PCA ranks them by the amount of information they capture. 

- **Select the Top k components** = Decide how many components to keep. Often choose enough to capture a high percentage of the total variance (~95%)
- **Transform the Data**: Project the original data onto the selected principal components. This means we express each data point in terms of new axes. Multiply the data by matrix of the top eigen vectors. 
- Result is reduced dimension data. 
