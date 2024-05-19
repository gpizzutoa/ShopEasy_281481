# ShopEasy AI Model

## Team Members
- Gianfranco Pizzuto 281481
- Santiago Cuesta 289581
- Petar Jelusic 286581


## Section 1: Introduction
Briefly describe your project here.

## Section 2: Methods (**EDA & Data Pre-Processing**)
Describe your proposed ideas, such as features, algorithms, training overview, design choices, etc., and your environment here. Ensure that:
- A reader can understand why you made your design decisions.
- A reader should be able to recreate your environment (include commands like `conda list`, `conda env export`, etc.).
- Consider including a figure illustrating your ideas, like a flowchart of your machine learning system. (Place it in the "images" folder and link it here using `![Flowchart Description](images/flowchart.png)`).


### **Visualizing and Understanding Data Structure & Values**

#### 1. Checking Data Integrity

First, we examined the dataset for missing values to ensure its completeness, checking for missing values and duplicates.

**Observations:**
- **Columns with Missing Data**:
  - `maxSpendLimit`: Only 1 missing value (0.01% of the total), which is negligible.
  - `leastAmountPaid`: 313 missing values (3.50% of the total). This higher percentage might still not significantly impact our analysis of payment behavior.
  - **Overall Data Completeness**: Most columns have a very low percentage of missing values or none at all, because of the low percentage we will drop those columns. We also observed there where no duplicates.

#### 2. Dropping Unnecessary Values & Columns

  - We removed the `personId` column since it's a primary key and not relevant for our analysis.
  -  We dropped rows that had any missing values across the DataFrame (after removing `personId`).

- This resulted in a new, clean DataFrame which we referred to as `ndf`.
- After cleaning, we re-checked for missing values to confirm that no missing data remained. All was good!

#### 3. Describing the New DataFrame

We then summarized the new DataFrame to understand the distribution and range of values in each column.

*Observations:* `leastAmountPaid` has a maximum value greater than the `accountTotal` suggesting potential errors or outliers.

### **Univariate Analysis**

#### 4. Distribution Graph

We began by examining the distribution of individual variables within the DataFrame to understand their spread and identify any potential skewness or outliers.

<img src="images/histograms.png" alt="Distribution Graph" width="600"/>


**Observations from the Data:**
- **Right-Skewed Distributions**: `accountTotal`, `itemCosts`, `maxSpendLimit`
- **Left-Skewed Distributions**: `frequencyIndex`, `accountLifespan`
- **Bimodal Distribution**: `itemBuyFrequency`
- **Uniform Distribution**: `webUsage`


#### Box Plots

To identify and analyze outliers, we utilized box plots for various features.

**Observations from Box Plots:**

- **High Variation**:
  - Many box plots exhibited a significant number of outliers.
  - These outliers indicate substantial variation within certain features.

- **Next Steps**:
  - Before delving deeper into outlier analysis, we will first select the most appropriate features for clustering.

*(Insert box plots for key features to illustrate the presence of outliers and variation)*

---

### **Bivariate Analysis**

#### Objective

We aimed to examine relationships between pairs of variables with similar values from the box plots to understand their interactions better.

#### Pairs Analyzed

- **itemCosts and singleItemCosts**
- **itemBuyFrequency and multipleItemBuyFrequency**

#### Outliers

We identified significant outliers in the dataset:
- **leastAmountPaid**: Outliers reach values around 80,000, which is significantly higher than the highest values in `monthlyPaid` or `accountTotal`.
- **monthlyPaid**: The description "Total amount paid by the user every month" is vague. It is unclear whether this refers to a fixed amount or an average.

#### Anomalies

We observed anomalies in the data:
- **leastAmountPaid vs. accountTotal**: `leastAmountPaid` has higher values than `accountTotal`, which is illogical. The least amount paid in a single transaction should not exceed the total amount spent since registration.

#### Next Steps

To further investigate any missed relationships or anomalies, we will conduct a multivariate analysis.

#### ScatterPlot & Correlation

##### itemBuyFrequency vs. multipleItemBuyFrequency

**Analysis of Relationship Between itemBuyFrequency and multipleItemBuyFrequency**

