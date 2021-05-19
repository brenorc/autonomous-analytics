# Oracle Analytics Cloud Tutorial

This guide was built to be used as a complement to the Oracle Analytics Cloud Hand-on Practice provided by Oracle Alliances & Channels. Not every step will be detailed here, but you can use it to check the main activities and replicate some labs in the future. I am always open to suggestions on how the workshops can be improved. Reach me at breno.comin@oracle.com if you want to talk about it.

## Oracle Analytics Quick-Tour

Before our hands-on practices we will have a quick overview of the features available on Oracle Analytics Cloud, for both versions Professional and Enterprise.

- How to provision OAC
- Home Page Key Points
- OAC Features Overview
- BICS Features Overview 

## Oracle Data Visualization

Oracle DV is part of our Oracle Analytics Cloud solution. It is responsible for generating a visual interactive interface for end-users to explore and dive into data, bring insights to every kind of users - from developers to business users.

Oracle releases also a Desktop version which anyone can download for free from the Oracle Technology Network: [Data Visualization Desktop](https://www.oracle.com/middleware/technologies/oracle-data-visualization-desktop.html). There is total interoperability between the OnPremises and the Cloud version, and projects can be created on one of them and further moved to another.

You will find a lot of features to explore on Data Visualization, which are mostly covered by those 2 free courses created by Oracle:

[Modern Data Visualization with Oracle Analytics Cloud](https://www.udemy.com/course/augmented-analytics/)

[Augmented Data Visualization with Machine Learning](https://www.udemy.com/course/machinelearning-analytics/)

### Data Visualization Exercise 

For our Data Viz exercise, we will load a .dva file into the platform and explore some visualization options. In the **Home Page**, click the **upper-right menu** and choose the **Import Project** button.

![Import Project](https://i.imgur.com/YMGZM6v.png)

You will shortly receive a success message if everything goes alright.

![ImportSuccess](https://i.imgur.com/5gAkU62.png)

A .dva file does not contain exclusively a Data Visualization project. It may contain as well other assets such as Datasets, Data Flows, Connections to the data sources, etc. They will also be imported to the cloud platform as you will can notice on the examples.

![ImportedProj](https://i.imgur.com/aqUaGn6.png)

The project can be found under the **Projects tab** in the hamburger menu or will be automatically displayed at the **Home Page**. Open it and you will see a canvas where you can begin exploring your data:

![Canvas](https://i.imgur.com/wvHU8hF.png)

To create a new visualization you just have to drag-and-drop information from the list of columns available in the dataset. Let's try to bring information about **Sales and Product Container**. 

![DragDrop](https://i.imgur.com/3rmu70V.png)

By bringing both of them to the blank area, OAC generates a Sales by Product Container report. Simple as that.

![DataViz](https://i.imgur.com/ThXp3FK.png)

### The Challenge

This is a sales history dataset full of transaction data. From the available variables, your challenge is to answer these questions:

- **How many product units were ordered in 2014? How did this number evolved in the lat years?**
- **Which Product Sub Categories bring more profit to the company?  Which of them have the higher Profit Ratios (Profit/Sales)?**
- **In which countries are we selling the most? Can we see this information on a map?**

By answering these question the user is able to know superficially core Oracle Data Visualization concepts, such as filtering, dashboarding, creating calculations and more.



## Connecting to Autonomous Data Warehouse

Oracle Autonomous Database is the most innovative cloud-native database available on the market today. This exercise will show every step needed to connect to this Data Source.

### Creating an Autonomous Database

Access https://cloud.oracle.com and login with the credentials provided by the trainer

![Cloud_login](https://i.imgur.com/c6aRDg5.png)



Select Autonomous Data Warehouse in the Upper-left menu (aka hamburger menu) 

![Cloud_login2](https://i.imgur.com/ibxQ1tO.png)

Create a new database. You will have to choose:

(For this example you can keep the default values)

- The cloud compartment where it will be located.
- The database name and its display name.
- The workload type (ATP or ADW)
- The deployment type (Serverless or Dedicated)
- The CPU count and Storage
- The password for the admin account
- The license type (BYOL or New License)

![Cloud_login3](https://i.imgur.com/nP4sAHl.png)

Every time we connect to the Autonomous Database we need to use the Wallet file. This is a file containing the credentials needed to create a connection to the ADB.

Open your **Autonomous Data Warehouse** and click **Database Connection**. This will open a window where you are able to download the credentials. Save them as a .zip file.

![img](https://i.imgur.com/BraZog5.png)



### Creating the connection

First of all, login into your Oracle Analytics Cloud account.

Click on **Create -> Connection**:

![image-20201027142612170](https://i.imgur.com/mbnLHH9.png)

Choose the **Oracle Autonomous Data Warehouse** connection type:

![image-20201027143440775](https://i.imgur.com/HXU9EwK.png)

Fill all the blanks with information related to your database environment: 

- A name for your connection
- The credential (just upload the wallet file)
- A Username (Default user for ADB is admin)
- A Password (Defined at ADB creation)
- A service name (May be High, Medium or Low)

![image-20201027143823812](https://i.imgur.com/RCKxfiC.png)

You should receive a confirmation message like the one below:

![image-20201027144254476](https://i.imgur.com/Y7gSyQj.png)

Now it is time to **Create** a **Data Set**

![image-20201027144349740](https://i.imgur.com/n4mZlLu.png)

Choose the connection that was just created

![image-20201027144431726](https://i.imgur.com/6yfzs4Y.png)

Choose your Schema and Tables, and **Click the Add Button**

![image-20201027144536833](https://i.imgur.com/wQcbYWD.png)

After that we can explore all the data included on the database using the fundamentals we just learned on the Data Visualization section!



## Dimensional Modeling

First of all, we will connect our Autonomous Database instance to Oracle Analytics Cloud. Although we have already done it on the previous steps, if we want to be able to  access the ADW via Classic Home / BI Modeler the Data Visualization connection isn't sufficient.

### Connection creation

So you have to go to the Home Page, and reach the connection area:

Menu > Console > Configuration and Administration > Connections

![1559653929693](C:\Users\bcomin\AppData\Roaming\Typora\typora-user-images\1559653929693.png)

Then, we will create a JDBC connection as the admin user, by clicking on the **Create** button. The TNS Descriptor used to connect to the Database can be found inside the wallet file, on **tnsnames.ora**.

If the connection creation is successful, you will see the new connection listed in the connections page.

![1559653985523](C:\Users\bcomin\AppData\Roaming\Typora\typora-user-images\1559653985523.png)

### Data Loading into ADWC

It is also necessary to create Create the DW tables and insert some rows before working with the Data Modeler. For that, you can directly paste the following SQL queries on SQL Developer Web.

```sql
--------------------------------------------------------
--  DDL for Table CLOUD_D_CUSTOMERS
--------------------------------------------------------

  CREATE TABLE CLOUD_D_CUSTOMERS
   ("CUST_BIRTH_DATE" DATE, 
	"CUST_CREDIT_RATE" NUMBER, 
	"CUST_GENDER" VARCHAR2(20 BYTE), 
	"CUST_MARITAL_STATUS" VARCHAR2(20 BYTE), 
	"CUST_NAME" VARCHAR2(255 BYTE), 
	"CUST_NUMBER" NUMBER, 
	"CUST_SEGMENT" VARCHAR2(255 BYTE), 
	"CUST_TYPE" VARCHAR2(255 BYTE)
   ) PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 NOCOMPRESS LOGGING
  STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645
  PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1 BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)
  TABLESPACE "SYSTEM" ;
--------------------------------------------------------
--  DDL for Table CLOUD_D_GEOGRAPHY
--------------------------------------------------------

  CREATE TABLE CLOUD_D_GEOGRAPHY
   (	"ADDRESS1" VARCHAR2(100 BYTE), 
	"ADDRESS2" VARCHAR2(100 BYTE), 
	"ADDR_KEY" NUMBER, 
	"CITY" VARCHAR2(100 BYTE), 
	"COUNTRY_CODE" VARCHAR2(3 BYTE), 
	"COUNTRY_NAME" VARCHAR2(100 BYTE), 
	"POSTAL_CODE" VARCHAR2(12 BYTE), 
	"STATE_PROV" VARCHAR2(40 BYTE)
   ) PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 NOCOMPRESS LOGGING
  STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645
  PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1 BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)
  TABLESPACE "SYSTEM" ;
--------------------------------------------------------
--  DDL for Table CLOUD_D_PRODUCTS
--------------------------------------------------------

  CREATE TABLE CLOUD_D_PRODUCTS
   (	"PRODUCT" VARCHAR2(100 BYTE), 
	"PROD_BRAND" VARCHAR2(100 BYTE), 
	"PROD_ITEM_DSC" VARCHAR2(100 BYTE), 
	"PROD_ITEM_KEY" VARCHAR2(100 BYTE), 
	"PROD_LOB" VARCHAR2(100 BYTE), 
	"PROD_TYPE" VARCHAR2(100 BYTE)
   ) PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 NOCOMPRESS LOGGING
  STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645
  PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1 BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)
  TABLESPACE "SYSTEM" ;
--------------------------------------------------------
--  DDL for Table CLOUD_F_BILL_REV
--------------------------------------------------------

  CREATE TABLE CLOUD_F_BILL_REV
   (	"ADDR_KEY" NUMBER, 
	"CHANNEL_NAME" VARCHAR2(20 BYTE), 
	"COST_FIXED" NUMBER, 
	"COST_VARIABLE" NUMBER, 
	"CUST_NUMBER" NUMBER, 
	"DISCOUNT_VALUE" NUMBER, 
	"ORDER_KEY" NUMBER, 
	"ORDER_STATUS" VARCHAR2(200 BYTE), 
	"PROD_ITEM_KEY" VARCHAR2(20 BYTE), 
	"REVENUE" NUMBER, 
	"TIME_BILL_DT" DATE, 
	"TIME_PAID_DT" DATE, 
	"UNITS" NUMBER
   ) PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 NOCOMPRESS LOGGING
  STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645
  PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1 BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)
  TABLESPACE "SYSTEM" ;
  
REM INSERTING into CLOUD_D_CUSTOMERS
SET DEFINE OFF;
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('03/01/74','DD/MM/RR'),'695','F','Married','Gary Schultz','224','Active Singles','TYPE 3');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'795','F','Married','Wesley Acland','916','Students','TYPE 1');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'708','M','Married','Jemma Lusty','2381','Students','TYPE 2');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'633','M','Single','Will Hayden','5100','Students','TYPE 3');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'601','M','Widow','Mike Sachs','4625','Rural based','TYPE 3');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'615','M','Single','John Miller','3355','Urban based','TYPE 3');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'657','M','Divorced','Renee Shillingford','5716','Seniors','TYPE 5');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'700','M','Divorced','Mirza Aslam','6601','Urban based','TYPE 4');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'695','F','Single','Awesome Azul','68','Rural based','TYPE 1');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'769','M','Divorced','Tobias Hartmann','6545','Urban based','TYPE 4');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'680','M','Divorced','Louise Wessbecher','7690','Students','TYPE 3');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'809','M','Married','Beto Avendaño','4035','Baby Boomers','TYPE 4');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'784','M','Divorced','Hallie Conway','7798','Urban based','TYPE 1');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'731','M','Married','Ijoe Milijoe','3582','Students','TYPE 1');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'699','M','Widow','Philina Chan','1535','Baby Boomers','TYPE 4');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'601','M','Divorced','Sam Abriol','6999','Urban based','TYPE 5');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'698','M','Divorced','Kerrion Nash','4435','Others','TYPE 4');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'701','M','Married','Richelle Gonzalez','8979','Rural based','TYPE 3');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'637','M','Divorced','Matt Vetter','9292','Students','TYPE 1');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'722','M','Married','El Ale','9343','Urban based','TYPE 3');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'600','M','Married','Leo Newcomer','641','Seniors','TYPE 2');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'696','M','Widow','Dimas Wirabuana','8258','Others','TYPE 4');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'817','M','Married','Ali Alkubaisi','1989','Others','TYPE 1');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'719','M','Widow','Salomon Sierra','7750','Urban based','TYPE 2');
Insert into CLOUD_D_CUSTOMERS (CUST_BIRTH_DATE,CUST_CREDIT_RATE,CUST_GENDER,CUST_MARITAL_STATUS,CUST_NAME,CUST_NUMBER,CUST_SEGMENT,CUST_TYPE) values (to_date('20/08/18','DD/MM/RR'),'795','M','Single','Nick Hamden','1657','Others','TYPE 4');
REM INSERTING into CLOUD_D_GEOGRAPHY
SET DEFINE OFF;
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('117','BALHAM HIGH ROAD','3574','London','GBR','United Kingdom','SW12 9','EN');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('72','Wyoming Ave','7418','Beirut','LBN','Lebanon','LB176597','Beirut');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('269','Executive Dr','6485','Zagreb','HRV','Croatia','HR101415','Grad Zagreb');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('8808','Northern Blvd','14390','Tijuana','MEX','Mexico','ME101654','Baja California');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('404','Broxton Ave','6358','Osaka','JPN','Japan','JP101675','Osaka');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('26921','Vassar St','8613','Port Louis','MUS','Mauritius','MU11583',null);
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('6','W Cornelia Ave','12569','Maputo','MOZ','Mozambique','MO11546','Maputo');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('9','9th St #4','13250','San Jose','CRI','Costa Rica','CR101665','San Jose');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('82136','Post Rd','14247','Sao Paulo','BRA','Brazil','BR131660','Sao Paulo');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('98247','Russell Blvd','13055','Suva','FJI','Fiji','FJ101586','Central');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('128','HIGH STREET NORTH','3600','London','GBR','United Kingdom','E6 2','EN');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('43','Nolan St','5385','Algiers','DZA','Algeria','DZ11666','Alger');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('9','34th Ave #69','13282','La Paz','BOL','Bolivia','BO101661','La Paz');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values (null,null,'1350','Svendborg','DNK','Denmark','DN101403','Syddanmark');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('80289','Victory Ave #9','13837','Lima','PER','Peru','PE11541','Lima');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('60481','N Clark St','6438','St. Petersburg','RUS','Russia','RU126696','City of St. Petersburg');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('1036','Malone Rd','9036','Hong Kong','HKG','Hong Kong S.A.R.','HK151509',null);
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('288','N 168th Ave #266','13223','Quito','ECU','Ecuador','EC101529','Pichincha');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('47','Hall St','8536','Donetsk','UKR','Ukraine','UK151688','Donets''k');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('7','Hall St','6790','Bogota','COL','Colombia','CO101655','Bogota');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('599','GUERRERO ST','3299','San Francisco','USA','United States','94110','California');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('2902','Edison Dr #278','6849','Johannesburg','ZAF','South Africa','ZA151697','Gauteng');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('6','Ackerman Rd','14242','Rio de Janeiro','BRA','Brazil','BR196660','Rio de Janeiro');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('1585','Salem Church Rd #59','14238','Sao Paulo','BRA','Brazil','BR206660','Sao Paulo');
Insert into CLOUD_D_GEOGRAPHY (ADDRESS1,ADDRESS2,ADDR_KEY,CITY,COUNTRY_CODE,COUNTRY_NAME,POSTAL_CODE,STATE_PROV) values ('8855','North Ave','12445','Bucharest','ROU','Romania','RO11422','Bucharest');
REM INSERTING into CLOUD_D_PRODUCTS
SET DEFINE OFF;
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('MP3 Speakers System','BizTech','DigiTech 5D Mark2 Speaker X400 Silver','1293','Electronics','Accessories');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('PocketFun ES','FunPod','FinFlex EOS Lens2/3'''' 17mm E100 Black','1167','Games','Portable');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('7 Megapixel Digital Camera','FunPod','DigiTech Camera SLR Alpha C600Silver Grey','1089','Digital','Camera');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('Game Station','FunPod','SoundX 14 inch Silver','853','Games','Fixed');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('CompCell RX3','BizTech','PhoneCell GPS Phone 3.7 inch M930 Black','1536','Communication','Phones');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('PocketFun ES','FunPod','PocketFun2/3'''' 17mm M280 White','1242','Games','Portable');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('V5x Flip Phone','BizTech','PhoneCell PDA Wifi3.5-inch M200 Black','1520','Communication','Phones');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('Touch-Screen T5','BizTech','PhoneCell Touch Screen Unlocked M300 Gold','1517','Communication','Smart Phones');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('MPEG4 Camcorder','FunPod','PhoneCell Touch Screen without camera E100 Gold','1510','Digital','Camera');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('7 Megapixel Digital Camera','FunPod','7 Megapixel Digital Camera X600 Silver Grey','1057','Digital','Camera');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('PocketFun ES','FunPod','DigiTech Touch XBox T400 Blue','774','Games','Portable');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('PocketFun ES','FunPod','DigiTech Optical XBox G70 Grey','886','Games','Portable');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('Game Station','FunPod','DigiTech Power Adapter PP33 Black','755','Games','Fixed');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('7 Megapixel Digital Camera','FunPod','DigiTech Trendy Case X300 Black','776','Digital','Camera');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('MP3 Speakers System','BizTech','DigiTech 5D Mark2 Speaker M350 Silver','1302','Electronics','Accessories');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('KeyMax S-Phone','BizTech','PhoneCell SmartPhoneUnlocked 4.7 inches L550 White','1564','Communication','Smart Phones');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('PocketFun ES','FunPod','KeyMax USB Keyboard E90 Silver','895','Games','Portable');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('HomeCoach 2000','FunPod','CompCell DesignJet Game X40 Grey','699','Games','Fixed');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('MaxiFun 2000','FunPod','CompCell Ink Ject Gun F300 E100 Grey','679','Games','Portable');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('Bluetooth Adaptor','BizTech','CompCell Color Ink Jet Printer X550 White','719','Electronics','Accessories');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('HomeCoach 2000','FunPod','CompCell Fax Ninja FT300Grey','677','Games','Fixed');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('MicroPod 60Gb','BizTech','MaxiFun 1/3'''' 8.5mm E200 Black','1222','Electronics','Audio');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('PocketFun ES','FunPod','PocketFun1/3'''' 8.5mm M380 White','1241','Games','Portable');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('PocketFun ES','FunPod','CompCell Photo Ink Ject Gamer X300 White','723','Games','Portable');
Insert into CLOUD_D_PRODUCTS (PRODUCT,PROD_BRAND,PROD_ITEM_DSC,PROD_ITEM_KEY,PROD_LOB,PROD_TYPE) values ('Bluetooth Adaptor','BizTech','CompCell Plain Paer Copier X300Grey','693','Electronics','Accessories');
REM INSERTING into CLOUD_F_BILL_REV
SET DEFINE OFF;
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('3574','Catalog','33.12','15.02','224','43.23','3019164','5-Paid','1293','603.09',to_date('20/08/18','DD/MM/RR'),to_date('01/09/12','DD/MM/RR'),'12');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('7418','Store','207.05','80.04','916','90.04','3228082','4-Billed','1167','2515.5',to_date('02/06/13','DD/MM/RR'),to_date('04/07/13','DD/MM/RR'),'10');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('6485','Store','45.02','20.2','2381','0','3409911','9-On Hold','1089','665.71',to_date('20/07/13','DD/MM/RR'),to_date('21/08/13','DD/MM/RR'),'7');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('14390','Store','158.62','19.75','5100','0','2778644','4-Billed','853','1635.48',to_date('06/05/12','DD/MM/RR'),to_date('22/06/12','DD/MM/RR'),'10');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('6358','Online','57.06','18.76','4625','0','2753200','3-Shipped','1536','755.19',to_date('07/05/12','DD/MM/RR'),to_date('21/06/12','DD/MM/RR'),'12');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('8613','Online','97.45','13.65','3355','55.86','2736310','2-Fulfilled','1242','1171.74',to_date('25/05/12','DD/MM/RR'),to_date('27/06/12','DD/MM/RR'),'10');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('12569','Store','88.87','18.76','5716','0','3592956','3-Shipped','1520','1697.13',to_date('16/09/13','DD/MM/RR'),to_date('28/10/13','DD/MM/RR'),'18');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('13250','Catalog','99','19.78','6601','55.49','3977610','6-Cancelled','1517','1242.8',to_date('06/09/12','DD/MM/RR'),to_date('10/10/12','DD/MM/RR'),'12');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('14247','Store','84.45','22.78','68','52.86','827243','9-On Hold','1510','886.18',to_date('10/01/13','DD/MM/RR'),to_date('10/02/13','DD/MM/RR'),'8');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('13055','Online','69.32','27.65','6545','0','3720686','9-On Hold','1057','490.16',to_date('26/12/13','DD/MM/RR'),to_date('10/02/14','DD/MM/RR'),'6');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('3600','Online','177.65','36.99','7690','36.99','3552912','4-Billed','774','1546.59',to_date('06/09/13','DD/MM/RR'),to_date('12/10/13','DD/MM/RR'),'8');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('5385','Store','308.65','9.89','4035','52.35','3256476','3-Shipped','886','1031.89',to_date('01/06/13','DD/MM/RR'),to_date('20/07/13','DD/MM/RR'),'3');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('13282','Catalog','130.55','13.65','7798','130.55','3777656','3-Shipped','755','1456.21',to_date('20/11/13','DD/MM/RR'),to_date('28/12/13','DD/MM/RR'),'11');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('1350','Online','36.78','0','3582','0','3541126','2-Fulfilled','776','845.92',to_date('07/10/13','DD/MM/RR'),to_date('08/11/13','DD/MM/RR'),'20');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('13837','Store','157.39','10.25','1535','0','3006753','1-Booked','1302','1749.89',to_date('01/08/12','DD/MM/RR'),to_date('18/09/12','DD/MM/RR'),'10');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('6438','Store','88.46','18.76','6999','53.7','3956422','4-Billed','1564','899.55',to_date('09/10/12','DD/MM/RR'),to_date('10/11/12','DD/MM/RR'),'8');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('9036','Online','72.98','17.64','4435','46.58','538617','9-On Hold','895','711.13',to_date('01/01/12','DD/MM/RR'),to_date('31/01/12','DD/MM/RR'),'9');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('13223','Store','77.77','31.86','8979','31.86','658275','6-Cancelled','699','824.37',to_date('08/02/12','DD/MM/RR'),to_date('10/03/12','DD/MM/RR'),'12');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('8536','Store','44.87','22.2','9292','22.2','905044','3-Shipped','679','561.96',to_date('07/02/13','DD/MM/RR'),to_date('23/03/13','DD/MM/RR'),'10');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('6790','Catalog','30.3','5.71','9343','5.71','3981934','1-Booked','719','434.68',to_date('22/09/12','DD/MM/RR'),to_date('30/10/12','DD/MM/RR'),'12');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('3299','Catalog','35.77','35.77','641','35.77','851080','5-Paid','677','599.7',to_date('30/01/13','DD/MM/RR'),to_date('04/03/13','DD/MM/RR'),'10');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('6849','Catalog','45.07','32.67','8258','32.67','850946','5-Paid','1222','606.37',to_date('30/01/13','DD/MM/RR'),to_date('19/03/13','DD/MM/RR'),'9');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('14242','Catalog','8.67','13.32','1989','12.21','3444037','4-Billed','1241','255.82',to_date('11/08/13','DD/MM/RR'),to_date('10/09/13','DD/MM/RR'),'21');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('14238','Catalog','10.09','22.78','7750','8.17','677130','4-Billed','723','136.19',to_date('27/02/12','DD/MM/RR'),to_date('16/04/12','DD/MM/RR'),'9');
Insert into CLOUD_F_BILL_REV (ADDR_KEY,CHANNEL_NAME,COST_FIXED,COST_VARIABLE,CUST_NUMBER,DISCOUNT_VALUE,ORDER_KEY,ORDER_STATUS,PROD_ITEM_KEY,REVENUE,TIME_BILL_DT,TIME_PAID_DT,UNITS) values ('12445','Store','52.88','19.87','1657','11.94','4171064','6-Cancelled','693','666.56',to_date('10/11/12','DD/MM/RR'),to_date('12/12/12','DD/MM/RR'),'13');

--------------------------------------------------------
--  Constraints for Table CLOUD_D_CUSTOMERS
--------------------------------------------------------

  ALTER TABLE CLOUD_D_CUSTOMERS MODIFY ("CUST_NUMBER" NOT NULL ENABLE);
  
--------------------------------------------------------
--  Constraints for Table CLOUD_F_BILL_REV
--------------------------------------------------------

  ALTER TABLE CLOUD_F_BILL_REV MODIFY ("REVENUE" NOT NULL ENABLE);
  ALTER TABLE CLOUD_F_BILL_REV MODIFY ("PROD_ITEM_KEY" NOT NULL ENABLE);
  ALTER TABLE CLOUD_F_BILL_REV MODIFY ("ORDER_KEY" NOT NULL ENABLE);
  ALTER TABLE CLOUD_F_BILL_REV MODIFY ("CUST_NUMBER" NOT NULL ENABLE);
  ALTER TABLE CLOUD_F_BILL_REV MODIFY ("ADDR_KEY" NOT NULL ENABLE);

```

Keep in mind we are going to work with a Star Schema model built with these tables we have just created:

- CLOUD_F_BILL_REV (Fact Table)
- CLOUD_D_CUSTOMERS 
- CLOUD_D_PRODUCTS
- CLOUD_D_GEOGRAPHY

Also, we will create 2 other dimension tables using Cloud Data Modeler:

- CLOUD_D_TIME 
- CLOUD_D_ORDERS 

After creating the tables, you should be able to see them on SQL Developer Web:

![TablesSQLDev](https://i.imgur.com/7vvQe1U.png)

A star schema example is displayed below:

![StarSchema](https://i.imgur.com/5df4t6j.gif)

### Dimensional Modeling

Your job here will be creating  the Star Schema Modeling on BI Cloud Modeler from a database with the required tables and columns. First step is to login to OAC and click on the upper-right menu, and choosing to be redirected to the Data Modeler page. 

![ToDataModeler](https://i.imgur.com/nmsd9ao.png)

Create a New Data Model

![CreateModel](https://i.imgur.com/jr6x2YF.png)

And choose the connection you have just created:

![ChooseConnection](https://i.imgur.com/bDQt8io.png)

So you will have to define which are the Fact and Dimension Tables, this is pretty intuitive and can be done in a lot of different ways, like drag-and-dropping or using the Add Buttons:

![InsertDimension](https://i.imgur.com/ZScHWs9.png)

You will also have to indicate every join in the model. This is done by clickin the **Add Join** button.

![AddJoin](https://i.imgur.com/YhExQtZ.png)

After joining every table your model should look something like that:

![JustJoined](https://i.imgur.com/ZYjmxKV.png)

But there is still some problems: There are 2 dimension tables missing and we don't have any measure calculation running in the fact table. Let's create the measures first. Click **CLOUD_F_BILL_REV**.

By choosing the aggregation type, OAC will begin to see the column as a measure. It is also possible to create new columns based on calculations and generate more complex results.

![Aggregation](https://i.imgur.com/zGYHBfb.png)

After defining the measure calculations, go back to the Model page and **Add a Time Dimension**. You will follow a simple wizard that will create a database table, a dimension and a hierarchy.

![Wizard](https://i.imgur.com/JBEGDHO.png)

For the orders dimension, you are going to create a View based on a database table. Go to **Database > Database Actions >Create View** and use the **SQL Query** below to generate the View.

```sql
SELECT
"CLOUD_F_BILL_REV"."ORDER_KEY",
"CLOUD_F_BILL_REV"."ORDER_STATUS",
"CLOUD_F_BILL_REV"."TIME_BILL_DT",
"CLOUD_F_BILL_REV"."TIME_PAID_DT",
"CLOUD_F_BILL_REV"."CHANNEL_NAME"

FROM
"CLOUD_F_BILL_REV" 
```

By **Saving it** and **Adding to Model** you will have the model complete. It is recommended to change the names of the dimensions to keep the model simple and clear.

![OrderDim](https://i.imgur.com/0fGHg9W.png)

Your model will only be ready for consumption when you click the **Publish Model** button in the upper-right. As soon as you do it, users will be able to use the model to generate analysis and dashboards, for example.

![Publish](https://i.imgur.com/jSdLkUc.png)

### Creating Hierarchies

To be added

Data models > My products >Hierarchies

Click in Detail e and change to LOB. Then add every hierarchy levels.

Total > Type > LOB > Brand > Product > Detail



### OBIEE Answers Example

![a](https://i.imgur.com/EpGWHDH.png)

OBIEE > New > Analysis > Select Subject Area > Your Data Model

![b](https://i.imgur.com/uMLqorU.png)

Create a Revenue by Region report.

![c](https://i.imgur.com/Yzt2c7s.png)

Select a visualization chart in the menu:

![d](https://i.imgur.com/PAuvWW9.png)

Save your Results.

![e](https://i.imgur.com/yVwhFff.png)

Next time you are working on the 'Data Visualization' area of OAC you are going to find this Data Model as an available source to create new Visualizations:

![f](https://i.imgur.com/ifYxqym.png)

![g](https://i.imgur.com/hXpCXoT.png)

And using what we have learned on this course we can leverage new ways of presenting our data!

![h](https://i.imgur.com/yLxGhRP.png)





# Creating an ADW data source on RPD

Cloud Modeler isn't the only way of doing Dimensional Modelling on Oracle Analytics Cloud.

In this part we will create a new repository (RPD) using the OAC Dev Tool for the ADWC connection. See **[Creating a Repository Using the Oracle BI Administration Tool](https://www.oracle.com/webfolder/technetwork/tutorials/obe/fmw/bi/bi1221/rpd/rpd.html)** for more Details.



![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/598ef48adb6983ad6e806baabe0ec445/rpd.PNG)



### Setting up the ADW Wallet for the Admin Tool

Now we are going to create a RPD repository to model a Database into Analytics

Edit the **sqlnet.ora** file and update the wallet location with the address where it was saved.

WALLET_LOCATION = (SOURCE = (METHOD = file) (METHOD_DATA = (DIRECTORY="< **Your Client Credentials Folder** >")))

Example:

**WALLET_LOCATION = (SOURCE = (METHOD = file) (METHOD_DATA = (DIRECTORY="C:\Oracle\ADW\Wallet_DB001")))**

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/52edb48e2128d4639fc11bf0e3f40e9e/52edb48e2128d4639fc11bf0e3f40e9e.png)

The examples in this post use a Command Prompt (CMD) window.

Set an **CRED_LOC** environment variable to the location of the ADW credentials folder.

SET CRED_LOC = <**ADW credentials folder**>

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/cb80adedf663c0fa152cf182fa3d8c16/cb80adedf663c0fa152cf182fa3d8c16.png)

### Start the Oracle Analytics Developer Client Tool

CD %OAC_HOME%\BI\BITOOLS\BIN
ADMINTOOL.CMD

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/9b372ef9c96f25851e5522e6d02cc41b/9b372ef9c96f25851e5522e6d02cc41b.png)

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/cf7230981dcd67279eb8733d0425d194/cf7230981dcd67279eb8733d0425d194.png)

 

### Creating a new Repository (RPD)

On **File** menu, click on **New Repository.**

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/529397e39a57166c89884372881c7014/529397e39a57166c89884372881c7014.png)

Enter a **Name** and **Location** (by default it is filled, but can be changed), select **Import Metadata Yes**, enter a password for **Repository Password** and click **Next.**

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/20be9f3e34646f669ef578807b07843d/20be9f3e34646f669ef578807b07843d.png)

### Importing a Physical Database

NIn the **Select Data Source** panel, select the **Connection Type** *Oracle Call Interface (OCI)*, enter the *TNS Connect Descriptor* from the tnsnames.ora file, as the **Data Source Name**, enter the **User Name** and **Password** of the database and click **Next**.

Below is an example of *Connect Descriptor*. See [**Connect Descriptor Descriptions**](https://docs.oracle.com/database/121/NETRF/tnsnames.htm#NETRF265) for additional details.

**(description= (address=(protocol=tcps)(port=1522)(host=< Your Host >))(connect_data=(service_name=< Your Service Name >))(security=(ssl_server_cert_dn="CN=adwc.uscom-east-1.oraclecloud.com,OU=Oracle BMCS US,O=Oracle Corporation,L=Redwood City,ST=California,C=US")) )**

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/960653926ef013f29303d8a0fad4f7e1/960653926ef013f29303d8a0fad4f7e1.png)

In **Select Metadata Types**, select the types of metadata you want to import and click **Next**.

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/20228df87d7376ab20a4f9b3fdd5edaf/20228df87d7376ab20a4f9b3fdd5edaf.png)

In the **Select Metadata Objects** panel, select a schema and tables that have joins in the database, for example, in the **SH** schema select the **CHANNELS** and **SALES** tables **Data Source View** and move to **Repository View**.

On the Connection Pool screen, **check** the **Require fully qualified table names** and click **OK**.

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/1e7a3c7672c12e55f32198966a8a0035/1e7a3c7672c12e55f32198966a8a0035.png)

Click **Finish.**

**Expand** the physical database and schema.

Test the connection by right-clicking the **table** and selecting **Update Row Count**.

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/15d63055b28b097c7b6692f196e72f1e/15d63055b28b097c7b6692f196e72f1e.png)

Rows count is displayed.

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/47fe1865fa5bd76363db8a59879235cc/47fe1865fa5bd76363db8a59879235cc.png)

**Right-click** on the physical database and select **Rename**, for example ADW.

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/585c35580cf763ac35f26b108a984e35/585c35580cf763ac35f26b108a984e35.png)

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/45b390d54e4f53bbc5c3d79935c8b235/45b390d54e4f53bbc5c3d79935c8b235.png)

### **Preparing the RPD to upload to OAC**

Existem duas formas de preparar a conexão do ADW que o RPD irá utilizar no Oracle Analytics Cloud, você There are two ways to prepare the ADW connection that the RPD will use in Oracle Analytics Cloud, you can use the **TNS Connect Descriptor** from the *tnsnames.ora* file from the ADW *wallet* file or the option **Externalize Connection** for Console Connection in Data Visualization.

You can check the options to activate the RPD connection in OAC on post **[OAC - Upload ADW Wallet e Criar Console Connection e Replication Connection](https://blogs.oracle.com/lad-cloud-experts/pt/oac-upload-adw-wallet-e-criar-console-connection-e-replication-connection)**

### Using the Externalize Connection option

As noted above, Externalize Connection can allow the connection pool to use a DV console connection created in OAC. See **[Connecting to a Data Source Using an External Connection](https://docs.oracle.com/en/cloud/paas/analytics-cloud/acabi/upload-data-models-oracle-bi-enterprise-edition.html#GUID-40474084-5325-4177-A4AD-C2D570E588B4)** for more details.

**Open** the ADW connection pool. Right-click **Connection Pool** and then **Properties.**

Check the option **Externalize Connection.**

**Enter** the **Connection name** exactly as it is in the DV Console Connection in OAC (case-sensitive). For example: ADWDV

Click **OK.**

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/799c246dda46dd59910ff27d1f6e97fc/799c246dda46dd59910ff27d1f6e97fc.png)

### Creating a Business Model

A simple way to test the connection of the RPD to the ADW in the cloud is to perform an analysis in the CAB. This requires a simple Business Model and a Presentation Subject Area.

**Right-click** anywhere on the Business Model and Mapping panel and choose **New Business Model **.

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/5cda64c12bd9ca972825f93aefb4fb59/5cda64c12bd9ca972825f93aefb4fb59.png)

Fill in the **Name** field, for example ADW.

 **Uncheck** the option **Disabled** and click **OK.**

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/775a0edfa47e2795652effd48529fbbd/775a0edfa47e2795652effd48529fbbd.png)     

**Drag** the tables from the Physical panel to the ADW business model. The tables appear below the model.

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/1ef3e71081dd9259e1aca5bd0d8c4c61/1ef3e71081dd9259e1aca5bd0d8c4c61.png)

**Click** with the right mouse button on the business model and choose **Business Model Diagram > Whole Diagram.**

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/a1ae0bb4b56227e970dc86d647e59029/a1ae0bb4b56227e970dc86d647e59029.png)

If the tables were joined in the database using a foreign key, they appear joined in the business model.

Otherwise, in the **Diagram** menu, create a **New Join** by selecting the **New Join tool** and drag the pointer from one table to another and click **OK**.

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/3ec39ad9f9170eb646aa07cc402b2676/3ec39ad9f9170eb646aa07cc402b2676.png)

**Close** the Business Model Diagram.

### Creating a Presentation Subject Area

Right-click and choose the option **Create Subject Areas for Logical Stars and Snowflakes.**

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/4f5a773c18c221ca25cde32195246c35/4f5a773c18c221ca25cde32195246c35.png)

The business model is added to the **Presentation Layer**

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/557945c875d9951db779d63ef5e2c90a/557945c875d9951db779d63ef5e2c90a.png)

## Validating the RPD

In the Tools menu select **Show Consistency Checker**.

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/d7e19a8e933cc98b142f066a21087ac0/d7e19a8e933cc98b142f066a21087ac0.png)

Click **Check All Objects** .

Make sure there are no errors (Warnings are OK) and click **Close.**

![img](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/1d5f4835-dba4-4adb-8b0c-73a75300a9da/File/0163ce5b0a7caf181b3956c4e0b44950/0163ce5b0a7caf181b3956c4e0b44950.png)

In the **File** menu click **Save**. click **No** for **Check Global Consistency.** From the **File menu,** click **Exit.** 

### **Summary** 

The validated RPD can be uploaded to OAC using the steps in the [Upload Data Models to OAC documentation](https://docs.oracle.com/en/cloud/paas/analytics-cloud/acabi/upload-data-models-oracle -bi-enterprise-edition.html # GUID-2BEB60F6-986D-4A7A-9D63-EEE67083E98A). This post detailed the steps required to create a connection to the ADW (Autonomous Data Warehouse) using the OAC Client Developer tool on Windows. It also prepared a complete RPD for upload to OAC. See post **[OAC - Upload ADW Wallet and Create Console Connection and Replication Connection](https://blogs.oracle.com/lad-cloud-experts/pt/oac-upload-adw-wallet-e-criar- console-connection-e-replication-connection) ** for details on how to enable the RPD connection in OAC. For further details, visit the [Autonomous Data Warehouse](https://docs.oracle.com/en/cloud/paas/autonomous-data-warehouse-cloud/index.html) and [Analytics Cloud Pages](https: / /docs.oracle.com/en/cloud/paas/analytics-cloud/index.html)



## Data Flow + Machine Learning

After trying out the 'front-end' of Oracle Analytics Cloud it is time to explore other practices and features. With Data Flow, we are able to create joins between different sources and logical operations with the data. Usually we will use it for ETL purposes , just keep in mind it is not as powerful as ODI (which can be provisioned on OCI as a free image - that means you will only pay for the Compute).

Good news is that a Data Flow may imported with a .dva file, so we can take a look on a Machine Learning project completely created in OAC. For that, we will import the **Naive Bayes Apply-Attrition+Analysis.dva** (the file password is **Admin123**).

You can check that you have imported:

- 1 Project

![Proj](https://i.imgur.com/EiBTY9D.png)

- 3 Datasets

![Dataset](https://i.imgur.com/qaSBd2S.png)

- 3 Data Flows

![Dataflow](https://i.imgur.com/4rP9e6R.png)

First, we are going to explore the Naives-Bayes Attrition Training Data Flow. Opening the Data Flow, we can see that we have a pretty straightforward flow: an input, a model training, and an output:

![Naives-Bayes](https://i.imgur.com/AuM2zSF.png)

For bringing data to the Data Flow, you will always use the **Add Data** option, the first one from the list. Other data preparation steps may be used, such as **Joins, Filters, Branchs and Custom Calculations**. An example of a more complex Data Flow is shown below:

![ComplexDF](https://i.imgur.com/JGCgKvO.png)

Going on with the Naives-Bayes Attrition Training example, first step selects the columns from the source dataset. This whole step may be prompted to select the data when the Data Flow runs, so a Flow is not exclusively used for a single data source.

![1stStep](https://i.imgur.com/ef2hQYX.png)

Second step is training a Machine Learning model using an algorithm provided by Oracle Analytics Cloud. In this case, it is a Naive-Bayes algorithm for Classification, used to analyze the history of people leaving the company (Attrition) and predict the behavior of all employees. Since the output is binary (Yes/No), a Binary Classifier is used, but there is also algorithms for **Numeric Predictions, Multi-classifiers and Clustering**, for example. You can set the algorithm parameters as you wish, as long as you keep Attrition as the target variable. This is what we want to predict: if someone is probably going to leave the company or not.

![2ndStep](https://i.imgur.com/SRbtqXV.png)

Last step is saving the model. When you run the Data Flow, the output of it will be a Machine Learning model, and you can edit its name and description.

![3rdStep](https://i.imgur.com/mca5HBc.png)

Data Flow can now be run and will perform every step in it. If there is need of building a big, complex flow, you can split it into smaller ones and run them as a **Sequence**. If one of those Data Flows fails, everything will be rolled back to the initial stage it was before the Sequence start.

![Success](https://i.imgur.com/Y3wTXWY.png)

When the Data Flow is completed, we can check our results on the Machine Learning tab.

![MLTab](https://i.imgur.com/cCF8uMJ.png)

Machine Learning area will display every Model trained by OAC, and also give you managing options.

![ML](https://i.imgur.com/ioPaZsE.png)

By **right-clicking** the Naives-Bayes - Attrition Train Model and **Inspecting** it, it is possible to see overall info about the model and its **Quality** (results like the Confusion Matrix for the model)

![Model](https://i.imgur.com/uu502EY.png)

As you can see, the automatically generated model does not have a very impressive performance. Having a Machine Learning model is not the end of the road for you. Now you are going to **Apply** this model to a dataset and generate a prediction.

You can do it by opening the **Naive-Bayes Apply Model - Attrition Prediction** Data Flow and executing it. Basically, if you choose the Apply Model step of the flow, you can change details like Inputs and Outputs, but we are not going to change anything right now.

![MLApply](https://i.imgur.com/u8qfsN0.png)

The model training data flow saved a model as output. By applying the machine learning model to the dataset, the output si now another dataset, with all the original dataset columns and 2 more: PredictedValue and PredictedConfidence.

As soon as the data flow ends, you can search for the dataset on Data. In this example its name is **Attiction Predicted Data**.

![MLData](https://i.imgur.com/emDKiFZ.png)

If you haven't defined the metrics columns at the Data Flow level, you should do it on the **Prepare Tab**. Otherwise, you can begin exploring the data with Data Visualization. In my case I've chosen to create a custom calculation to count the predicted values. You can do it by **right-clicking My Calculations** and **Adding a Calculation**.

![AddCalc](https://i.imgur.com/MIfmS1G.png)

The name can be CountPredict or any other, and you can choose between lots of logical and mathematical operators. A simple count will do the job here:

```sql
COUNT(<column name>)
```

![count](https://i.imgur.com/TeQDydP.png)

By using the new CountPredict column as Values, PredictedAttrition as Colors and Department as Trellis Columns, a new visualization was generated. One that shows how many employees were predicted to leave and how many were not, grouped by the department where they work:

![Viz](https://i.imgur.com/L042dEx.png)

To take a look on a more advanced dashboard, it is possible to take a look at the project imported with the .dva file:

![ProjectML](https://i.imgur.com/SI8t7CS.png)

There is a lot of visualizations created, all of them could be explored, filtered and shared to other users.

![MLDashboard](https://i.imgur.com/B1QM4h4.png)

That's all for the Machine Learning practice!

## Embedding Oracle Analytics Cloud 

Last but not least we are going to embedded some of our projects into other webpages. This is a pretty straightforward process, but some details may hold you back, so follow this guide step by step.

First of all we are going to need to create a virtual machine to host our website. You could try to run the web page locally, but personally I have ran into a lot of CORS issues (Cross Origin Resource Sharing) when trying to do so. 

### Setting up the Virtual Cloud Network

To create the Virtual Cloud Network, you should go to the **hamburger menu -> Networking -> Virtual Cloud Network** (we usually call them VCNs)

![img](https://i.imgur.com/AyY81E0.png)

Then, you can just choose the **Networking Quickstart** option

![img](https://i.imgur.com/4rcWGjp.png)

Choose the **VCN with Internet Connectivity** and click **Start Workflow**

A wizard will be launched, simply define a name for the VCN and configure the CIDR Block using the values on the examples (just below the boxes)

![img](https://i.imgur.com/HfH0l5Y.png)

Click **Next** and your VCN will be created, along with a Private and a Public Subnet. Since we are going to connect to our Linux instance via ssh and via internet, we are going to use the Public Subnet when we're given the choice.

Click **View Virtual Cloud Network** to check the network details.

Since we are going to use some ports on this network, we will have to enable the connection via those ports. Scroll down to **Resources -> Security Lists** and choose the **Default Security List for your VCN**

![img](https://i.imgur.com/hwr5wb3.png)

Click **Add Ingress Rules** and unlock the **port 80** to allow HTTP communication through the webserver and Oracle Analytics Cloud. That's all we are going to need on the network side (Image unlocks other ports because was taken from another example)

![img](https://i.imgur.com/xORwPl8.png)

After clicking Add Ingress Rules, just check if you have the rule created. We are now ready to create our Linux Virtual Machine.

### Setting up the Linux VM

To create the Linux VM, you will have to go to the Compute Instances console. You can find it at the **Hamburger Menu -> Compute -> Instance**

![img](https://i.imgur.com/DP52OaL.png)

Click **Create Compute Instance** and a Wizard will show up. 

- Choose the **Oracle Linux 7.9** distribution under the Oracle Image tab 
- Set the Intance Shape to **VM.Standard.E2.1**
- Make sure that you are using your **VCN**, and do not forget to choose the **Public Subnet** and to **Assign a Public IP address** to your Linux Instance
- Upload a ssh .pub file to use for connecting with your VM

After sending the request to create your VM, it should take some minutes to get it running (usually no more than 3 min). You will be able to find the **Public IP Address** on the **Instance Details**

![img](https://i.imgur.com/69wTPey.png)

We will use this Public IP Address to ssh into the OS and host our website. 

So you are going to need tools to ssh into the instance and transfer files via FTP. I am going to use [MobaXTerm](https://mobaxterm.mobatek.net/), but feel free to use other alternatives like Putty and WinSCP, for example.

Log into your instance by using:

- Your Public IP Address
- The opc username
- Port 22
- Your .ppk key file

![img](https://i.imgur.com/r3dJLHZ.png)

After connecting to the VM, let's setup the Apache Server to run our website:

```bash
sudo yum install httpd
sudo systemctl enable httpd
sudo systemctl restart httpd
```

These commands will install and initialize the server, do not forget to also open port 80:

```bash
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --reload
```

Our environment is now set. Let's create our HTML file to embed the application. OAC's documentation makes easier for us by having a example in https://docs.oracle.com/en/cloud/paas/analytics-cloud/acubi/embed-javascript.html#GUID-FFBB4351-F80B-453A-9CC3-E3AA4B42539F

```html
<!DOCTYPE html>
<html dir="ltr">
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
        <title>Embeded Oracle Analytics Project Example</title>
        <script src="https://<instance>.analytics.ocp.oraclecloud.com/public/dv/v1/embedding/<embedding mode>/embedding.js" type="application/javascript">
        </script>

    </head>
    <body>
        <h1>Embeded Oracle Analytics Project</h1>
        <div style="border:1px solid black;position: absolute; width: calc(100% - 40px); height: calc(100% - 120px)" >
            <!-- The following tag is the tag that will embed the specified project.
Veryfy the project-path is the same as the server you are hosting this project on. -->
            <oracle-dv
               project-path="<project path>"
               active-page="canvas"
               active-tab-id="1">
            </oracle-dv>
        </div>
        <!--Apply Knockout bindings after DV project is fully loaded.  This should be executed in a body onload handler or in a <script> tag after the <oracle-dv> tag.
        -->
        <script>
        requirejs(['knockout', 'ojs/ojcore', 'ojs/ojknockout', 'ojs/ojcomposite', 'jet-composites/oracle-dv/loader'], function(ko) {
        ko.applyBindings();
        });
        </script>
    </body>
</html>

```

Since this code is just a template, let's generate a code for our project. This is automatic generated on any Canvas if you click on the **upper-right menu -> Developer**

![image-20210514094031549](C:\Users\bcomin\AppData\Roaming\Typora\typora-user-images\image-20210514094031549.png)



This will open the page with the code for embedding our viz.

![image-20210514094133112](C:\Users\bcomin\AppData\Roaming\Typora\typora-user-images\image-20210514094133112.png)



There are two fields that must be replaced on our HTML file.

**Change #1:** The instance address in the <head> section

```html
<script src="https://<instance>.analytics.ocp.oraclecloud.com/public/dv/v1/embedding/<embedding mode>/embedding.js" type="application/javascript"> </script>
```

Also, change the <embedding mode> area to **jet** or **standalone**

- Use `jet` if you're embedding content within an existing Oracle JET application. If you use `jet`, then the version of Oracle JET that’s used by the application must match the version of Oracle JET used by Oracle Analytics.

  Oracle JET is a set of Javascript-based libraries used for the Oracle Analytics user interface.

- Use `standalone` when embedding visualization content in a generic application that doesn’t use Oracle JET. That's what we are going to use in our example.

My code ended up like this:

```html
 <script src="https://workshop-grri30nzv1ul-gr.analytics.ocp.oraclecloud.com/public/dv/v1/embedding/standalone/embedding.js" type="application/javascript">    </script>
```

**Change #2:** The path for our project (directly from the Embed page)

```html
<oracle-dv     project-path="<project path>"
               active-page="canvas"
               active-tab-id="1">         </oracle-dv>
```

My example:

```html
 <oracle-dv    project-path="/@Catalog/users/breno.comin@oracle.com/Sample Project"
               active-page="canvas"
               active-tab-id="1">        </oracle-dv>
```

Save your code as an index.html file and make any changes to the HTML page if you feel like. My final result is: 

```html
<!DOCTYPE html>
<html dir="ltr">
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
        <title>Embeded Oracle Analytics Project Example</title>
        <script src="https://workshop-grri30nzv1ul-gr.analytics.ocp.oraclecloud.com/public/dv/v1/embedding/standalone/embedding.js" type="application/javascript">
        </script>

    </head>
    <body>
        <h1>Embeded Oracle Analytics Project</h1>
        <div style="border:1px solid black;position: absolute; width: calc(100% - 40px); height: calc(100% - 120px)" >
            <oracle-dv
               project-path="/@Catalog/users/breno.comin@oracle.com/Sample Project"
               active-page="canvas"
               active-tab-id="1">
            </oracle-dv>
        </div>
        <script>
        requirejs(['knockout', 'ojs/ojcore', 'ojs/ojknockout', 'ojs/ojcomposite', 'jet-composites/oracle-dv/loader'], function(ko) {
        ko.applyBindings();
        });
        </script>
    </body>
</html>
```



Now we just have to transfer this webpage to our Apache Server.

SSH into the environment using the FTP tool and copy your file to the **/var/www/html** folder

```bash
sudo cp /home/opc/index.html /var/www/html/
```

Now before accessing your Virtual Machine through your public IP address you will have to allow it inside Oracle Analytics. Go to the Burger Menu -> Console -> Safe Domains and add your instance's Public IP as a Safe Domain:

![image-20210514100722464](C:\Users\bcomin\AppData\Roaming\Typora\typora-user-images\image-20210514100722464.png)

And you will be able to see your projects just by entering login credentials:

![image-20210514100234778](C:\Users\bcomin\AppData\Roaming\Typora\typora-user-images\image-20210514100234778.png)

That's all! I hope you have enjoyed this training session and have learned a lot from it!

### Thank You !!!

