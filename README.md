<h1 align="center">ETL Building for an E-commerce Jeans Company</h1>

<p align="center">A Data Engineering Project</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/66283452/209023968-1effb930-5759-4694-89d6-8cecc3ee5db1.png" width=550 />
</p>

*Obs: The company and business problem are both fictitious, although the data is real.*

*The in-depth Python code explanation is available in [this](https://github.com/brunodifranco/project-star-jeans-data-engineering/blob/main/star-jeans.ipynb) Jupyter Notebook.*

# 1. **Star Jeans and Business Problem**
<p align="justify"> Michael, Franklin and Trevor, after several successful businesses, are starting new a company called Star Jeans. For now, their plan is to enter the USA fashion market through an E-commerce. The initial idea is to sell one product for a specific audience, which is <b>male jeans</b>. Their goal is to keep prices low and slowly increase them, as they get new clients. However, this market already has strong competitors, such as H&M for instance. In addition to that, the three businessmen aren't familiar with this segment in particular. Therefore, in order to better understand how this market works they hired a Data Science/Engineering freelancer, to gather information regarding H&M. They want to know the following information about H&M male jeans: </p>

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

- <b> Extraction </b>: Scraping product_id and product_type in showroom page (Job 01); getting other attributes from each product and saving it all in Pandas DataFrame (Job 02). More information in <a href="https://github.com/brunodifranco/project-star-jeans-data-engineering#3-extraction">Section 3</a>.</p>

- <b> Transformation </b>: Data Cleaning (Job 03). More information in <a href="https://github.com/brunodifranco/project-star-jeans-data-engineering#4-transformation">Section 4</a>.</p>

- <b> Loading </b>: Inserting data in a PostgreSQL Database (Job 04). More information in <a href="https://github.com/brunodifranco/project-star-jeans-data-engineering#5-loading">Section 5</a>.</p>

- <b> Streamlit App </b>: Loading Database in the Streamlit App (Job 05); displaying data and adding filters in the Streamlit App (Job 06). More information in <a href="https://github.com/brunodifranco/project-star-jeans-data-engineering#6-streamlit-app">Section 6</a>.</p>

[Here](https://docs.google.com/spreadsheets/d/1ipHa7oxNVYF1zpFfDz5yG63RP0GvpRLFOzBozBHIdRA/edit?usp=sharing) you can find the full ETL documentation, and below there's an illustration showing the complete ETL process and dynamic: 

<p align="center">
  <img src="https://user-images.githubusercontent.com/66283452/208748904-f7ada2f7-8ced-4bbd-a473-85102fab9c5e.png"/>
</p>

<p align="justify"> All jobs are performed sequentially. Jobs 01-04 are being run by <a href="https://github.com/brunodifranco/project-star-jeans-data-engineering/blob/main/star-jeans-etl/webscraping-hm.py">this</a> script, while the Streamlit App (Jobs 05 and 06) is built by <a href="https://github.com/brunodifranco/project-star-jeans-data-engineering/blob/main/star-jeans-etl/streamlit-app/star-jeans-app.py">this</a> script. Jobs 01-04 are scheduled to run on a weekly basis via Windows Task Scheduler, which makes the Streamlit App (Jobs 05 an 06) also updates in the same period frequency, since Job 05 loads the Database in Streamlit, after it's been processed by the ETL. </p>

## 2.2. Tools and techniques used:
- [Python 3.10.8](https://www.python.org/downloads/release/python-3108/), [Pandas](https://pandas.pydata.org/) and [Beautiful Soup](https://beautiful-soup-4.readthedocs.io/en/latest/).
- [SQL](https://www.w3schools.com/sql/) and [PostgresSQL](https://www.postgresql.org/).
- [Jupyter Notebook](https://jupyter.org/) and [VSCode](https://code.visualstudio.com/).
- [Web Scraping](https://towardsdatascience.com/a-step-by-step-guide-to-web-scraping-in-python-5c4d9cef76e8).
- [ETL Process](https://www.keboola.com/blog/etl-process-overview) and [Windows Task Scheduler](https://www.windowscentral.com/how-create-automated-task-using-task-scheduler-windows-10).
- [Streamlit](https://streamlit.io/).
- [Git](https://git-scm.com/) and [Github](https://github.com/).

# 3. **Extraction**
<p align="justify"> The extraction is made by scraping the H&M male jeans webpage, using Python and Beautiful Soup library. This process is divided in two jobs: </p>

- <p align="justify"> <b> Job 01 </b>: Gathering the web page link and product id for each product in the showroom, as well as the product type, since this information isn't available in each individual product page. Then, the product id is split in style id (first 7 digits) and color id (last 3 digits) for latter merging. </p>

- <p align="justify"> <b> Job 02 </b>: Getting other attributes from each product and saving it all in Pandas DataFrame. These other attributes were product name, fit, color, composition and price. In addition to that, a scraping_datetime variable is added every time this process is executed, showing when the scraping was done.  </p>

<i> Job 02 is by far the most time consuming in terms of script running, out of all Jobs. </i>

# 4. **Transformation**
<p align="justify"> After the full raw table is available it needs to be cleaned, which is <b>Job 03</b>. Firstly, all column names are set to snake case style, and all rows from product_color, product_fit, product_name and product_price are also set to snake case style. </p>

<p align="justify"> The most difficult column to fix is the product_composition, since it's split in another six columns: cotton, spandex, elastomultiester, lyocell, rayon. Each one of these columns indicates how much, in percentage terms, they contribute to the product's composition. Finally the duplicated values are dropped and columns are rearranged. The final table definition is as follows: </p>

<div align="center">

| **Column**          | **Definition** |
|:--------------------:|----------------|
|      product_id     | A 10-digit number uniquely assigned to each product, composed of style_id and color_id |
|      style_id       | A 7-digit number uniquely assigned to each product style| 
|      color_id       | A 3-digit number assigned to each product color|
|      product_name   | Product's name |
|      product_type   | Product's type |
|      product_color  | Product's color |
|      product_fit    | Product's fit - if it's slim, skinny, loose, etc |
|      cotton         | Percentage of cotton in the product's composition |
|      spandex        | Percentage of spandex in the product's composition |
|      polyester      | Percentage of polyester in the product's composition |
|    elastomultiester | Percentage of elastomultiester in the product's composition |
|       lyocell       | Percentage of lyocell in the product's composition |
|       rayon         | Percentage of rayon in the product's composition |
|       product_price | Product's unit price |
|  scraping_datetime  | The Date of which the data scraping was performed |

</div>

# 5. **Loading**
<p align="justify">After the data is cleaned, the script inserts it in a PostgreSQL Database using Python's SQLAlchemy library. For this project, a free Database is being used from <a href="https://neon.tech/">Neon.tech</a>. This whole process is the <b>Job 04</b>. 
  
<i> It's important to notice that the new data is always being appended to the database, not replaced, so it can be possible to spot differences in prices for the same product over time. </i> </p>

# 6. **Streamlit App**
<p align="justify"> Streamlit was the chosen application to display the data since it's easy to create interactive tools, such as filters for instance. In addition to that, its deployment can be made directly through Streamlit Cloud itself, not requiring another Cloud. Once the data is added in the PostgreSQL database, the Streamlit App has two jobs: </p>
  
- <p align="justify"> <b> Job 05 </b>: Insert the data from the PostgreSQL Database to Streamlit. </p>  
- <p align="justify"> <b> Job 06 </b>: Displaying data in a table and adding interactive filters to it. </p>    

<b> Click here to access the App:</b> [![Streamlit App](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=Streamlit&logoColor=white)](https://star-jeans.streamlit.app/)

# 7. **Conclusion**
In this project the main objective was accomplished:

 <p align="justify"> <b> We managed to create an ETL process that extracts data from H&M, a Star Jeans competitor, cleans it, and saves it to a PostgreSQL database on a weekly basis. Then, the database can be added and displayed with filters in a Streamlit App, where it can be accessed from anywhere by Star Jeans' owners, so they can have a better understanding on how the USA male jeans market works. </b> </p>
  
# 8. **Next Steps**
<p align="justify"> Further on, this solution could be improved by using <a href="https://airflow.apache.org/">Apache Airflow</a> instead of Windows Task Scheduler to automate the ETL process, since with Windows Task Scheduler the computer must be on for the script to run. </p>

# Contact

- brunodifranco99@gmail.com
- [![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/BrunoDiFrancoAlbuquerque/)
