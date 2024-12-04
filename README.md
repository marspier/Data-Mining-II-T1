# Data-Mining-II-Task 2


# Uncovering the Factors That Separate Customers: A Demographic and Usage-Based Analysis
This project uses Principal Component Analysis (PCA) to identify key customer characteristics and preferences that differentiate distinct customer segments. By simplifying complex data, PCA enables the creation of targeted marketing strategies, helping to better engage and retain customers.

### Key Highlights:
- **Methodology**: Employed K-Means clustering algorithm to categorize customers based on usage patterns.
- **Outcomes**: Provided actionable insights to inform targeted marketing strategies and enhance customer engagement.

Part 1: Research Question:

A1. What are the main factors that separate customers based on their demographics and usage data?

A2.  The goal is to find the customer characteristics and preferences  that differentiate distinct customer segments.This will help create better marketing strategies to keep customers interested and engaged. This is reasonable because PCA allows us to reduce the complexity of the data. “Principal Component Analysis (PCA) reduces the complexity of a dataset by condensing information from many different features into just a few representative features. It is especially helpful when the dataset has a very large number of features(Virtualitics, 2021).” In this scenario, using PCA will help us find the key factors that define customer differences, making it easier to create targeted marketing strategies.

Part II: Technique Justification

B1. K-means clustering works by dividing customers into groups based on how similar they are to one another, using the variables like tenure, income, and monthly charges.
    i. As an exploratory method, PCA helps us understand the structure of data by reducing its complexity. It does this by transforming original variables (such as income, tenure, monthly charges, and churn) into new variables called principal components (PCs). Therefore, PCA simplifies the data to focus on the most important patterns, without losing variance. “PCA reduces the number of variables in your data, making analyzing and interpreting data easier. This dimensionality reduction keeps the most crucial information while discarding less important details” (ChartExpo).
    The explanation of PCA steps was based on a detailed guide on Principal Component Analysis provided by BuiltIn. The  PCA process begins by standardizing all variables so they are scaled to the same range. For example, income can have a larger range of numbers compared to churn. Without standardization, the variable with the larger range may skew the results and give it more influence than variables with smaller ranges.Then, a covariance matrix is created to observe correlations between variables. In other words, it helps identify whether there is a linear relationship between them. Finally, eigenvectors and eigenvalues are calculated from the covariance matrix to determine the principal components, which represent the directions of maximum variance in the data. This highlights the directions in which the data varies the most.These steps are needed to  identify significant patterns and reduce the data’s dimensionality.PCA also aids in feature selection by identifying variables with low eigenvalues. We can then choose to drop variables that are less important and leave the variables that  capture the majority of the information.The expected outcome of PCA is to uncover hidden patterns or relationships that were not obvious before. Once we gain a better understanding of the main factors that affect  churn we can then help inform decision-making.

 
B2. One key assumption of PCA is that the dataset should have continuous variables, meaning the features (like income or monthly charges) should be measured on a numerical scale.According to Jaadi , “PCA transforms a dataset of continuous variables into a new set of uncorrelated variables called ‘principal components’. The first principal component derived explains the largest amount of the variability in the underlying dataset. The second principal component derived explains the second largest amount of variability in the dataset and so on”(Jaadi).
Therefore, if the dataset has categorical variables (like gender or state), they will need to be converted into numerical form or handled separately.


Part III Data Preparation

C1. For my analysis, the continuous variables I will use include:
      Population
      Age
      Income
      Tenure
      Monthly Charge
      Bandwidth_GB_Year
      Children
      Outage_sec_perweek
These variables will be used to form the dataset for PCA

C2.  Standardize the continuous data set variables identified in part C1. 


**List of continuous variables to standardize**
   columns_to_standardize = ['Population', 'Age', 'Income', 'Tenure', 'Monthly Charge', 'Bandwidth_GB_Year', 'Children', 'Outage_sec_perweek']
 **Fit and transform the data using StandardScaler**
   scaler = StandardScaler()
   standardized_data = scaler.fit_transform(df[columns_to_standardize]) 
**Convert the standardized data back to a DataFrame**
   standardized_df = pd.DataFrame(standardized_data, columns=columns_to_standardize) 
**Normalize the standardized data using MinMaxScaler**
   min_max_scaler = MinMaxScaler() 
   norm_data = min_max_scaler.fit_transform(standardized_df) 
**Convert the normalized data back to a DataFrame**
   scaled_df = pd.DataFrame(norm_data, columns=standardized_df.columns) 
**Save to CSV**
   scaled_df.to_csv('Scaled_df.csv', index=False) 
   scaled_df.head()
 
Part IV: Analysis
D1. PCA 
1.  Determine the matrix of all the principal components.
**Perform PCA to fit the data**
    pca = PCA() pca.fit(scaled_df[columns_to_standardize])
    PC_df = pd.DataFrame(data=pca_result, columns=['PC1', 'PC2'])
    loading_matrix = pd.DataFrame(pca.components_, columns=columns_to_standardize, index=['PC1', 'PC2'])


    PC_df.head()


2. Total number of principal components, using the elbow rule or the Kaiser criterion. 
**Kaiser Criterion**


  eigenvalues = pca.explained_variance_


