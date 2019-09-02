## Autonomous DB Tutorials 

#### OCI First Steps

Oracle Cloud is available in some Regions.

Each region have 3 Availability Domains, which represents 3 different Data Centers. Each one have Fault Domains, which represents physical machines located at the Data Center.

These machines are connected via Physical Networks that makes sure our customer will have high speed / low latency services, but they do not have to manage these networks.

We offer off-box Network Virtualization, which moves
storage and network IO out of the hypervisor and enables lower overhead and bare metal instances.

Over this basic infrastructure, we provide the PaaS and IaaS necessary for our customer to reach their goals. 

![OCI - Overview](C:\Users\bcomin\Desktop\OCI - Overview.png)

##### Hands-on: Creating the Autonomous DB

Access https://cloud.oracle.com and login with the credentials provided by the trainer

![Cloud_login1](C:\Users\bcomin\Desktop\Prints Workshop\Cloud_login1.png)

Select Autonomous Data Warehouse in the Upper-left menu (aka hamburger menu) 

![Cloud_login2](C:\Users\bcomin\Desktop\Prints Workshop\Cloud_login2.png)

Create a new database. You will have to choose:

(For this example you can keep the default values)

- The cloud compartment where it will be located.
- The database name and its display name.
- The workload type (ATP or ADW)
- The deployment type (Serverless or Dedicated)
- The CPU count and Storage
- The password for the admin account
- The license type (BYOL or New License)

![Cloud_login3](C:\Users\bcomin\Desktop\Prints Workshop\Cloud_login3.png)

To make sure the database is working as intended, we are going to log into SQL Developer Web.

In the Database, go to Service Console

![OML Users](C:\Users\bcomin\Desktop\Prints Workshop\OML Users.png)

Select SQL Developer Web in the Development Menu

![SampleQuery](C:\Users\bcomin\Desktop\Prints Workshop\SampleQuery.png)

Login with the admin user and your password, and use the following sample query:

```sql
SELECT channel_desc, TO_CHAR(SUM(amount_sold),'9,999,999,999') SALES$,
   RANK() OVER (ORDER BY SUM(amount_sold)) AS default_rank,
   RANK() OVER (ORDER BY SUM(amount_sold) DESC NULLS LAST) AS custom_rank
FROM sh.sales, sh.products, sh.customers, sh.times, sh.channels, sh.countries
WHERE sales.prod_id=products.prod_id AND sales.cust_id=customers.cust_id
  AND customers.country_id = countries.country_id AND sales.time_id=times.time_id
  AND sales.channel_id=channels.channel_id
  AND times.calendar_month_desc IN ('2000-09', '2000-10')
  AND country_iso_code='US'
GROUP BY channel_desc;
```

Your results should look like the picture below:

![SampleQuery2](C:\Users\bcomin\Desktop\Prints Workshop\SampleQuery2.png)

##### DEMO: Connecting to the service through SQL Developer 

##### DEMO: Testing different connections (High, Medium, Low)

##### DEMO: Loading data through SQL*Net

#### Loading Data to ADW

In this lab, you will be uploading files to the Oracle Cloud Infrastructure (OCI) Object Storage, creating sample tables, loading data into them from files on the OCI Object Storage, and troubleshooting data loads with errors.

To load data from files in the cloud into your Autonomous Data Warehouse database, use the new PL/SQL DBMS_CLOUD package. The DBMS_CLOUD package supports loading data files from the following Cloud sources: Oracle Cloud Infrastructure Object Storage, Oracle Cloud Infrastructure Object Storage Classic, Amazon AWS S3 and Microsoft Azure Object Store.

##### ** Navigate to Object Storage**

From the Autonomous Data Warehouse console, pull out the left side menu from the top-left corner and select **Object Storage**. 

![lab1](C:\Users\bcomin\Desktop\Prints Workshop\lab1.jpg)

To learn more about the OCI Object Storage, refer to its [documentation](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Tasks/addingbuckets.htm) .

In OCI Object Storage, a bucket is the terminology for a container of multiple files.

Click the **Create Bucket** button:

![lab3](C:\Users\bcomin\Desktop\Prints Workshop\lab3.jpg)

Name your bucket  and click the **Create Bucket** button.

![lab4](C:\Users\bcomin\Desktop\Prints Workshop\lab4.jpg)

##### **Upload Files to Your OCI Object Store Bucket**

Click on your **bucket name** to open it:

![lab5](C:\Users\bcomin\Desktop\Prints Workshop\lab5.jpg)

Click on the **Upload Object** button:

![lab6](C:\Users\bcomin\Desktop\Prints Workshop\lab6.jpg)

