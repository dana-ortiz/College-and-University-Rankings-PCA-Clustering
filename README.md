# American College and University Rankings - PCA and Clustering

### Basic Information

* **Members:** Dana Ortiz, [dana.ortiz@gwu.edu](mailto:dana.ortiz@gwu.edu)
* **Date:** April 2026
* **Model Version:** 1.0
* **License:** MIT
* **Model Implementation Code:** [University PCA and Clustering](University%20PCA%20and%20Clustering.ipynb)

### Intended Use

* **Intended Uses:** This project is an educational example of using unsupervised learning methods to analyze American college and university ranking data. The project uses hierarchical clustering to group similar universities based on continuous measurements, uses those clusters to support missing-value imputation, and uses principal components analysis to reduce dimensionality and identify major patterns in the data.
* **Out-of-Scope Use Cases:** This project should not be used for official university rankings, admissions decisions, funding decisions, institutional evaluation, or real-world policy decisions. The clustering and PCA results are for academic demonstration only and should not be treated as a complete or authoritative measure of university quality.

### Training Data

* **Data Dictionary:**

| Name                         | Modeling Role             | Measurement Level  | Description                                                            |
| ---------------------------- | ------------------------- | ------------------ | ---------------------------------------------------------------------- |
| **College Name**             | identifier                | string             | Name of the college or university                                      |
| **State**                    | characterization variable | categorical        | U.S. state where the institution is located                            |
| **Public (1)/ Private (2)**  | characterization variable | categorical/binary | Indicates whether the university is public or private                  |
| **# appli. rec'd**           | input                     | numeric/count      | Number of applications received                                        |
| **# appl. accepted**         | input                     | numeric/count      | Number of applicants accepted                                          |
| **# new stud. enrolled**     | input                     | numeric/count      | Number of newly enrolled students                                      |
| **% new stud. from top 10%** | input                     | numeric/percentage | Percentage of new students from the top 10% of their high school class |
| **% new stud. from top 25%** | input                     | numeric/percentage | Percentage of new students from the top 25% of their high school class |
| **# FT undergrad**           | input                     | numeric/count      | Number of full-time undergraduate students                             |
| **# PT undergrad**           | input                     | numeric/count      | Number of part-time undergraduate students                             |
| **in-state tuition**         | input                     | numeric/dollar     | Tuition cost for in-state students                                     |
| **out-of-state tuition**     | input                     | numeric/dollar     | Tuition cost for out-of-state students                                 |
| **room**                     | input                     | numeric/dollar     | Estimated room cost                                                    |
| **board**                    | input                     | numeric/dollar     | Estimated board cost                                                   |
| **add. fees**                | input                     | numeric/dollar     | Additional student fees                                                |
| **estim. book costs**        | input                     | numeric/dollar     | Estimated textbook and book costs                                      |
| **estim. personal $**        | input                     | numeric/dollar     | Estimated personal expenses                                            |
| **% fac. w/PHD**             | input                     | numeric/percentage | Percentage of faculty with a PhD                                       |
| **stud./fac. ratio**         | input                     | numeric/ratio      | Student-to-faculty ratio                                               |
| **Graduation rate**          | input                     | numeric/percentage | Graduation rate of the institution                                     |

* **Source of Training Data:** `Universities.csv`, the American College and University Rankings dataset containing 1,302 American colleges and universities offering undergraduate programs.
* **How training data was divided into training and validation data:** Records with missing measurements were removed before clustering and PCA. From the complete records, a random sample of 90% was taken for the main analysis. No supervised train-validation split was used because this is an unsupervised learning project.
* **Number of rows in training and validation data:**

  * Original dataset rows: 1,302
  * Complete records after removing missing measurements: 471
  * Training/sample rows used for clustering and PCA: 424
  * Validation rows: Not applicable
  * Complete records excluded by 90% sample: 47

### Test Data

* **Source of test data:** The partial records from `Universities.csv` with missing measurements. Pomona College was used as the main imputation example.
* **Number of rows in test data:** 831 incomplete records exist in the dataset. The specific imputation example uses one partial record: Pomona College.
* **State any differences in columns between training and test data:** The training data contains complete observations for all variables. The test/imputation records contain the same columns, but some measurements are missing. For Pomona College, the missing measurements are `# PT undergrad`, `room`, and `board`.

### Model Details

