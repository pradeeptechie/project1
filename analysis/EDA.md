

# Exploratory Data Analysis (EDA) findings

This document summarizes the complete exploratory data analysis performed on the image-derived feature dataset consisting of 27,200 samples and 3,690 numerical features, along with associated metadata columns.

The same analysis is been done in the corresponding file - EDA.ipynb.. However, to make it easy to read created this file with Extensive analysis block below and to further ease up process final conclusive analysis is also identified in 8 points below

1. The dataset is high-dimensional, sparse, and moderately noisy, consistent with classical CV pipelines.
2. Extreme class imbalance remains the primary challenge for downstream modeling.
3. Several engineered features contain outliers and redundancy.
4. PCA significantly reduces dimensionality but does not meaningfully improve separability.
5. Minority classes remain difficult to separate due to overlapping feature distributions.
6. SMOTE improves numeric balance but risks introducing artifacts into the feature space.
7. Grid-based labeling introduces ambiguity that propagates through EDA and all downstream tasks.
8. The combined analysis indicates the need for enhanced feature engineering, improved labeling accuracy, or alternative sampling strategies to support reliable model development.

---

## Extensive Analysis can be find below:

---

## 1. Dataset Structure and Initial Inspection

1. The dataset contains 3,694 columns, of which 3,690 are numerical features (`f0` to `f3689`) generated through image feature extraction processes.
2. Additional metadata columns include `label`, `image_path`, `cell_number`, and `cell_image_path`.
3. The presence of approximately 3.7K features indicates a high-dimensional dataset typical of pipelines using HOG, LBP, color moments, and other classical descriptors.
4. Initial inspection confirmed no missing values, indicating successful completion of preprocessing and feature extraction.
5. The dataset shows consistent row counts, and every cell-image pair is accounted for without missing references.

---

## 2. Missing Value Analysis

6. A complete scan for missing values found zero missing entries across all fields.
7. This absence of missing data indicates stable image decoding and extraction.
8. No imputation strategies were required in the EDA phase.

---

## 3. Statistical Summary Observations

9. Many feature distributions have means close to zero due to sparsity in descriptors such as HOG and LBP.
10. The median of many features is zero, confirming heavy left-skewness across the dataset.
11. Maximum feature values often reach 1.0, consistent with normalized feature extraction.
12. Feature ranges vary significantly, indicating that scaling is essential for PCA or other distance-based models.
13. The variability across features differs substantially, reinforcing the need for normalization.
14. The `label` column is numeric-encoded, with statistical values confirming correct data type usage rather than meaningful numeric behavior.
15. `cell_number` appears uniformly distributed between 1 and 64, validating grid-based decomposition of each image.

---

## 4. Class Distribution and Imbalance Insights

16. The dataset exhibits extreme class imbalance: approximately 91.47 percent of samples belong to the `no_object` class.
17. Minority classes (`ball`, `bat`, and `stump`) collectively account for less than 9 percent of the dataset.
18. This imbalance poses a major challenge for supervised model training.
19. The imbalance directly explains early models' tendency to predict `no_object` overwhelmingly.
20. The distribution complicates EDA visualizations, as minority samples appear sparsely throughout the dataset.

---

## 5. Feature Quality: Zero Variance Checks

21. Two features (`f446` and `f447`) displayed zero variance across samples.
22. Such features offer no utility for learning and can safely be removed.
23. The low number of zero-variance features indicates that the majority of descriptors contain at least some signal.

---

## 6. Feature Range and Scaling Analysis

24. All features fall within the 0–1 range, confirming normalized extractions.
25. The ratio between maximum and minimum feature ranges is mathematically infinite due to features with zero minimum, reinforcing the necessity for scaling.
26. The dataset is appropriate for PCA, t-SNE, SVM, and other algorithms that assume normalized or standardized data.

---

## 7. Outlier Detection Findings

27. Outlier analysis revealed that approximately 1,408 features have more than 5 percent outliers.
28. About 334 features exhibit more than 10 percent outliers.
29. The feature with the highest outlier percentage (`f460`) reached 24.7 percent.
30. Overall outlier presence is moderate but cannot be ignored.
31. RobustScaler is recommended as an alternative to StandardScaler when handling outlier-sensitive models.

---

## 8. Feature Correlation and Redundancy

32. Correlation heatmaps reveal no strong pairwise correlations between features.
33. The lack of significant correlation indicates that the extracted descriptors capture diverse visual aspects.
34. Low correlation also implies difficulty in applying correlation-based feature selection.
35. High-dimensional datasets with low redundancy typically benefit from PCA for compression.

---

## 9. Multicollinearity Analysis (VIF)

36. Several features show high Variance Inflation Factor (VIF) values, with some exceeding 10, indicating multicollinearity.
37. High multicollinearity can destabilize linear models and inflate variance.
38. PCA is suitable here because interpretability of individual features is not required.
39. Features with moderate VIF values (between 3 and 10) indicate structural redundancy.
40. Multicollinearity is expected due to the inclusion of multiple similar texture and gradient descriptors.

---

## 10. Principal Component Analysis (PCA) Findings

41. PCA analysis confirms that the dataset is high-dimensional and sparse.
42. Capturing 90 percent variance requires 938 principal components.
43. Capturing 95 percent variance requires 1,223 components, representing a 67 percent dimensionality reduction.
44. Capturing 99 percent variance requires 2,101 components, showing long-tail variance contribution.
45. The final PCA-transformed dataset has shape (27,200, 1,223).
46. PCA reduces memory footprint and mitigates multicollinearity.
47. The first few principal components do not correlate strongly with the labels, indicating global rather than object-specific variance.
48. Later principal components contain necessary discriminative information due to the diffuse structure of the original features.

---

## 11. PCA Component Insights

49. PCA visualizations reveal overlapping clusters across classes.
50. PCA preserves variance structure but does not optimize for class separation.
51. The large number of significant components reflects the heterogeneous nature of image-derived features.
52. PCA captures meaningful structure but does not inherently support classification due to overlap.

---

## 12. t-SNE Visualization Insights

53. t-SNE projections reveal between three to six loosely formed clusters.
54. The clusters do not align cleanly with the four target classes.
55. The majority class (`no_object`) forms a dense central region overshadowing minority class clusters.
56. Minority classes exhibit significant overlap, suggesting weak discriminative power in the existing feature set.
57. t-SNE results reinforce the need for improved feature extraction or additional contextual information.

---

## 13. SMOTE + Tomek Links Analysis

58. Before resampling, the dataset exhibited extreme imbalance with minority classes under 10 percent combined.
59. SMOTE expanded the dataset from 27,200 to 99,524 samples, a 266 percent increase.
60. After resampling, each class contains exactly 24,881 samples.
61. Tomek Links removed ambiguous samples from decision boundaries after oversampling.
62. The substantial number of synthetic samples indicates sparse locality in feature space.
63. SMOTE assumes linear interpolation between samples is meaningful, which may not hold in high-dimensional CV features.
64. Resampling improves class balance numerically but does not inherently correct poor feature separability.
65. Oversampling exposes potential structural weaknesses in the feature representation.

---


