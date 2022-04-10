# Data Engineering: Pipelines on the Cloud from scratch

## Used tools
 **Python 3.8**
 **MySQL workbench**
 **SQLAlchemy (connect python to SQL)**
 **AWS (Amazon Relational Database Service - Amazon RDS, Lambda function)**
 **APIs: City by API-Ninjas, AeroDataBox, OpenWeather Map**
 
## Project description
 Gans is a startup developing an e-scooter-sharing system. It aspires to operate in the most populous cities all around the world. In each city, it will have hundreds of e-scooters parked in the streets and allows users to rent them by the minute.Gans has seen that its operational success depends on having their scooters parked where users need them.Ideally, scooters get rearranged organically by having certain users moving from point A to point B, and then an even number of users moving from point B to point A. There are some elements that create asymmetries, like:

* In the morning, there is a general movement from residential neighbourhoods towards the city centre.
* Whenever it starts raining, e-scooter usage decreases drastically.
* Whenever airplanes with back-pack young tourists lands, a lot of scooters are needed close to the airport. 

 The task will be to collect data from external sources that can potentially help Gans predict e-scooter movement. Since data is needed every day, in real time and accessible by everyone in the company, the challenge is going to be to assemble and automate a data pipeline in the cloud. 
In a nut shell, I collected data regarding population, weather, and arrivals. However I used only one city (Berlin, DE), the scripts can be easily adapted to colect data for more than one city with a for loop. 

## Data collection 

### Demographycal data 
 I have gethered data for the 12th most populated cities in Germany using an API called "City by API-Ninjas" from rapidapi.com. 

### Weather forecast
 I used Open Weather Map API to forecast weather for 5 days on every 3rd hour for Berlin. 
 
### Arrival flights info
 Rapidapi.com served me once again, this time to get info for the arrival flights in berlin on a daily basis. I used the AeroDataBox API. Since this API is not allowing to get data for further than 12 hours I did th eprocedure twice: firts to get data for the flights between 00:00 - 12:00 and the second time for the flights between 12:00 - 24:00. After that I combined both DataFrames into one.
 
## Set up a local database: 

 * Open MySQL Workbench and create a ne schema into your local conection
 
 * Use the SQLAlchemy libary to establish a conection between python and MySQL
 
      import sqlalchemy

      schema="name of the scheme"
      host="127.0.0.1"
      user="root"
      password="your password for MySQL"
      port=3306
      con = f'mysql+pymysql://{user}:{password}@{host}:{port}/{schema}'
  
 * Import and update the tables to the MySQL
 
      df.to_sql('german_cities', con=con, if_exists='append', index=False)
      
 The method pandas.DataFrame.to_sql() will convert a DataFrame into a MySQL table in a single step. If the table does not exist yet in your database, it will be created, and you will not even have to worry about data types —they will be inferred from the existing data types in the DataFrame. If the table exists in your database it will append the current DataFrame to it. 
 **NOTE:** This method will couse duplicates if there is a repetetive information within both DataFrames. 

## Pipelines on the cloud 

### Set up a cloud MySQL instance: 

  * Sign up for an AWS account 
  * Set up an Amazon RDS MySQL Instance
  * Connect to your Amazon RDS MySQL Instance
  * Allow all traffic to your database
  
 **Note:** Follow the step-by-step guidance in folder "Guides"
 
### Move the scripts to the cloud: 

  * Create a “Role”. This role will allow your Lambda function to connect to your RDS instance
  * Create a Lambda function
  * Upload a Layer(s) with Python modules to your Lambda function
  * Connect your Lambda function to your RDS instance
  * Implement the Python scripts to the Lambda function
  
 **Note:** Follow the step-by-step guidance  in folder "Guides"
 
### Automate the data pipeline

 One can set triggers with EventBridge (CloudWatch Events).I set two triggers: one for the arrivals each day and one for the weather every fifth day. 
 
 **Note:** I used cron expression generator. Just have in mind that AWS is not having seconds.
 
 ## ATTENTION: you have to use your own API keys, passwords and hosts to run the scrips!!!