We observed a strong positive relationship between these two variables. This is expected as their definitions are closely related:
- **itemBuyFrequency**: Frequency of items purchased by the user.
- **multipleItemBuyFrequency**: Frequency of items bought in multiple installments by the user.

To quantify this relationship, we calculated their correlation.

- **Correlation Result**: The correlation is very strong (0.86).
  - **Interpretation**: This high correlation suggests that users frequently opt for multiple installments, resulting in a close alignment between the overall buying frequency and the frequency of installment purchases.
  - **Alternative Explanation**: Purchases made in installments tend to be frequent, thereby contributing to the strong correlation.

**Key Points:**
- The scatter plot visually confirms the strong positive relationship.
- The correlation coefficient quantifies this relationship, indicating a value of 0.86.
- Users often choose multiple installments, or frequent installment purchases align closely with the overall buying frequency, leading to this high correlation.

*(Insert scatter plot here showing the relationship between itemBuyFrequency and multipleItemBuyFrequency)*

##### itemCosts vs. singleItemCosts

**Analysis of Relationship Between itemCosts and singleItemCosts**

We observed a strong positive relationship between these two variables. This is expected as their definitions are closely related:
- **itemCosts**: Total costs of items purchased by the user.
- **singleItemCosts**: Costs of items that the user bought in a single purchase without opting for installments.

To quantify this relationship, we calculated their correlation.

- **Correlation Result**: The correlation is very strong (close to 1).
  - **Interpretation**: This high correlation suggests that most users prefer single purchases without multiple installments, leading to a high overlap between total costs and single-item costs.
  - **Alternative Explanation**: Single purchases tend to have higher values compared to multiple installment purchases, contributing to the strong correlation.

**Key Points:**
- The scatter plot visually confirms the strong positive relationship.
- The correlation coefficient quantifies this relationship, indicating a value close to 1.
- Users typically opt for single purchases without installments or the value of single purchases is higher, leading to this high correlation.

*(Insert scatter plot here showing the relationship between itemCosts and singleItemCosts)*

##### itemCosts vs. multipleItemCosts

We also checked the correlation between `itemCosts` and `multipleItemCosts`, finding it to be moderately strong at 0.68. This indicates a significant relationship between total costs and costs of items bought in multiple installments.

*(Insert scatter plot here showing the relationship between itemCosts and multipleItemCosts)*

#### Anomaly between the two relationships

**Analysis of Relationship Between accountTotal and leastAmountPaid**

We observed a moderate positive relationship between these two variables:
- **accountTotal**: Total amount in the account.
- **leastAmountPaid**: Least amount paid on the account.

To quantify this relationship, we calculated their correlation.

- **Correlation Result**: The correlation is moderate (0.40).
  - **Interpretation**: This correlation suggests that while there is a relationship between the total amount in the account and the least amount paid, it is not very strong. Users with higher account totals do not always pay higher amounts, but there is still a noticeable tendency.
  - **Alternative Explanation**: Higher account totals might be associated with higher payments to some extent, thereby contributing to the moderate correlation.

**Key Points:**
- The scatter plot visually shows the moderate positive relationship.
- The correlation coefficient quantifies this relationship, indicating a value of 0.40.
- There is a moderate alignment between the total amount in the account and the least amount paid, leading to this moderate correlation.

*(Insert scatter plot here showing the relationship between accountTotal and leastAmountPaid)*

#### Integrity Issue in Account Data

We observed that numerous users have values for `leastAmountPaid` that are higher than the values for `accountTotal`, which should not be possible. This discrepancy suggests an inconsistency in the data that needs to be addressed to ensure accurate analysis.

#### Using Categorical Values

We analyzed the `location` and `accountType` to determine if there is a predominant category. Both categories appeared to have an equal distribution.

*(Insert visualizations here showing the distribution of location and accountType)*

#### KDE (Kernel Density Estimate)

**Analysis of Relationship Between itemBuyFrequency and multipleItemBuyFrequency**

We observed a strong positive relationship between these two variables:
- **itemBuyFrequency**: Frequency of items purchased by the user.
- **multipleItemBuyFrequency**: Frequency of items bought in multiple installments by the user.

To quantify this relationship, we calculated their correlation.

