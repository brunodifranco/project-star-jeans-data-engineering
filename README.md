# project-star-jeans-data-engineering
WORK IN PROGRESS
![etl](https://user-images.githubusercontent.com/66283452/208748904-f7ada2f7-8ced-4bbd-a473-85102fab9c5e.png)
https://docs.google.com/spreadsheets/d/1ipHa7oxNVYF1zpFfDz5yG63RP0GvpRLFOzBozBHIdRA/edit?usp=sharing

improvments:
- Airflow


<h1 align="center">Customer loyalty program for E-commerce</h1>

<p align="center">A clustering project</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/66283452/204150853-c732bdf8-1651-4eb2-a27b-8bfc2fdfc02a.png" alt="drawing" width="450" height="550"/>
</p>

*Obs: The company and business problem are both fictitious, although the data is real.*

*The in-depth Python code explanation is available in [this](https://github.com/brunodifranco/project-outleto-clustering/blob/main/notebooks/outleto.ipynb) Jupyter Notebook.*

# 1. **Outleto and Business Problem**
<p align="justify"> Outleto is a multibrand outlet company, meaning it sells second line products of various companies at lower prices, through an E-commerce platform. Outleto's Marketing Team noticed that some customers tend to buy more expensive products, in higher quantities and more frequently than others, therefore contributing to a higher share of Outleto's total gross revenue. Because of that, the Marketing Team wishes to launch a customer loyalty program, dividing the 5,702 customers in clusters, on which the best customers will be placed in a cluster named Insiders. </p>

<p align="justify"> To achieve this goal, the Data Science Team was requested to provide a list of customers that will participate in Insiders, as well as a business report regarding the clusters, answering the following questions: </p>
  
##### 1) **How many customers will be a part of Insiders?**
##### 2) **How many clusters were created?**
##### 3) **How are the customers distributed amongst the clusters?**
##### 4) **What are these customers' main features?**
##### 5) **What's the gross revenue percentage coming from Insiders? And what about other clusters?**
##### 6) **How many items were purchased by each cluster?**

With that list and report the Marketing Team will promote actions to each cluster, in order to increase revenue, but of course focusing mostly in the Insiders cluster.

# 2. **Data Overview**
The data was collected from Kaggle in the CSV format. The initial features descriptions are available below:

<div align="center">
  
| **Feature**          | **Definition** |
|:--------------------:|----------------|
|       InvoiceNo      | A 6-digit integral number uniquely assigned to each transaction |
|       StockCode      | Product (item) code | 
|       Description    | Product (item) name |
|       Quantity       | The quantities of each product (item) per transaction |
|       InvoiceDate    | The day when each transaction was generated |
|       UnitPrice      | Unit price (product price per unit) |
|       CustomerID     | Customer number (unique id assigned to each customer) |
|       Country        | The name of the country where each customer residers |
  
</div>

# 3. **Business Assumptions**

- All observations on which unit_price <= 0 were removed, as we're assuming those are gifts when unit_price = 0, and when unit_price < 0 it's described as "Adjust bad debt".  
- Some stock_code identifications weren't actual products, therefore they were removed.  
- Both description and country columns were removed, since those aren't relevant when modelling.  
- <p align="justify">Customer number 16446 was removed because he (she) bought 80995 items and returned them in the same day, leading to extraordinary values in other features. Other 12 customers were removed because they returned all items bought. In addition to that, three other users were also removed because they were considered to be data inconsistencies, since they had their return values greater than quantity of items bought, which doesn't make sense. These 16 were named "bad users".<p>

# 4. **Solution Plan**
## 4.1. How was the problem solved?

<p align="justify"> To provide the clusters final report the following steps were performed: </p>

- <b> Understanding the Business Problem</b>: Understanding the main objective we are trying to achieve and plan the solution to it. 

- <b> Collecting Data</b>: Collecting data from Kaggle.

- <b> Data Cleaning</b>: Checking data types, treating Nan's, renaming columns, dealing with outliers and filtering data.

- <b> Feature Engineering</b>: Creating new features from the original ones, so that those could be used in the ML model. More information in <a href="https://github.com/brunodifranco/project-outleto-clustering#5-feature-engineering">Section 5</a>.</p>

- <p align="justify"> <b> Exploratory Data Analysis (EDA)</b>: Exploring the data in order to obtain business experience, look for data inconsistencies, useful business insights and find important features for the ML model. This was done by using the <a href="https://pypi.org/project/pandas-profiling/">Pandas Profiling</a> library. Two EDA profile reports are available for download <a href="https://github.com/brunodifranco/project-outleto-clustering/tree/main/pandas-profiling-reports"> here</a>, one still with the bad users and one without them. 

- <b> Data Preparation</b>: Applying <a href="https://www.atoti.io/articles/when-to-perform-a-feature-scaling/">Rescaling Techniques</a> in the data.

- <b> Feature Selection</b>: Selecting the best features to use in the Machine Learning algorithm.

- <b> Space Analysis and Dimensionality Reduction</b>: <a href="https://builtin.com/data-science/step-step-explanation-principal-component-analysis">PCA</a>, <a href="https://umap-learn.readthedocs.io/en/latest/">UMAP</a> and <a href="https://gdmarmerola.github.io/forest-embeddings/">Tree-Based Embedding</a> were used to get a better data separation. 

- <p align="justify"> <b> Machine Learning Modeling</b>: Selecting the number of clusters (K) and then training Clustering Algorithms. More information in <a href="https://github.com/brunodifranco/project-outleto-clustering#6-machine-learning-modeling">Section 6</a>.</p>

- <b> Model Evaluation</b>: Evaluating the model by using Silhouette Score and Silhouette Visualization.

- <b>Cluster Exploratory Data Analysis</b>: Exploring the clusters to obtain business experience and to find useful business insights. In addition to that, this step also helped building the business report. The top business insights found are available in <a href="https://github.com/brunodifranco/project-outleto-clustering#7-top-business-insights"> Section 7</a>. 

- <p align="justify"> <b>Final Report and Deployment</b>: Providing a business report regarding the clusters, as well as a list of customers that will participate in Insiders. This report was built using <a href="https://powerbi.microsoft.com/pt-br/">Power BI</a>, as well as <a href="https://render.com/">Render Cloud</a> and <a href="https://www.google.com/intl/pt-BR/drive/">Google Drive</a>, so that it could be accessed from anywhere. More information in <a href="https://github.com/brunodifranco/project-outleto-clustering#8-final-report-and-deployment"> Section 8</a>.</p>

## 4.2. Tools and techniques used:
- [Python 3.10.8](https://www.python.org/downloads/release/python-3108/), [Pandas](https://pandas.pydata.org/), [Matplotlib](https://matplotlib.org/), [Seaborn](https://seaborn.pydata.org/), [Sklearn](https://scikit-learn.org/stable/), [SciPy](https://scipy.org/) and [Pandas Profiling](https://pypi.org/project/pandas-profiling/).
- [SQL](https://www.w3schools.com/sql/) and [PostgresSQL](https://www.postgresql.org/).
- [Jupyter Notebook](https://jupyter.org/) and [VSCode](https://code.visualstudio.com/).
- [Power BI](https://powerbi.microsoft.com/pt-br/). 
- [Render Cloud](https://render.com/) and [Google Drive](https://www.google.com/intl/pt-BR/drive/).
- [Git](https://git-scm.com/) and [Github](https://github.com/).
- [Exploratory Data Analysis (EDA)](https://towardsdatascience.com/exploratory-data-analysis-8fc1cb20fd15). 
- [Techniques for Feature Selection](https://machinelearningmastery.com/feature-selection-with-real-and-categorical-data/).
- [Clustering Algorithms (K-Means,  Gaussian Mixture Models, Agglomerative Hierarchical Clustering and DBSCAN)](https://towardsdatascience.com/the-5-clustering-algorithms-data-scientists-need-to-know-a36d136ef68).

# 5. **Feature Engineering**
In total, 10 new features were created by using the original ones: 

<div align="center">
  
| **New Feature** | **Definition** |
|:--------------------:|----------------|
| Gross Revenue | Gross revenue for each customer, which is equal to quantity times unit price |
| Average Ticket | Average monetary value spent on each purchase |
| Recency Days | Period of time from current time to the last purchase | 
| Max Recency Days | Max time a customer's gone without making any purchases |
| Frequency | Average purchases made by each customer during their latest purchase period and first purchase period |
| Purchases Quantity | Amount of times a customers's made any purchase |
| Quantity of Products | Total of products purchased |
| Quantity of Items | Total quantity of items purchased |
| Returns | Amount of items returned |
| Purchased and Returned Difference | Natural log of difference between items purchased and items returned | 
  
</div>

# 6. **Machine Learning Modeling**
<p align="justify"> In order to get better data separation a few dimensionality reduction techniques were tested: PCA, UMAP and Tree-Based Embedding. The Results were satisfactory with Tree-Based Embedding, which consists of: </p>

- Setting gross_revenue as a response variable, so it becomes a supervised learning problem.
- Training a Random Forest (RF) model to predict gross_revenue using all other features.
- Plotting the embedding based on RF's leaves.

In total four clustering algorithms were tested, for a cluster number varying from 2 to 24:
- K-Means
- Gaussian Mixture Models (GMM) with Expectation–Maximization (EM)
- Agglomerative Hierarchical Clustering (HC)
- DBSCAN 

<p align="justify"> The models were evaluated by silhouette score, as well as clustering visualization. Our maximum cluster number was set to 11 due to practical purposes for Outleto's Marketing Team, so they can come up with exclusive actions for each cluster. DBSCAN had its parameters optimized with Bayesian Optimization, however, because it provided a very high number of clusters it was withdrawn as a possible final model. The results were quite similar for K-Means, GMM and HC with clusters from 8 to 11, however <b>KMeans</b> were chosen with <b>8</b> clusters because its silhouette score is slightly better, being equal to 0.6168. Those 8 cluster names are <b> Insiders, Runners Up, Promising, Potentials, Need Attention, About to Sleep, At Risk </b> and <b> About to Lose </b>.
  
## <i>Metrics Definition</i>
There're two properties we look for when creating clusters:

- <b>Compactness</b>: Observations from the same cluster must be compact with one another, meaning the distance between them should be as small as possible.

- <b>Separation</b>: Different clusters must be as far apart from one another as possible.

<b>Silhouette Score</b> covers both these properties. It goes from -1 to 1, the higher the better.

# 7. **Top Business Insights**

 - ### 1st - Customers from Insiders are responsible for 58.3% of the total items purchased.
<p align="center">
  <img src="https://user-images.githubusercontent.com/66283452/204071720-ab1d3d7c-e603-48e7-a13e-1cc0e97a4436.png" alt="drawing" width="850"/>
</p>

--- 
- ### 2nd - Customers from Insiders are responsible for 53.5% of the total gross revenue.
<p align="center">
  <img src="https://user-images.githubusercontent.com/66283452/204071721-5af58f13-3bcc-420c-87a8-7a5ececfe2a2.png" alt="drawing" width="850"/>
</p>

--- 

- ### 3rd - Customers from Insiders have a number of returns higher than other customers, on average.
<p align="center">
  <img src="https://user-images.githubusercontent.com/66283452/204071722-6f5827cc-a4f8-465e-81ae-ee10981caf62.png" alt="drawing" width="850"/>
  
<i>That's probably because those customers buy a really high amount of items.</i>  </p>
  
---

# 8. **Final Report and Deployment**

<p align="justify"> The final business report was built using Power BI, containing answers to the following questions previously demanded by Outleto's Marketing Team. This is how the report was built: </p>
  
- Firstly, a new PostgreSQL database was created in Render Cloud.
- Then, the final data containing all customers already classified in their respective clusters was saved in this PostgreSQL database.
- Continuing, the database was added in Power BI, making it possible to create the final business report, where it was saved in PDF format.
- Finally, the report was uploaded to Google Drive, so it could be shared.
  
<b> Click here to access the report: </b>[![Google Drive](https://img.shields.io/badge/Google%20Drive-4285F4?style=for-the-badge&logo=googledrive&logoColor=white)](https://drive.google.com/file/d/1Ys3bbyh2evWvzbG5mXlM6MrH-z6neXmb/view)

<p align="justify"> <b> Whereas for the list of customers that made it to insiders it was saved in CSV format, and it's available <a href="https://github.com/brunodifranco/project-outleto-clustering/blob/main/lists/insiders_list.csv">here</a> </b>. </p>
   
<i> The complete list of Outleto's customers is also available for download <a href="https://github.com/brunodifranco/project-outleto-clustering/blob/main/lists/full_list.csv">here</a> </i>. 
 
# 9. **Conclusion**
In this project the main objective was accomplished:

 <p align="justify"> <b> We managed to provide a business report using Power BI, containing answers to the questions previously demanded by Outleto's Marketing Team, as well as a list of eligible customers to be a part of Insiders. With that report the Marketing Team will promote actions to each cluster, in order to increase revenue, but of course focusing mostly in the Insiders cluster, since they represent 53.5% of the total gross revenue. In addition to that, some useful business insights were found. </b> </p>
  
# 10. **Next Steps**
<p align="justify"> Further on, this solution could be improved by a few strategies:
  
- Requesting more data from Outleto, such as product associated data. 
  
- Creating even more features. 

- Making the final report even more automatic for when new data comes in, so it could be run every time requested and the data instantly saved in the PostgreSQL database. This could be done by using the Papermill library.  </p>

# Contact

- brunodifranco99@gmail.com
- [![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/BrunoDiFrancoAlbuquerque/)
