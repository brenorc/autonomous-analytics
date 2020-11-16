# Autonomous DB Tutorial 

**First of all, download all the required material [here](http://140.238.185.3/assets/files/workshop_files.zip)**

## OCI First Steps

Oracle Cloud is available in some Regions.

Each region have 3 Availability Domains, which represents 3 different Data Centers. Each one have Fault Domains, which represents physical machines located at the Data Center.

These machines are connected via Physical Networks that makes sure our customer will have high speed / low latency services, but they do not have to manage these networks.

We offer off-box Network Virtualization, which moves
storage and network IO out of the hypervisor and enables lower overhead and bare metal instances.

Over this basic infrastructure, we provide the PaaS and IaaS necessary for our customer to reach their goals. 

![OCI - Overview](https://i.imgur.com/NoDpBq7.png)



### Hands-on: Creating the Autonomous DB

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

To make sure the database is working as intended, we are going to log into SQL Developer Web.

In the Database, go to Service Console

![OML Users](https://i.imgur.com/Hp4KKJw.png)

Select SQL Developer Web in the Development Menu

![SampleQuery](https://i.imgur.com/jZsRoF0.png)

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

![SampleQuery2](https://i.imgur.com/tWRkQRl.png)



## Oracle Application Express

Oracle Application Express (APEX) is a low-code development platform that enables you to build scalable, secure enterprise apps, with world-class features, that can be deployed anywhere. APEX is included on every Autonomous Database deployment, and is enabled by a single click.

For this Hands-on Lab I am going to use a guide included in the [Workshop material](http://140.238.185.3/assets/files/workshop_files.zip) (same download available in the first paragraph of this page).

My recommendation is to do the first 2 labs on this material:

- Lab 100 - Provision your Autonomous Database (Go straight to pg 17)
- Lab 200 - Adding Spatial to an APEX Application
- The other labs if you feel like you want to dive deeper into APEX. 

Feel free to skip the introduction and begin on **Page 17 - Prepare the APEX Workspace** since the ADB instance is already created. (APEX lab -> ATP_APEX_HOL_TRIAL_V1.1_PART1_OAC.pdf) 

PS: Follow every step of the guide, using the same Workspace Name otherwise the Import App process will fail. Also, you can skip the Create an Oracle Analytics Cloud exercise (pgs 22-24).



## Loading Data to ADW

In this lab, you will be uploading files to the Oracle Cloud Infrastructure (OCI) Object Storage, creating sample tables, loading data into them from files on the OCI Object Storage, and troubleshooting data loads with errors.

To load data from files in the cloud into your Autonomous Data Warehouse database, use the new PL/SQL DBMS_CLOUD package. The DBMS_CLOUD package supports loading data files from the following Cloud sources: Oracle Cloud Infrastructure Object Storage, Oracle Cloud Infrastructure Object Storage Classic, Amazon AWS S3 and Microsoft Azure Object Store.



### Navigate to Object Storage

From the Autonomous Data Warehouse console, pull out the left side menu from the top-left corner and select **Object Storage**. 

![lab1](https://i.imgur.com/1i410KQ.jpg)

To learn more about the OCI Object Storage, refer to its [documentation](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Tasks/addingbuckets.htm) .

In OCI Object Storage, a bucket is the terminology for a container of multiple files.

Click the **Create Bucket** button:

![lab3](https://i.imgur.com/n2cVKib.jpg)

Name your bucket  and click the **Create Bucket** button.

![lab4](https://i.imgur.com/NCnst5O.jpg)



### Upload Files to Your OCI Object Store Bucket

Click on your **bucket name** to open it:

![lab5](https://i.imgur.com/uE1pUd5.jpg)

Click on the **Upload Object** button:

![lab6](https://i.imgur.com/pS8WSiT.jpg)

Using the browse button or select all the files downloaded in the earlier step, click Upload and wait for the upload to complete:

![lab7](https://i.imgur.com/fSa78SX.jpg)



### Copy the URL for the Files in Your OCI Object Storage

Copy following base URL that points to the location of your files staged in the OCI Object Storage. The simplest way to get this URL is from the "Object Details" in the right hand side ellipsis menu in the Object Store.

![lab9](https://i.imgur.com/SEnOa3S.png)

![lab10](https://i.imgur.com/KwgbBk4.png)

Take a look at the URL you copied. In this example above, the **region name** is us-phoenix-1, the **tenant name** is adwctraining8, and the **bucket name** is ADWCLab.

*Note:* The URL can also be constructed as below:

```lin
https://objectstorage.<region_name>.oraclecloud.com/n/<tenant_name>/b/<bucket_name>/o
```

**Save** the base URL you copied in a note. We will use the base URL in following steps.



### Creating an Object Store Auth Token

To load data from the Oracle Cloud Infrastructure(OCI) Object Storage you will need an OCI user with the appropriate privileges to read data (or upload) data to the Object Store. The communication between the database and the object store relies on the Swift protocol and the OCI user Auth Token.

Go back to the **Autonomous Data Warehouse Console** in your browser. From the pull-out menu on the top left, under **Identity**, click **Users**.

![lab11](https://i.imgur.com/Eyx5o4l.jpg)

Click the **user's name** to view the details. Also, remember the username as you will need that in the next step. This username could also be an email address.

![lab12](https://i.imgur.com/FnEOJPh.jpg)

On the left side of the page, click **Auth Tokens**.

![lab13](https://i.imgur.com/OownUhz.jpg)

Click **Generate Token**.

![lab14](https://i.imgur.com/5JNOUks.jpg)

Enter a friendly **description** for the token and click **Generate Token**.

![lab15](https://i.imgur.com/nyxJJMd.jpg)

The new Auth Token is displayed. Click **Copy** to copy the Auth Token to the clipboard. You probably want to save this in a temporary notepad document for the next few minutes (you'll use it in the next step).
*Note: You can't retrieve the Auth Token again after closing the dialog box.*

![lab16](https://i.imgur.com/ZqlaVGo.jpg)



### Create a Database Credential for Your User

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

![lab17](https://i.imgur.com/RJtBHii.jpg)

Run the script.

![lab18](https://i.imgur.com/aU6fl5q.png)

Now you are ready to load data from the Object Store.

To check if your credential was correctly created, use the command below:

```sql
SELECT * FROM all_credentials;
```

This will list every credential available on your Autonomous Database, along with its OWNER, CREDENTIAL_NAME, USERNAME, WINDOWS_DOMAIN, COMMENTS and if it is ENABLED or not. 



### Creating a Table to receive the data

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



### Loading Data Using the PL/SQL Package, DBMS_CLOUD**

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

To check if all your data was uploaded, you can use the command below:

```sql
select count (*) from CREDIT_SCORING_100K
```

If your result is 100000, it means every row was correctly updated to the table.



### Troubleshooting DBMS_CLOUD data loads**

Connected as your user in SQL Developer, run the following query to look at past and current data loads.

```sql
select * from user_load_operations;
```

Notice how this table lists the past and current load operations in your schema. Any data copy and data validation operation will have backed up records in your Cloud.

Run the following query to see the load that errored out.

```sql
select * from user_load_operations where status='FAILED';
```



## Using Oracle Machine Learning

### Creating OML Users 

In the Database, go to Service Console

![OML Users](https://i.imgur.com/Hp4KKJw.png)

Select Administration and `Manage Oracle ML Users`.

![OML Users2](https://i.imgur.com/00E39Nn.png)

Log in as admin user and password that you assigned when you created the instance and Create new ML user. You will receive shortly an e-mail with your credentials and the link to the Oracle Machine Learning webpage.



### Working with Zeppelin Notebook for Data Mining 

- Login into OML
- Load a notebook
- Explore Zeppelin Notebooks features



##  Working with ATP and Node.js

For this example we are going to create a website running on Node.js and connect it to an Autonomous Transaction Processing Database. All the requests performed by the website are going to be sent directly to the database.

For this practice we are going to need:

- An Autonomous Transaction Processing instance (1OCPU)
- A Virtual Cloud Network
- A Linux Compute Instance

### Setting up the Virtual Cloud Network

We are not going through the ADB instance creation all again, if you have any doubts just go back to **Hands-on: Creating the Autonomous DB** 

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

Click **Add Ingress Rules** and unlock the ports **80,1522,443,3000,8080** That's all we are going to need in this example.

![img](https://i.imgur.com/xORwPl8.png)

After clicking Add Ingress Rules, just check if you have 8 total Ingress Rules. We are now ready to create our Linux Virtual Machine.

### Setting up the Linux VM

To create the Linux VM, you will have to go to the Compute Instances console. You can find it at the **Hamburger Menu -> Compute -> Instance**

![img](https://i.imgur.com/DP52OaL.png)

Click **Create Compute Instance** and a Wizard will show up. 

- Choose the **Oracle Linux 7.7** distribution under the Oracle Image tab 
- Set the Intance Shape to **VM.Standard.E2.1**
- Make sure that you are using your **VCN**, and do not forget to choose the **Public Subnet** and to **Assign a Public IP address** to your Linux Instance
- Upload a ssh .pub file to use for connecting with your VM

After sending the request to create your VM, it should take some minutes to get it running (usually no more than 3 min). You will be able to find the **Public IP Address** on the **Instance Details**

![img](https://i.imgur.com/69wTPey.png)

We will use this Public IP Address to ssh into the OS and host our website. The code is ready and available to be cloned from our Github repository.

So you are going to need tools to ssh into the instance and transfer files via FTP. I am going to use [MobaXTerm](https://mobaxterm.mobatek.net/), but feel free to use other alternatives like Putty and WinSCP, for example.

Log into your instance by using:

- Your Public IP Address
- The opc username
- Port 22
- Your .ppk key file

![img](https://i.imgur.com/r3dJLHZ.png)

After connecting to the VM, let's change to the root user and update the OS:

```bash
sudo su
yum update -y
```

This step may take a couple minutes to finish.

Install git, which we are going to use to clone the website code:

```bash
yum install -y git
```

Create a directory and browse to it, then clone the git project with all the code needed:

```bash
mkdir /home/dev
cd /home/dev
git clone https://github.com/gustavogaspar/workshops.git
```

Alter the instancePrep.sh permissions so you can run this script. This will prepare all the environment for us.

```bash
chmod 775 workshops/instancePrep.sh
source workshops/instancePrep.sh
```

Browse to the /workshop/lib directory and install the Oracle Database Instant Client 19.x by running:

```bash
cd workshops/lib
yum -y install oracle-instantclient19.3-basic-19.3.0.0.0-1.x86_64.rpm
```

Now we are going to connect our VM to the Autonomous DB services

### Connecting to the ADB

Every time we connect to the Autonomous Database we need to use the Wallet file. This is a file containing the credentials needed to create a connection to the ADB.

Open your **Autonomous Transaction Processing** and click **Database Connection**. This will open a window where you are able to download the credentials. Save them as a .zip file.

![img](https://i.imgur.com/BraZog5.png)



Connect via FTP to your environment and **copy the whole .zip file**

![img](https://i.imgur.com/WxczE1I.png)

Copy your wallet file to the Instant Client folder and unzip the files. Don't forget to put your wallet name in the code

```bash
cp /home/opc/<WalletName> /usr/lib/oracle/19.3/client64/lib/network/admin/

cd /usr/lib/oracle/19.3/client64/lib/network/admin/
unzip <WalletName> 
```

Start the Instant Client services by running:

```bash
sh -c "echo /usr/lib/oracle/19.3/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf"
ldconfig
```

Return to the directory created in the Instance configuration and install the npm packages:

```bash
cd /home/dev/workshops/backend
npm install
```

Edit the db.connect.js file according to your database configuration:

```bash
vim src/dbconnect.js
```

After opening the file, click **i** to enter the insert mode. Update your **database password** and **connectionString** (we will use your DatabaseName_TP). After editing the file, hit **ESC** and then **:wq ENTER** to save your updates and quit. We are now able to start the server:

```bash
npm start & 
```

Press **ENTER** to go back to terminal without ending the process.

To verify if your service is running you may use the command:

```bash
ps -a
```



### Frontend configuration

The backend is all configured, so all we need is to edit the frontend and our application will be ready to run.

Browse to the dev directory and edit the app.js file:

```bash
cd /home/dev
vim /home/dev/workshops/frontend/app.js
```

You will have to input you Public IP Address, so identify the string, use **i** to enter insert mode and when finished use **ESC :wq ENTER**

Finally we are going to remove the content from nginx html folder and copy our frontpage into it:

```bash
rm -rf /usr/share/nginx/html/*
cp /home/dev/workshops/frontend/* /usr/share/nginx/html/
```

The next step is to open your browser and type the Public IP address of your VM. If everything is correct, you will see the page below:

![img](https://i.imgur.com/lGQQVKx.png)



Basically you're given 3 options:

- Test your connection to the Database (mostly for troubleshooting)

- By using Simple Oracle Document Access (SODA), creates a table named CARROS and inserts a BLOB into the database includiung a JSON payload.
- Creates the table SERIES and inserts a simple record into the databse via SQL.

Click each one of the buttons and see if your result is a positive status. Those commands will create 2 tables in the database and insert some information into the tables. It's interesting to connect to the database via SQL Developer Web after doing the inserts to query the database and check which kind of information we have just loaded into the database.

If the data was added correctly, this is the end of our Tutorial. 



## Using the Autonomous JSON Database

*Based on: https://blogs.oracle.com/jsondb/autonomous-json-database*

It is also possible to work with JSON objects using any Oracle Autonomous Database, but the Autonomous JSON Database is optimized to do so. In this quick practice we are going to use SODA (Simple Oracle Document Access) - a native, open-source document store API for common programming languages including Java, JavaScript and Python. Developing applications with JSON and SODA is as easy with Oracle as it is with NoSQL databases like MongoDB.

First of all, **create an Autonomous Database**. I suggest creating an Autonomous JSON Database but this practice will work on the other ADB versions as well.

After logging in to **SQL Developer Web**, you can type in 'soda help' to get an overview of the soda commands:

```
soda help
```

![https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/3cf3e953-ae46-4d55-b48c-0f9981b017ef/Image/9fed4c2b9aa010bf884f7ca54ea7b426/step9.png](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/3cf3e953-ae46-4d55-b48c-0f9981b017ef/Image/9fed4c2b9aa010bf884f7ca54ea7b426/step9.png)

Type the following to create a collection 'movies' and insert two JSON documents, with information related to 2 of my personal favorites

```json
soda create movies;
soda insert movies {"name":"Matrix", "launch_year":"1999","genre":["Science Fiction","Action"]}
soda insert movies {"name":"Forrest Gump", "launch_year":"1994","genre":["Comedy","Drama"]}
```

We can now query the collection to find documents matching a search/filter criteria.For example, I will look for movies with the gender set as 'Comedy'

```json
soda get movies -f {"genre":"Comedy"}  
```

![image-20201116170516521](https://i.postimg.cc/t4msDn4g/image-20201116170516521.png)

It is also possible to filter using other expressions as 'greater than', The example below queries for any movies released after 1996 (launch year greater than 1996):

```json
soda get movies -f {"launch_year":{"$gt":1996}}
```

![image-20201116171457354](https://i.postimg.cc/VNrdhsJW/image-20201116171457354.png)

In these examples we used a console to enter SODA commands. Typically, you would use SODA directly from a programming language. We do have SODA drivers for Java, JavaScript (nodeJS), Python, REST, Pl/Sql and ODPI-C.

With the JSON data stored in an Oracle Database it is also possible to use SQL to access the very same data. First, let's describe the collection:

```json
describe movies;
```

![image-20201116171558014](https://i.postimg.cc/5th6Wcvd/image-20201116171558014.png)

As one can see the JSON collection is backed by a regular table. The JSON data is stored in a binary representation optimized for fast reads and piece-wise updates. In order to convert it to a JSON string we use JSON_SERIALIZE.

```sql
select JSON_Serialize(JSON_Document) from movies;
```

![image-20201116171723295](https://i.postimg.cc/9MyzWxt4/image-20201116171723295.png)

With JSON_Table it is possible to unnest the JSON data and project it to relational columns and rows. Please note that the two JSON documents generate 4 rows as both movie have two genres each.

```sql
select j.* from movies NESTED json_document 
            COLUMNS (name, launch_year number, 
              NESTED genre[*] 
              COLUMNS(genre PATH '$')) j;
              
```

![image-20201116171857204](https://i.postimg.cc/cCmH3rz0/image-20201116171857204.png)

Going from a relational representation back to JSON is similarly easy. All we do is add one (or more) JSON generation functions to a query. In the following we are generating an array of all movies names. 

```sql
select JSON_ArrayAgg(c.json_document.name) from movies c;
```

![image-20201116172058361](https://i.postimg.cc/WzjztMX5/image-20201116172058361.png)

Autonomous JSON Database is part of the [Oracle Autonomous Database](https://www.oracle.com/autonomous-database/) family. Autonomous JSON Database shares all of the core features for automation, lifecycle management, security, availability, scalability and elasticity with all other Autonomous Database services and is now extending all of the autonomous benefits to JSON application developers.

## Congratulations !!!

You have completed all of the available tutorials.

For questions, comments, or feedback, feel free to send an e-mail message to me at breno.comin@oracle.com. 

Hope you have enjoyed!

