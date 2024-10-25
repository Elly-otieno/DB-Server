# **Bulding a DB server and Interacting with it using an App**
<br>

**Amazon Relational Database Service** (Amazon RDS) makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while managing time-consuming database administration tasks, which allows us to focus on our applications and business. Amazon RDS provides us with six familiar database engines to choose from: Amazon Aurora, Oracle, Microsoft SQL Server, PostgreSQL, MySQL and MariaDB.

## **Objectives.**
<br>

- Launch an Amazon RDS DB instance with high availability.
- Configure the DB instance to permit connections from our web server.
- Open a web application and interact with our database.

## **Creating a Security Group for the RDS Instance.**  
<br>

1. In the Console, select the **Services** menu, and then select **VPC** under **Networking & Content Delivery**.

2. In the left navigation pane, click **Security Groups**.

3. Click `Create security group` and then configure:

    - Security group name: `DB Security Group`

    - Description: `Permit access from Web Security Group`

    - VPC: `select your VPC`

    You will now add a rule to the security group to permit inbound database requests. The security group currently has no rules. You will add a rule to permit access from the `Web Security Group`.

4. In the **Inbound rules** section, click **Add rule**, then configure:

    - Type: `MySQL/Aurora (3306)`

    - Source: Type `sg` in the search field and then select `Web Security Group`.

    This configures the Database security group to permit inbound traffic on port 3306 from any EC2 instance that is associated with the Web Security Group.

5. Scroll to the bottom of the screen, then click **Create security group**. You will use this security group when launching the Amazon RDS database.


## **Creating a DB Subnet Group.**
<br>

6. In the Console, select the **Services menu**, and then select ****RDS** under **Database**.

7. In the left navigation pane, click **Subnet groups**.

    If the navigation pane is not visible, click the menu icon in the top-left corner.

8. Click **Create DB Subnet Group** then configure:

    - Name: `DB Subnet Group`

    - Description: `DB Subnet Group`

    - VPC ID: `select your VPC`

9. In the **Add subnets** section for *Availability zones*, click the down arrow, then:

    - Select the first Availability zone

    - Select the second Availability zone

10. For **Subnets**, click the drop down, then:

    - For the first Availability zone, *example:* select  10.0.1.0/24

    - For the second Availability zone, *example:* select  10.0.3.0/24

11. Click **Create**

    This adds Private Subnet 1 (10.0.1.0/24) and Private Subnet 2 (10.0.3.0/24). You will use this DB subnet group when creating the database in the next task.

## **Creating an Amazon RDS DB Instance**.
<br>

Here we will configure and launch a Multi-AZ Amazon RDS for MySQL database instance.

Amazon RDS **Multi-AZ** deployments provide enhanced availability and durability for Database (DB) instances, making them a natural fit for production database workloads. When you provision a Multi-AZ DB instance, Amazon RDS automatically creates a primary DB instance and synchronously replicates the data to a standby instance in a different Availability Zone (AZ).

12. In the left navigation pane, click **Databases**.

13. Click **Create database**

    If there is a **Switch to the new database creation flow** at the top of the screen, please click it.

14. Choose **Create database**, then choose **Standard create**.

15. Under the **Engine options** section, for** Engine type**, choose **MySQL**.

16. For **Engine version**, choose the latest version.

17. For **Templates**, choose **Dev/Test** / *select your apppropriate template*.

18. For **Availability and durabilit**y, choose **Multi-AZ DB Instance**.

20. Under **Settings**, configure the following:

    - **DB instance identifier:** `lab-db` / `Enter your DB instance identifier`

    - **Master username:** main

    - **Master password:** `lab-password` / `Enter password`

    - **Confirm password:** `lab-password` / `Enter password`

21. Under **Instance configuration**, configure the following for **DB instance class**:

    - Select  Burstable classes (includes t classes).

    - Select db.t3.medium. 

22. Under **Storage**, configure:

    - Select *General Purpose (SSD)* under **Storage type**.    

23. Under **Connectivity**, configure:

    - **Virtual Private Cloud (VPC)**: Lab VPC

24. Under **VPC security group** select **Choose existing**

25. Under **Existing VPC security groups**

    - Use X to Remove default.

    - Select **DB Security Group** to highlight it in blue.

26. Under **Monitoring**, expand **Additional configuration** and then configure the following:

    - For **Enhanced Monitoring**, uncheck  *Enable Enhanced monitoring*.

27. Scroll down to the **Additional configuration** section and expand this option. Then configure:

    - **Initial database name:** `Enter DB name`

    - Under **Backup**, uncheck  **Enable automated backups**.

    This will turn off backups, which is not normally recommended, but will make the database deploy faster.

28. Scroll to the bottom of the screen, then click **Create database**

    The database will now be launched.

29. Click **`your DB name`** (click the link itself).

    You will now need to wait **approximately 4 minutes** for the database to be available. The deployment process is deploying a database in two different Availability zones.

    Note: If you are prompted with the **Suggested add-ons for lab-db window**, choose Close

    While waiting, we might want to review the [Amazon RDS FAQs](https://aws.amazon.com/rds/faqs/) or grab a cup of coffee.

30. Wait until the **Status** changes to **Modifying** or **Available**.

31. Scroll down to the **Connectivity & Security** section and copy the **Endpoint** field.

    It will look similar to: *`your-db-name`.cggq8lhnxvnv.us-west-2.rds.amazonaws.com*

32. Paste the Endpoint value into a text editor to use later interacting with the DB.


## **Interacting with our Database**.
<br>

Here we will open a web application running on our web server and configure it to use the database.

33. Copy the **WebServer IP address** 

34. Open a new web browser tab, paste the *WebServer IP address* and press Enter.

    The web application will be displayed, showing information about the EC2 instance.

35. At the top of the web application page, click the **RDS** link.

    You will now configure the application to connect to your database.

36. Configure the following settings:

    - **Endpoint:** Paste the Endpoint you copied to a text editor earlier

    - **Database:** `lab` / `Enter your db name`

    - **Username:** `main`

    - **Password:** `Enter your password`

    - Click **Submit**

    A message will appear explaining that the application is running a command to copy information to the database. After a few seconds the application will display an Address Book.

    The Address Book application is using the RDS database to store information.

40. Test the web application.