- **Correlation Result**: The correlation is very strong (0.86).
  - **Interpretation**: This high correlation suggests that users frequently opt for multiple installments, resulting in a close alignment between the overall buying frequency and the frequency of installment purchases.
  - **Alternative Explanation**: Purchases made in installments tend to be frequent, thereby contributing to the strong correlation.

**Key Points:**
- The scatter plot visually confirms the strong positive relationship.
- The correlation coefficient quantifies this relationship, indicating a value of 0.86.
- Users often choose multiple installments, or frequent installment purchases align closely with the overall buying frequency, leading to this high correlation.

The KDE plots indicated that the distribution of itemBuyFrequency is similar across different account types and locations. This suggests that neither accountType nor location significantly affects the distribution of other features.

*(Insert KDE plots here for itemBuyFrequency across accountType and location)*

Based on this observation, we determined that the location feature might not be relevant for understanding customer buying habits and behavior. In an e-commerce context, where most transactions occur online, the customer's physical location is less significant.

#### PairPlot with accountType

We used a pair plot to investigate the potential correlation between accountType and other key features that might influence customer buying habits and behavior. 

Using the pair plot, we could not visualize a clear correlation between `accountType` and any other variable.

*(Insert pair plot here showing the relationship between accountType and other features)*

### Label Encoding

**Encoding for accountType**

- **Rationale**:
  - We applied label encoding to `accountType` because it can be considered ordinal, where the order of categories is meaningful.

- **Ordering**:
  - **Premium** `0`: Pays the most.
  - **Regular** `1`: Pays a standard amount.
  - **Student** `2`: Pays the least due to discounts.

This encoding reflects the hierarchy of payment amounts among different account types.

**Encoding for location**

- **Data Type**:
  - Location values are nominal, meaning they have no inherent order.

- **Encoding Choice**:
  - We avoided label encoding to prevent implying any natural order.
  - Instead, we used dummy variables, where each category is represented by a binary vector.

This approach ensures that the encoding accurately reflects the nature of the location data.

*(Insert table or example of encoded variables here)*

---
Sure, here is a detailed markdown description for the correlation heatmap section with clear explanations and suggestions for including graphs:

---

### **Correlation Heatmap**

Using this heatmap, we'll visualize the correlation between all attributes to observe any correlations that we might have missed.

#### Correlation Heatmap Analysis

- **Location**:
  - No correlation with other attributes.
  - Location is not relevant for our analysis as it doesn't affect customer behavior on the e-commerce website.
  - *Drop location feature.*

- **Account Type**:
  - Relevant to our analysis despite no correlation with other features in the heatmap.
  - *Keep accountType feature.*

- **Irrelevant or Redundant Features (To Be Dropped)**:
  - **accountTotal**:
    - Inconsistent values compared to `itemCosts` and `monthlyPaid`.
    - Possibly represents non-monetary value, different currency, or inaccurate data.
  - **leastAmountPaid**:
    - No correlation with other features.
    - Inconsistent values compared to `monthlyPaid`.
  - **monthlyPaid**:
    - Inconsistencies with `itemCosts`.
    - No additional insights over `itemCosts`.
  - **webUsage** and **frequencyIndex**:
    - Little to no correlation with other features.
    - Similar definitions to `itemBuyFrequency`, which is relevant.
  - **paymentCompletionRate**:
    - Lack of correlation with other features.
    - Provides no significant insights.
  - **maxSpendLimit**:
    - Inconsistencies with `singleItemCosts`.
    - Definition conflicts with actual values.
  - **singleItemBuyFrequency** and **multipleItemBuyFrequency**:
    - More specific versions of `itemBuyFrequency`.
    - Can be generalized by `itemBuyFrequency`.
  - **singleItemCosts** and **multipleItemCosts**:
    - More specific versions of `itemCosts`.
    - Can be generalized by `itemCosts`.
  - **emergencyUseFrequency**:
    - High correlation with `emergencyCount`.
    - Redundant feature.

*(Insert correlation heatmap here to visualize the correlations among all features)*

### **Dropping Values**

We created a new variable, Final Data Frame (`fdf`), in which we dropped all the columns listed above.

### **Removing Outliers**

