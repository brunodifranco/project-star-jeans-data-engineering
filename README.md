# **WORK IN PROGRESS**



improvments:
- Airflow


<h1 align="center">Customer loyalty program for E-commerce</h1>

<p align="center">A Data Engineering Project</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/66283452/204150853-c732bdf8-1651-4eb2-a27b-8bfc2fdfc02a.png" alt="drawing" width="450" height="550"/>
</p>

*Obs: The company and business problem are both fictitious, although the data is real.*

*The in-depth Python code explanation is available in [this](https://github.com/brunodifranco/project-outleto-clustering/blob/main/notebooks/outleto.ipynb) Jupyter Notebook.*

# 1. **Outleto and Business Problem**
<p align="justify"> Michael, Franklin and Trevor, after several successful businesses, are starting new a company called Star Jeans. For now, their plan is to enter the USA fashion market through an E-commerce. The initial idea is to sell one product for a specific audience, which is male jeans. Their goal is to keep prices low and slowly increase them, as they get new clients. However, this market already has strong competitors, such as H&M for instance. In addition to that, the three businessmen aren't familiar with this segment in particular. Therefore, in order to better understand how this market works they hired a Data Science/Engineering freelancer, to gather information regarding H&M. Star Jeans wants to know the following information on H&M male jeans: </p>

- Product Name
- Product Type
- Product Fit
- Product Color
- Product Composition
- Product Price </p>

# 2. **Solution Plan**
## 2.1. How was the problem solved?

<p align="justify"> We managed to gather the information from H&M male jeans by creating an ETL, which consists of following steps (jobs): </p>

- <b> Understanding the Business Problem</b>: Understanding the main objective we are trying to achieve and plan the solution to it. 

- <b> Extraction </b>: Scraping product_id and product_type in showroom page (Job 01); getting other attributes from each product and saving it all in Pandas DataFrame (Job 02). More information in <a href="https://github.com/brunodifranco/project-outleto-clustering#5-feature-engineering">Section 5</a>.</p>

- <b> Transformation </b>: Data Cleaning (Job 03). More information in <a href="https://github.com/brunodifranco/project-outleto-clustering#5-feature-engineering">Section 5</a>.</p>

- <b> Loading </b>: Inserting data in PostgreSQL Database (Job 04). More information in <a href="https://github.com/brunodifranco/project-outleto-clustering#5-feature-engineering">Section 5</a>.</p>

- <b> Streamlit App </b>: Loading Database in Streamlit App (Job 05); displaying data and adding filters in Streamlit App (Job 06). More information in <a href="https://github.com/brunodifranco/project-outleto-clustering#5-feature-engineering">Section 5</a>.</p>

[Here](https://docs.google.com/spreadsheets/d/1ipHa7oxNVYF1zpFfDz5yG63RP0GvpRLFOzBozBHIdRA/edit?usp=sharing) you can find the full ETL documentation, and below there's an illustration showing the complete ETL process and dynamic: 

<p align="center">
  <img src="https://user-images.githubusercontent.com/66283452/208748904-f7ada2f7-8ced-4bbd-a473-85102fab9c5e.png" alt="drawing" />
</p>



## 2.2. Tools and techniques used:
- [Python 3.10.8](https://www.python.org/downloads/release/python-3108/), [Pandas](https://pandas.pydata.org/) and [Beautiful Soup](https://beautiful-soup-4.readthedocs.io/en/latest/).
- [Jupyter Notebook](https://jupyter.org/) and [VSCode](https://code.visualstudio.com/).
- [ETL Process](https://www.keboola.com/blog/etl-process-overview)
- [Web Scraping](https://towardsdatascience.com/a-step-by-step-guide-to-web-scraping-in-python-5c4d9cef76e8)
- [Streamlit](https://streamlit.io/)
- [SQL](https://www.w3schools.com/sql/) and [PostgresSQL](https://www.postgresql.org/).
- [Git](https://git-scm.com/) and [Github](https://github.com/).

# 3. **Extraction**
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

# 4. **Transformation**
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

# 5. **Loading**

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

# 6. **Streamlit App**

<p align="justify"> The final business report was built using Power BI, containing answers to the following questions previously demanded by Outleto's Marketing Team. This is how the report was built: </p>
  
- Firstly, a new PostgreSQL database was created in Render Cloud.
- Then, the final data containing all customers already classified in their respective clusters was saved in this PostgreSQL database.
- Continuing, the database was added in Power BI, making it possible to create the final business report, where it was saved in PDF format.
- Finally, the report was uploaded to Google Drive, so it could be shared.
  
<b> Click here to access the report: </b>[![Google Drive](https://img.shields.io/badge/Google%20Drive-4285F4?style=for-the-badge&logo=googledrive&logoColor=white)](https://drive.google.com/file/d/1Ys3bbyh2evWvzbG5mXlM6MrH-z6neXmb/view)

<p align="justify"> <b> Whereas for the list of customers that made it to insiders it was saved in CSV format, and it's available <a href="https://github.com/brunodifranco/project-outleto-clustering/blob/main/lists/insiders_list.csv">here</a> </b>. </p>
   
<i> The complete list of Outleto's customers is also available for download <a href="https://github.com/brunodifranco/project-outleto-clustering/blob/main/lists/full_list.csv">here</a> </i>. 
 
# 7. **Conclusion**
In this project the main objective was accomplished:

 <p align="justify"> <b> We managed to provide a business report using Power BI, containing answers to the questions previously demanded by Outleto's Marketing Team, as well as a list of eligible customers to be a part of Insiders. With that report the Marketing Team will promote actions to each cluster, in order to increase revenue, but of course focusing mostly in the Insiders cluster, since they represent 53.5% of the total gross revenue. In addition to that, some useful business insights were found. </b> </p>
  
# 8. **Next Steps**
<p align="justify"> Further on, this solution could be improved by a few strategies:
  
- Requesting more data from Outleto, such as product associated data. 
  
- Creating even more features. 

- Making the final report even more automatic for when new data comes in, so it could be run every time requested and the data instantly saved in the PostgreSQL database. This could be done by using the Papermill library.  </p>

# Contact

- brunodifranco99@gmail.com
- [![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/BrunoDiFrancoAlbuquerque/)