**Apply the Kaiser criterion**
  kaiser_criterion = eigenvalues > 1
  number_of_components_to_keep = np.sum(kaiser_criterion)


  print("Eigenvalues:", eigenvalues)
  print("Components to retain (Kaiser Criterion):", number_of_components_to_keep)
  Eigenvalues: [1.99362072 1.0422924 ]
  Components to retain (Kaiser Criterion): 2


**Scree Plot**


  pcomp = np.arange(1, len(eigenvalues) + 1)




  plt.figure(figsize=(13, 6))
  plt.plot(pcomp, eigenvalues, 'b-', marker='o')  # Blue line with markers
  plt.title('Scree Plot (Kaiser Criterion)', fontsize=16)
  plt.xlabel('Number of Components', fontsize=16)
  plt.ylabel('Eigenvalues', fontsize=16)
  plt.axhline(y=1, color='g', linestyle='--')  # Green dashed line at y=1
  plt.grid()
  plt.show()






**Elbow Method**
  explained_variance = pca.explained_variance_ratio_
**Scree Plot**
  plt.figure(figsize=(10, 6))
  plt.plot(range(1, len(explained_variance) + 1), explained_variance, marker='o')
  plt.title('Elbow Method for Optimal Number of Components')
  plt.xlabel('Number of Components')
  plt.ylabel('Explained Variance Ratio')
  plt.grid()
  plt.axhline(y=0.1, color='r', linestyle='--')  # Optional: Add a horizontal line for reference
  plt.show()

3.  Identify the variance of each of the principal components identified in part D2.


    The variance of each of the principal components identified in part D2 is shown by the eigenvalues of the principal components. Based on the output of the PCA, the variances of the principal components are:
    PC1: The variance captured by the first principal component is 1.9936.
    PC2: The variance captured by the second principal component is 1.0423.
    These eigenvalues indicate the amount of variance each component captures from the dataset, with PC1 capturing the largest portion of the variance. 
4.  Identify the total variance captured by the principal components identified in part D2.
    These eigenvalues indicate that PC1 captures more variance than PC2, with PC1 explaining more of the underlying variability in the data. The explained variance ratio is [0.24917767, 0.13027352]; in terms of percentages, it is 24.9% and 13.0%. This means that 24.9% of all the variability in the dataset can be explained by PC1 and 13% by PC2.Based on Saturn Cloud,  “Explained variance ratio is a measure of the proportion of the total variance in the original dataset that is explained by each principal component. The explained variance ratio of a principal component is equal to the ratio of its eigenvalue to the sum of the eigenvalues of all the principal components”(Saturn). 
5.  Summary of my results

    Based on the Kaiser criterion, it shows that I kept  two principal components, and  each had an eigenvalue above 1 ( as seen above under D2.  in figure 2) The first principal component (PC1) has an eigenvalue of 1.99, which explains 24.9% of the total variance, and the second component (PC2) has an eigenvalue of about 1.04, explaining 13% of the variance. Together, these two components explain 37.9% of the total variance in the dataset, which is a reasonable amount. The total variance is 3.036 therefore the two principal components captured around 37.9% of the total variance in the 3.036.  
    Looking at the loading matrix, PC1 is strongly linked to Tenure and Bandwidth_GB_Year, meaning these variables tend to change together in the data. PC2 has high values for Children, Age, and Income, which capture demographic details about the customers. For Age, the loading is high but negative, showing an opposite trend. The loadings for Children and Income are positive, showing they tend to increase together. This shows that age, number of children, and income levels are related.
Under Figure 3 (in D2), we can observe that the smaller range on the PC1 axis and the larger range on the PC2 axis indicate that PC1 has less variation, while PC2 has greater variation. Therefore, PC1 captures more specific patterns in the data, whereas PC2 captures broader, more diverse patterns. When combined with the results from the loading matrix, we can infer that PC1 reflects more specific factors related to Tenure and Bandwidth usage, while PC2 captures a wider range of patterns linked to Age, Income, and Children.
Under Figure 4 (in D2) it’s showing at what percentage the cumulative  variance is being explained by each principal component. 



Biostatsquid. "PCA Simply Explained." Biostatsquid, 2023, https://biostatsquid.com/pca-simply-explained/.


Kamara, K. (2024). PCA data preprocessing. Panopto. https://wgu.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=b094cb1f-ba6f-40c1-a033-b140010a4314


Kamara, K. (2024). Create PCA object and analyze data. Panopto. https://wgu.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=d656d2c9-9409-41e0-a01c-b14001099afa 


"What Is a Principal Component Analysis?" ChartExpo, https://chartexpo.com/blog/what-is-a-principal-component-analysis#. Accessed 14 Nov. 2024.


 Jaadi, Zakaria. "Step-By-Step Explanation of Principal Component Analysis." Built In, 22 Aug. 2023, https://builtin.com/data-science/step-step-explanation-principal-component-analysis.


 Saturn Cloud. (2023, July 6). What is sklearn PCA explained variance and explained variance ratio difference? Retrieved from https://saturncloud.io/blog/what-is-sklearn-pca-explained-variance-and-explained-variance-ratio-difference/


Virtualitics. "Reducing Dataset Complexity Using Principal Component Analysis (PCA) Desktop." Virtualitics, 2021, https://docs.virtualitics.com/hc/en-us/articles/23735407780243-Reducing-Dataset-Complexity-Using-Principal-Component-Analysis-PCA-Desktop. Accessed 14 Nov. 2024.