To address outliers, we removed values that are more than 4 standard deviations from the mean. Below, we identify and remove these outliers, ensuring we don't lose too much data.
- **Outliers Removed**: The number of rows removed is 536, which is relatively small and can be considered negligible. Therefore, we proceed with the removal process.

*(Insert histogram or box plot here showing the distribution before and after outlier removal)*

### **Data Scaling**

Since the attribute values have significantly different ranges, we scaled the data as a general practice to normalize the data.

*(Insert a table or summary showing the range of values before and after scaling)*

### **Correlation Heatmap for Cleaned & Scaled Data Frame**

After cleaning and scaling the data, we generated a new correlation heatmap to ensure the data is now ready for further analysis.

*(Insert correlation heatmap for the cleaned and scaled data frame here)*

---


## Section 3: Experimental Design
Describe the experiments conducted to validate the target contributions of your project:

### Purpose
1-2 sentence high-level explanation of each experiment.

### Baselines
Describe the methods used for comparison.

### Evaluation Metrics
List which metrics were used and why.

## Section 4: Results
Describe your main findings and conclusions drawn from the work:

- Main findings: report final results here.
- Include placeholder figures and tables to communicate findings (use Markdown image and table syntax).

Ensure that all figures containing results are generated from the code.

## Section 5: Conclusions
Concluding remarks:

- Summarize the key take-away from your work in one paragraph.
- Discuss any questions not fully answered by your work and potential future directions.

# main.ipynb

Ensure that your `main.ipynb` notebook includes:

- Alternating text and code cells from start to finish.
- Descriptions above each code cell explaining the intent and choice of the code below.
- Explanations of the outputs of each cell, especially figures, such that a reader understands what they are about to see.

# Additional Resources

## Images Folder
All figures displayed in the `README.md` should be stored in the `images` folder.


# Project Description: ShopEasy
Imagine a platform named ShopEasy, a leading e-commerce site that sells a variety of products, from 
books and gadgets to furniture and fashion. Over the years, they have amassed a vast amount of user 
data. This data is a gold mine of insights waiting to be discovered. ShopEasy aims to provide 
personalized user experiences, special promotions, and improved services. But to do this effectively, 
they first need to understand the buying habits and behaviors of their customers. By applying 
segmentation to this dataset, ShopEasy aims to uncover these hidden patterns and provide an 
enhanced, personalized shopping experience for its users.

## Dataset Features
- **personId:** Unique identifier for each user on the platform
- **accountTotal:** Total amount spent by the user on ShopEasy since their registration
- **frequencyIndex:** Reflects how frequently the user shops, with 1 being very frequent and values less than 1 being less frequent
- **itemCosts:** Total costs of items purchased by the user
- **singleItemCosts:** Costs of items that the user bought in a single purchase without opting for installments
- **multipleItemCosts:** Costs of items that the user decided to buy in installments
- **emergencyFunds:** Amount that the user decided to keep as a backup in their ShopEasy wallet for faster checkout or emergency purchases
- **itemBuyFrequency:** Frequency with which the user makes purchases
- **singleItemBuyFrequency:** How often the user makes single purchases without opting for installments
- **multipleItemBuyFrequency:** How often the user opts for installment-based purchases
- **emergencyUseFrequency:** How frequently the user taps into their emergency funds
- **emergencyCount:** Number of times the user has used their emergency funds
- **itemCount:** Total number of individual items purchased by the user
- **maxSpendLimit:** The maximum amount the user can spend in a single purchase, set by ShopEasy based on user's buying behavior and loyalty
- **monthlyPaid:** Total amount paid by the user every month
- **leastAmountPaid:** The least amount paid by the user in a single transaction
- **paymentCompletionRate:** Percentage of purchases where the user has paid the full amount
- **accountLifespan:** Duration for which the user has been registered on ShopEasy
- **location:** User's city or region
- **accountType:** The type of account held by the user. Regular for most users, Premium for those who have subscribed to ShopEasy premium services, and Student for users who have registered with a student ID
- **webUsage:** A metric (0-100) indicating the frequency with which the user shops on ShopEasy via web browsers. A higher number indicates more frequent web usage