* **Columns used as inputs in the final model:** `# appli. rec'd`, `# appl. accepted`, `# new stud. enrolled`, `% new stud. from top 10%`, `% new stud. from top 25%`, `# FT undergrad`, `# PT undergrad`, `in-state tuition`, `out-of-state tuition`, `room`, `board`, `add. fees`, `estim. book costs`, `estim. personal $`, `% fac. w/PHD`, `stud./fac. ratio`, `Graduation rate`
* **Column(s) used as target(s) in the final model:** None. This is an unsupervised learning project, so there is no target variable.
* **Type of model:** Hierarchical clustering using complete linkage and Euclidean distance; principal components analysis; cluster-based missing-value imputation.
* **Software used to implement the model:** Python, pandas, NumPy, scikit-learn, SciPy, Matplotlib, Seaborn, Jupyter Notebook
* **Version of the modeling software:** Python 3.14
* **Hyperparameters or other settings of your model:**

```python
StandardScaler()
```

```python
linkage(
    X_scaled,
    method='complete',
    metric='euclidean'
)
```

```python
PCA()
```

```python
# Imputation approach:
# 1. Normalize continuous variables.
# 2. Cluster complete records using hierarchical clustering.
# 3. For a partial record, compute Euclidean distance to each cluster centroid
#    using only the available measurements.
# 4. Assign the partial record to the closest cluster.
# 5. Impute missing values using the average values from that cluster.
```

### Quantitative Analysis

* **Metrics Used to Evaluate:** Because this is an unsupervised learning project, RMSE, accuracy, precision, and recall are not the main evaluation metrics. Instead, the analysis evaluates cluster interpretability, cluster summary statistics, categorical relationships, Euclidean distance for imputation, and PCA explained variance.

| Analysis Step                    | Result                                                                   |
| -------------------------------- | ------------------------------------------------------------------------ |
| Original dataset size            | 1,302 universities                                                       |
| Complete records                 | 471 universities                                                         |
| 90% sample used for analysis     | 424 universities                                                         |
| Clustering method                | Complete-linkage hierarchical clustering                                 |
| Distance metric                  | Euclidean distance                                                       |
| Normalization used               | Yes                                                                      |
| Final cluster selection          | Based on dendrogram inspection                                           |
| PCA normalization                | Yes, recommended because variables are measured on very different scales |
| Pomona College missing values    | `# PT undergrad`, `room`, `board`                                        |
| Pomona College imputation method | Closest-cluster mean imputation                                          |

The PCA results should be interpreted using standardized variables because the dataset combines variables measured in very different units, including counts, percentages, dollar amounts, and ratios. Without normalization, variables with large numeric scales, such as tuition, enrollment, and application counts, would dominate the principal components.

The main components can generally be interpreted as broad institutional patterns. One important component tends to capture academic selectivity and cost, including variables such as tuition, graduation rate, percentage of students from the top 10% or top 25% of their high school class, and student-faculty ratio. Another important component tends to capture institutional size, including number of applications received, applications accepted, new students enrolled, and full-time undergraduate enrollment.

### Ethical Considerations

* **Potential negative impacts of using this model:**

  * *Math or Software Problems:* The results depend heavily on preprocessing choices, including removing missing records, normalizing variables, selecting the number of clusters from a dendrogram, and choosing cluster averages for imputation. Removing all incomplete records may bias the clustering process if missingness is not random. Cluster-based imputation may also oversimplify differences between institutions by replacing missing values with group averages.
  * *Real World Risks:* If used outside an educational setting, this type of analysis could reinforce misleading or overly simplistic interpretations of university quality. Variables such as tuition, graduation rate, faculty credentials, and selectivity are shaped by institutional resources, geography, public/private status, state funding, and student demographics. Clusters should not be treated as rankings or as proof that one type of university is better than another.

* **Uncertainties relating to the impacts of using the model:**

  * *Math or Software Uncertainties:* Hierarchical clustering can produce different interpretations depending on the linkage method, distance metric, scaling method, and selected number of clusters. PCA components are also descriptive, not causal. The project identifies patterns in the data but does not prove why those patterns exist.
  * *Real World Uncertainties:* The dataset may not reflect current university conditions, tuition levels, rankings, or graduation rates. External information such as university endowment, regional cost of living, state funding levels, institutional mission, student demographics, research intensity, and Carnegie classification could help explain why certain schools cluster together.

* **Unexpected Results:** Some clusters may be strongly influenced by institutional size, tuition, or selectivity rather than by an overall measure of university quality. This means that large public universities, selective private colleges, and smaller regional institutions may separate into different clusters for structural reasons. Additionally, categorical variables such as `State` and `Public (1)/ Private (2)` were not used in the clustering model but can still help explain why clusters differ after the fact.

### AI Use Disclosure

AI tools were used during this project for README drafting. All final submissions are work of the project author.