Using the browse button or select all the files downloaded in the earlier step, click Upload and wait for the upload to complete:

![lab7](C:\Users\bcomin\Desktop\Prints Workshop\lab7.jpg)

##### **Copy the URL for the Files in Your OCI Object Storage**

Copy following base URL that points to the location of your files staged in the OCI Object Storage. The simplest way to get this URL is from the "Object Details" in the right hand side ellipsis menu in the Object Store.

![lab9](C:\Users\bcomin\Desktop\Prints Workshop\lab9.png)

![lab10](C:\Users\bcomin\Desktop\Prints Workshop\lab10.png)

Take a look at the URL you copied. In this example above, the **region name** is us-phoenix-1, the **tenant name** is adwctraining8, and the **bucket name** is ADWCLab.

*Note:* The URL can also be constructed as below:

```lin
https://objectstorage.<region_name>.oraclecloud.com/n/<tenant_name>/b/<bucket_name>/o
```

**Save** the base URL you copied in a note. We will use the base URL in following steps.

##### **Creating an Object Store Auth Token**

To load data from the Oracle Cloud Infrastructure(OCI) Object Storage you will need an OCI user with the appropriate privileges to read data (or upload) data to the Object Store. The communication between the database and the object store relies on the Swift protocol and the OCI user Auth Token.

Go back to the **Autonomous Data Warehouse Console** in your browser. From the pull-out menu on the top left, under **Identity**, click **Users**.

![lab11](C:\Users\bcomin\Desktop\Prints Workshop\lab11.jpg)

Click the **user's name** to view the details. Also, remember the username as you will need that in the next step. This username could also be an email address.

![lab12](C:\Users\bcomin\Desktop\Prints Workshop\lab12.jpg)

On the left side of the page, click **Auth Tokens**.

![lab13](C:\Users\bcomin\Desktop\Prints Workshop\lab13.jpg)

Click **Generate Token**.

![lab14](C:\Users\bcomin\Desktop\Prints Workshop\lab14.jpg)

Enter a friendly **description** for the token and click **Generate Token**.

![lab15](C:\Users\bcomin\Desktop\Prints Workshop\lab15.jpg)