## Assignment
- **Perform an Explanatory data analysis (EDA)** with visualization using the entire dataset.
- **Preprocess the dataset** (remove duplicates, encode categorical features with one hot encoding, not necessarily in this order).
- **Define the problem type** (regression, classification, or clustering), explain your choice, and design your model accordingly. Test at least 2 different models.
- **Identify the proper number of segments**, and evaluate different options.
- **Describe the properties of the segments** you have identified.
- **Describe the properties of the customers** belonging to each segment.





Certainly! Here is the markdown description for your machine learning models section, organized and formatted for clarity:

---

## **ML MODELS**

### Description

**Why This is a Clustering Problem**

- **Data Characteristics**:
  - The dataset includes both numerical and categorical data, ideal for segmentation and clustering.

- **Not Regression**:
  - Regression predicts continuous outcomes, which is not the focus here.

- **Not Classification**:
  - Classification involves supervised learning with predefined categorical outcomes. Our goal is to uncover clusters, not use predefined ones.

- **Clustering Approach**:
  - Clustering is used in unsupervised learning to discover patterns and group customers into clusters without predefined outcomes.

The objective is to identify natural groupings within the data to enhance the user experience through personalized recommendations.

### **K-Means++**

#### Description

**What is K-means++?**

K-means++ is an enhanced initialization method for the K-means clustering algorithm. Instead of randomly selecting initial centroids like the standard K-means algorithm, K-means++ carefully selects initial centroids to be more spread out. Here’s how we will do it:

1. **First Centroid:** Select the first centroid randomly from the data points.
2. **Subsequent Centroids:** For each subsequent centroid, select a data point from the remaining data points with a probability proportional to its squared distance from the nearest existing centroid.
3. **Repeat:** Continue this process until we have chosen \( k \) centroids.

**Why is K-means++ Good for This Problem?**

- **Better Initialization:** K-means++ ensures that initial centroids are spread out, reducing the chances of poor initialization that could lead to suboptimal clustering results.
- **Faster Convergence:** With better initial centroids, K-means++ will often converge faster than standard K-means, reducing the number of iterations required.
- **Improved Clustering Quality:** By starting with well-separated centroids, K-means++ increases the likelihood of finding clusters that better represent the inherent structure of the data.

For our clustering analysis on customer purchasing behavior, K-means++ will be particularly beneficial because:
- It helps identify distinct customer segments more accurately.
- It reduces the variability in clustering results, leading to more consistent and reliable insights.
- It enhances the performance and efficiency of the clustering process, making it suitable for large datasets.

Overall, K-means++ is a robust choice for our clustering tasks, providing a solid foundation for segmenting customers effectively and deriving meaningful business insights.

#### Choosing the Optimal Number of Clusters

##### **Elbow Method**

**Purpose**: To determine the ideal number of clusters for K-means clustering.

**Inertia**: Measures how close values in a cluster are to their centroid (within-cluster sum of squares). Inertia decreases as the number of clusters increases.

**Procedure**:
- Run K-means with different numbers of clusters (k).
- Plot inertia (y-axis) against the number of clusters (x-axis).
- The "elbow point," where inertia starts to decrease more slowly, indicates the optimal number of clusters. Beyond this point, adding more clusters offers diminishing returns.

*(Insert Elbow Method graph here showing inertia vs. number of clusters)*

This method suggests a cluster number of 3 or 4 given the graph.

##### **Silhouette Score**

- **Usage**: We will also use the Silhouette score to determine the appropriate number of clusters, taking into account the graph.
- **Function**: Measures how similar an object is to its own cluster compared to other clusters.
- **Importance**: Helps in identifying distinct clusters and minimizing overlap.

Here we see that using 3 clusters is significantly better than 4. We also do not expect a large number of clusters because it is less relevant in a clustering problem such as this one.

**Rationale**:
- **Customer Segmentation**: The goal is to identify general customer types, making the elbow method a practical choice.
- **Complex Data**: Using the Silhouette score alone might suggest more clusters than necessary, leading to overfitting and making clusters less interpretable.
- **Insightful Analysis**: Both methods combined provide a balanced approach, ensuring meaningful and manageable clusters without focusing on subtle, non-essential differences in complex, high-dimensional data. Because of this, we will use `3` clusters.

