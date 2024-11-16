<h1>Sample Database design, creation, and management</h1>
<p>This Project was a course project where I was asked create a mock database and data warehouse for an accounting firm</p>
<h2>Step 1: Design an Entity Relationship Diagram for the Database</h2>
<p>In this step I determined which entities would be most important for the mock accounting firm. I determined that
<br> it would be best to keep it simple. This ERD contains relationships between the small accounting firm's employees, 
<br> the clients (booking, payroll, tax, etc) who receive and pay invoices for services performed.</p>
<img width="1036" alt="ERD" src="https://github.com/heinerkace/GoodStart/assets/159738836/f6b7e8ee-3860-4e80-8b4c-df4ae39157d6">
<h2>Step 2: Design an ERD for the data warehouse</h2>
<p>First I needed to determine what dimensions I wanted to focus on in the data warehouse. 
<br> I decided on Time, Client, Invoice, and Aggregates related to the firm's revenue.
<br> I then designed a Star Schema ERD for the data warehouse. </p>
<img width="998" alt="starschema" src="https://github.com/heinerkace/GoodStart/assets/159738836/23f29bf7-3ba1-45f5-bf0d-61adff877773">

<p>With the ERDs created and the database and warehouse outline, it was then time to create the database inside
<br>SQL Server Management Studio. </p>

<h2>Step 3: Use SQL to create tables to hold our data </h2>
<p>Here I created tables for all the entities in our ERD diagram (only 4 are shown here)
<br><br> It was important to include the correct datatypes and sizes for when I generated the random data later one. 
<br> </p>
<img width="356" alt="CreateDB" src="https://github.com/heinerkace/GoodStart/assets/159738836/2a335492-b30c-453a-9d79-760e18260e81">
<h2>Step 4: Create the data warehouse as a separate Database in SSMS</h2>
<h3>Here is the same being done but for the tables in the data warehouse. </h3>

<img width="768" alt="CreateDW" src="https://github.com/heinerkace/GoodStart/assets/159738836/6140d7c0-8111-4928-87cf-2b4e95cd2bb3">

<h2>Step 5: Generating Random Data for the tables</h2>
<p> I wanted some good sample data for the databse, so I used an website called Mockaroo to generate random
<br> data for the tables. Those data were exported to CSV files and that copied into SQL code to populate the databse tables. </p>
<img width="973" alt="TablesPop1" src="https://github.com/heinerkace/GoodStart/assets/159738836/16632de1-c23d-4278-92aa-b6be8c344aac">
<img width="887" alt="TablesPop3" src="https://github.com/heinerkace/GoodStart/assets/159738836/f5a81486-9fc1-4772-870c-a2debae5a95f">

<h2>Step 6: Using Procedural SQL programs to populate the data warehouse </h2>
<p>I used a few different procedural SQL programs written from scratch to populate the data warehouse tables
<br>These programs make use of disabling referential integrity and enabling it after the program is run
<br> in order to disable foreign key constraints for values not yet used. </p>
<h3>Populating the InvoiceDimension table</h3>
<p>In this program, I simply selected all the values from the already populated Invoice Table from the Accounting 
<br> database and inserted them into the data warehouse table. I did the same for the client dimesion.  </p>
<img width="562" alt="dimInvoiceProc" src="https://github.com/heinerkace/GoodStart/assets/159738836/b5a28c65-8764-41d9-abb5-4c3a1a77cb8d">
<img width="503" alt="DimClientProc" src="https://github.com/heinerkace/GoodStart/assets/159738836/cbdf1aca-af7a-4576-9c21-53a8e9fd56f5">

<h3>Populating the Time Dimension Table</h3>
<p>Part of the Time dimension table is an attribute called dateId. This is an ID created from a calendar date. 
<br> I created this ID for each date called by using a Function</p>
<h4>DateID Function</h4>
<p>In this function called FetchDateID, I create two variables for the Date and Hour. I then set those variables equal
<br> to the date passed when the function is called. 
<br> The DateID variable is set to the cast of the datepart and hourpart variables. The format of the dateID
<br> should be yyyymmdd, so I used style 112 to format the results as such. 
<br> DateID is returned by the function</p>
<img width="448" alt="DateIdFunction" src="https://github.com/heinerkace/GoodStart/assets/159738836/431602fa-daf4-4d01-8abd-ef6b9d004904">
<h4>Time Demension Table Procedure</h4>
<p>The popdimTime procedure utlizes the FetchDateID function to populate the dateId portion of the dimTime Table. 
<br>It then goes through a loop from April 20 of 2023 to April 20 of 2024 and populates the table with values from
<br> each date during that time period. </p>
<img width="598" alt="dimTIme" src="https://github.com/heinerkace/GoodStart/assets/159738836/1d72bdd1-2961-4e97-a556-8c856cdae3e4">

<h3>Using a Trigger to Update Invoice Status when Payment is received</h3>
<p> I created a trigger to update the status of an invoice from 'Pending' to 'Paid' when a new row is added
<br> to the payments table (A payment is received). 
<br>It uses 'after insert' and the keyword 'inserted' to access the row or rows that are newly inserted in the update
<br> to the Payment table.</p>
<img width="1035" alt="FunctionTrigger" src="https://github.com/heinerkace/GoodStart/assets/159738836/5f0a5740-f749-4663-bf30-cac1dfaa78f2">