The new Auth Token is displayed. Click **Copy** to copy the Auth Token to the clipboard. You probably want to save this in a temporary notepad document for the next few minutes (you'll use it in the next step).
*Note: You can't retrieve the Auth Token again after closing the dialog box.*

![lab16](C:\Users\bcomin\Desktop\Prints Workshop\lab16.jpg)

##### Create a Database Credential for Your User

In order to access data in the Object Store you have to enable your database user to authenticate itself with the Object Store using your OCI object store account and Auth token. You do this by creating a private CREDENTIAL object for your user that stores this information encrypted in your Autonomous Data Warehouse. This information is only usable for your user schema.

Connected as your user in SQL Developer, copy and paste the code below to SQL Developer worksheet.

```plsql
set define off
begin
  DBMS_CLOUD.create_credential(
    credential_name => 'OBJ_STORE_CRED',
    username => '<Username>',
    password => '<Auth Token>'
  );
end;
/
set define on
```

Specify the credentials for your Oracle Cloud Infrastructure Object Storage service: The username will be your **OCI username** (usually your email address, not your database username) and the password is the OCI Object Store **Auth Token** you generated in the previous step. In this example, the credential object named **OBJ_STORE_CRED** is created. You reference this credential name in the following steps.

![lab17](C:\Users\bcomin\Desktop\Prints Workshop\lab17.jpg)

Run the script.

![lab18](C:\Users\bcomin\Desktop\Prints Workshop\lab18.png)

Now you are ready to load data from the Object Store.

##### Creating a Table to receive the data

Next we will create a Table to where we are going to load all the data we have just uploaded to the Object Storage

```sql
DROP TABLE admin.credit_scoring_100k;
CREATE TABLE admin.credit_scoring_100k 
   (customer_id number(38,0), 
    age number(4,0), 
    income number(38,0), 
    marital_status varchar2(26 byte), 
    number_of_liables number(3,0), 
    wealth varchar2(4000 byte), 
    education_level varchar2(26 byte), 
    tenure number(4,0), 
    loan_type varchar2(26 byte), 
    loan_amount number(38,0), 
    loan_length number(5,0), 
    gender varchar2(26 byte), 
    region varchar2(26 byte), 
    current_address_duration number(5,0), 
    residental_status varchar2(26 byte), 
    number_of_prior_loans number(3,0), 
    number_of_current_accounts number(3,0), 
    number_of_saving_accounts number(3,0), 
    occupation varchar2(26 byte), 
    has_checking_account varchar2(26 byte), 
    credit_history varchar2(26 byte), 
    present_employment_since varchar2(26 byte), 
    fixed_income_rate number(4,1), 
    debtor_guarantors varchar2(26 byte), 
    has_own_phone_no varchar2(26 byte), 
    has_same_phone_no_since number(4,0), 
    is_foreign_worker varchar2(26 byte), 
    number_of_open_accounts number(3,0), 
    number_of_closed_accounts number(3,0), 
    number_of_inactive_accounts number(3,0), 
    number_of_inquiries number(3,0), 
    highest_credit_card_limit number(7,0), 
    credit_card_utilization_rate number(4,1), 
    delinquency_status varchar2(26 byte), 
    new_bankruptcy varchar2(26 byte), 
    number_of_collections number(3,0), 
    max_cc_spent_amount number(7,0), 
    max_cc_spent_amount_prev number(7,0), 
    has_collateral varchar2(26 byte), 
    family_size number(3,0), 
    city_size varchar2(26 byte), 
    fathers_job varchar2(26 byte), 
    mothers_job varchar2(26 byte), 
    most_spending_type varchar2(26 byte), 
    second_most_spending_type varchar2(26 byte), 
    third_most_spending_type varchar2(26 byte), 
    school_friends_percentage number(3,1), 
    job_friends_percentage number(3,1), 
    number_of_protestor_likes number(4,0), 
    no_of_protestor_comments number(3,0), 
    no_of_linkedin_contacts number(5,0), 
    average_job_changing_period number(4,0), 
    no_of_debtors_on_fb number(3,0), 
    no_of_recruiters_on_linkedin number(4,0), 
    no_of_total_endorsements number(4,0), 
    no_of_followers_on_twitter number(5,0), 
    mode_job_of_contacts varchar2(26 byte), 
    average_no_of_retweets number(4,0), 
    facebook_influence_score number(3,1), 
    percentage_phd_on_linkedin number(4,0), 
    percentage_masters number(4,0), 
    percentage_ug number(4,0), 
    percentage_high_school number(4,0), 
    percentage_other number(4,0), 
    is_posted_sth_within_a_month varchar2(26 byte), 
    most_popular_post_category varchar2(26 byte), 
    interest_rate number(4,1), 
    earnings number(4,1), 
    unemployment_index number(5,1), 
    production_index number(6,1), 
    housing_index number(7,2), 
    consumer_confidence_index number(4,2), 
    inflation_rate number(5,2), 
    customer_value_segment varchar2(26 byte), 
    customer_dmg_segment varchar2(26 byte), 
    customer_lifetime_value number(8,0), 
    churn_rate_of_cc1 number(4,1), 
    churn_rate_of_cc2 number(4,1), 
    churn_rate_of_ccn number(5,2), 
    churn_rate_of_account_no1 number(4,1), 
    churn_rate__of_account_no2 number(4,1), 
    churn_rate_of_account_non number(4,2), 
    health_score number(3,0), 
    customer_depth number(3,0), 
    lifecycle_stage number(38,0), 
    credit_score_bin varchar2(100 byte)
   );

     grant select any table to public;
```

##### **Loading Data Using the PL/SQL Package, DBMS_CLOUD**

It is possible to use the DBMS_CLOUD package to load the data directly form object storage to the table. This is the preferred choice for any load automation.

```plsql
begin
 dbms_cloud.copy_data(
    table_name =>'CREDIT_SCORING_100K',
    credential_name =>'<insert credential>',
    file_uri_list => '<insert URL>',
    format => json_object('ignoremissingcolumns' value 'true', 'removequotes' value 'true', 'dateformat' value 'YYYY-MM-DD HH24:MI:SS', 'blankasnull' value 'true', 'delimiter' value ',')
 );
end;
```



#####  

##### Creating OML Users 

In the Database, go to Service Console

![OML Users](C:\Users\bcomin\Desktop\Prints Workshop\OML Users.png)

Select Administration and `Manage Oracle ML Users`.

![OML Users2](C:\Users\bcomin\Desktop\Prints Workshop\OML Users2.png)

Log in as admin user and password that you assigned when you created the instance and Create new ML user. 

You will receive shortly an e-mail with your credentials and the link to the Oracle Machine Learning webpage.

##### 





#####  Working with Zeppelin Notebook for Data Mining 



##### Connecting ADW to OAC to generate reports

##### Enriching analyses with Analytics 

##### Displaying results on Oracle Day-by-Day 

##### Using Data Sync to connect to ADWC 