*(Insert Silhouette Score graph here showing the scores for different numbers of clusters)*

#### Running the Model & Visualization

Once we have determined the optimal number of clusters, we run the K-means++ model and visualize the results.

##### Scatter Plot

*(Insert scatter plot here showing the clusters in a 2D space)*

##### Pie Graph

*(Insert pie chart here showing the proportion of each cluster)*

##### Mean Visualization

*(Insert bar chart or table here showing the mean values of key features for each cluster)*

---

### **DBSCAN**

#### **Description**

**What is DBSCAN?**

DBSCAN (Density-Based Spatial Clustering of Applications with Noise) is a clustering algorithm that groups together points that are closely packed together while marking points that lie alone in low-density regions as outliers. Here’s how we will do it:

1. **Core Points:** We will identify core points, which have at least a minimum number of neighbors (MinPts) within a specified radius (epsilon).
2. **Directly Reachable:** We will consider points that are within the epsilon radius of a core point as directly reachable.
3. **Density Reachable:** We will find points that are reachable from core points through a chain of other core points.
4. **Cluster Formation:** We will form clusters from connected core points and their reachable points.
5. **Noise Identification:** Points that are not reachable from any core point will be marked as noise or outliers.

**Why is DBSCAN Good for This Problem?**

- **Discovering Arbitrarily Shaped Clusters:** DBSCAN allows us to find clusters of arbitrary shapes, which is useful if our customer behavior patterns do not form clear, spherical clusters.
- **Handling Noise:** DBSCAN effectively identifies and handles outliers in the data, which is beneficial for detecting anomalies in customer behavior.
- **No Need for Predefined Number of Clusters:** Unlike K-means, DBSCAN does not require us to specify the number of clusters in advance, making it flexible for exploring the natural structure of our data.

For our clustering analysis on customer purchasing behavior, DBSCAN will be particularly beneficial because:
- It helps us identify natural clusters in the data without assuming a specific number of clusters.
- It enables us to detect outliers or anomalies in customer behavior, which can be critical for understanding unusual spending patterns.
- It allows us to discover clusters of varying shapes and sizes, providing a more nuanced understanding of customer segments.

Overall, DBSCAN is a powerful tool for our clustering tasks, allowing us to uncover meaningful patterns and insights from customer purchasing behavior while also identifying and managing outliers effectively.

#### **Finding the Optimal EPS & Min Samples**

##### Optimal `EPS`

To determine the best value for `eps`, we will use the k-nearest neighbors (k-NN) distance plot. This plot helps us visualize the distances between points and identify the "elbow" point where the distance starts to increase more rapidly, indicating a suitable value for `eps`.

**Analyzing the k-NN Distance Plot**

From the k-NN distance plot, we can observe an "elbow" point, where the distances start to increase more rapidly. This elbow point suggests a good candidate for the `eps` value. Based on the plot, we will choose `eps` to be slightly above the elbow point.

In this case, the elbow appears to be around `eps = 1.17`, which we will use as our `eps` value for the DBSCAN algorithm.

*(Insert k-NN distance plot here to show the elbow point)*

##### Finding the Optimal `min_samples`

The `min_samples` parameter represents the minimum number of points required to form a dense region. A common heuristic is to set `min_samples` to be at least the dimensionality of the dataset plus one. We will experiment with different values around this heuristic to find the optimal `min_samples`.

**Setting the Initial `min_samples`**

Based on the dimensionality heuristic, we set `min_samples` to the number of features plus one. In this case, the dataset has a dimensionality of 10, so we set `min_samples` to 11. We can adjust this value later based on the clustering results.

*(Insert explanation or table showing the process of choosing min_samples)*

#### **Running the Model & Visualization**

With the chosen `eps` and `min_samples` values, we will run the DBSCAN clustering algorithm on our scaled data. We will then analyze the resulting clusters and visualize them using PCA for a 2D plot.

##### Scatterplot

*(Insert scatter plot here showing the clusters in a 2D space using PCA)*

##### Pie Graph

*(Insert pie chart here showing the proportion of each cluster, including noise)*

##### Mean Visualization

*(Insert bar chart or table here showing the mean values of key features for each cluster)*

---
