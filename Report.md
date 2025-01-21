The first part of the report will develop a relational database management system for a company selling car, new and used, and car accessories. The early diagrams were designed with Lucid Chart app, the normalisation tables were made in Excel and the database was developed with MySQL Workbench software. 
This coursework should be considered as a first approach to solve the business problem. A suitable launch and evaluation would give enough feedback to fulfil the design and implementation of the database.

**1.	Requirement Analysis**

•	Background Information

South London Motors is a multi-million company which has expanded its business over the last years with multiple branches around London. It was managing their information in excel files, but that is no longer sustainable.

•	Problem Statement

The company manages many branches (stores) and a great number of employees (mainly sales agents). The volume of cars has also increased, and it is necessary to have all the information centralised in a way that is accessible to the relevant staff in all branches. The customers shouldn’t have to go to the same store or wait a long time to change their cars. This will secure the loyalty of high paying customers who would change their cars frequently.

•	Functional Requirements

The application must show the right information to the right staff. The sales agents would need the car sales history of a client when they start a negotiation, as well as the history of the cars they have available for sale. The store managers would need to have the record of the sales and transactions per branches, as well as the performance of each sale agent for evaluation. A centralised human resources team would also need the relevant information of all employees and an accountant team would need the information of all the transactions in and out. 

•	Non- Functional Requirements

In this case the relational database will be developed in MySQL Workbench, but the company would opt for a commercial DBMS like Microsoft SQL Server or Oracle. Any modern computer would be suitable for the access and management of the database. For the storage and backup though, a cloud storage service would be more recommendable. The database should have a strong password system according to the sensitive data is storing (Identifications, addresses, payment details) and who is accessing it.

After some consideration, we are going to work in the implementation of the tables for customers, sales agents, cars, accessories, manufacturers, branches, car sales, accessories sales and car returns, as defined in Table 1.

*Table 1. Data dictionary of entities*
| **Entity**        | **Description**                                                                                    | **Attributes**     | **Datatype** | **Constraint** |
| ----------------- | -------------------------------------------------------------------------------------------------- | ------------------ | ------------ | -------------- |
| Customers         | Person recorded for buying a car or a car accessory                                                | Customer_ID        | INT          | Primary key    |
|                   | Customer_Name                                                                                      | Varchar (99)       | Not null     |
|                   | Customer_Surname                                                                                   | Varchar (99)       | Not null     |
|                   | Address                                                                                            | Varchar (99)       | Not null     |
| Agents            | Employees in charge of dealing with customers                                                      | Agent_ID           | INT          | Primary key    |
|                   | Agent_Name                                                                                         | Varchar (99)       | Not null     |
|                   | Agent_Surname                                                                                      | Varchar (99)       | Not null     |
| Cars              | Motorised vehicles of any model, available for sale                                                | Car_ID             | INT          | Primary key    |
|                   | Car_model                                                                                          | Varchar (25)       | Not null     |
|                   | Car_description                                                                                    | Varchar (99)       | Not null     |
|                   | Car_Cost                                                                                           | Decimal            | Not null     |
|                   | Is_New                                                                                             | Bool               | Not null     |
|                   | ManufacturingYear                                                                                  | INT                | Not null     |
|                   | Manufacturer_ID                                                                                    | INT                | Foreign key  |
| Manufacturer      | Business in charge of the production of vehicles                                                   | Manufacturer_ID    | INT          | Primary key    |
|                   | ManufacturerName                                                                                   | Varchar (99)       | Not null     |
| Branches          | Any of the physical stores own by the company                                                      | Branch_ID          | INT          | Primary key    |
|                   | Branch_location                                                                                    | Varchar (99)       | Not null     |
| Accessories       | Devices that enhance a car’s functionality or comfort, sold by the company.                        | Accessories_ID     | INT          | Primary key    |
|                   | Accessories_Name                                                                                   | Varchar (99)       | Not null     |
|                   | Accessories_description                                                                            | Varchar (99)       | Not null     |
|                   | Accessories_brand                                                                                  | Varchar (99)       | Not null     |
|                   | Accessories_cost                                                                                   | Decimal            | Not null     |
| Car Sales         | Record of transactions of cars from any of the branches to a customer                              | CarSale_ID         | INT          | Primary key    |
|                   | Customer_ID                                                                                        | INT                | Foreign key  |
|                   | Car_ID                                                                                             | INT                | Foreign key  |
|                   | Agent_ID                                                                                           | INT                | Foreign key  |
|                   | Branch_ID                                                                                          | INT                | Foreign key  |
|                   | Pay_Method                                                                                         | Varchar (25)       | Not null     |
|                   | Date_purchase                                                                                      | Date               | Not null     |
| Accessories Sales | Record of the sales of accessories.                                                                | AccessoriesSale_ID | INT          | Primary key    |
| Customer_ID       | INT                                                                                                | Foreign key        |
| Accessories_ID    | INT                                                                                                | Foreign key        |
| Agent_ID          | INT                                                                                                | Foreign key        |
| Branch_ID         | INT                                                                                                | Foreign key        |
|                   | PayMethod                                                                                          | Varchar (25)       | Not null     |
|                   | Date_purchase                                                                                      | Date               | Not null     |
| Car Returns       | Record of the transaction in which a customer returns a car as part of payment for a new purchase. | CarReturn_ID       | INT          | Primary key    |
|                   | Customer_ID                                                                                        | INT                | Foreign key  |
|                   | Car_ID                                                                                             | INT                | Foreign key  |
|                   | Agent_ID                                                                                           | INT                | Foreign key  |
|                   | Branch_ID                                                                                          | INT                | Foreign key  |
|                   | PayMethod                                                                                          | Varchar (25)       | Not null     |
|                   | Date_return                                                                                        | Date               | Not null     |

**2.	Entity Relationship Diagrams**
After understanding the business needs, the first phase of the design process involves understanding how the tables would relate in the database.

•	Conceptual Entity Relationship Diagram

We part from the main needs of the company: to sell cars, to sell car accessories and to manage car returns. 
The table of car sales would need to have the information of the client (customer), the sale agent, which branch the transaction is taking place and the car information. This for each record would make a big table, and some information would repeat, so, car sales would need to be connected to the tables with the information needed. In the end it would contain many customers, many agents, and many cars. 
In the same way the table of car returns, would be like the car sales, differing in the fact that cars are coming in instead of out.
The accessories sales table would need to connect to the information of the accessories, and the customers, agents, and branches.
Figure 1 shows the conceptual entity relationship diagram of the tables.

![Conceptual ERD](https://github.com/ACV1904/Databases/blob/main/figures/ERD-CW-concept.png)
*Figure 1. Conceptual ERD*

•	Logical Entity Relationship Diagram

Now in the logical step, we need to think about the information these tables would contain. For instance, the cars table would contain the unique car ID to identify the car and to call its information from other tables. We must include all the relevant information of the car like car model, car description, cost, if it is new or not, the year it was manufactured, and the manufacturer information. In this stage I decided to make a separate table for manufacturers because a manufacturer may many cars of different models, and the information would repeat if we kept it just in the cars table.
Figure 2 shows the logical entity relationship diagram, with the attributes of each table. This information would be suitable to present to different audiences and explain the design structure of the database.

![Logical ERD](https://github.com/ACV1904/Databases/blob/main/figures/ERD-CW-log.png)
*Figure 2. Logical ERD*

•	Physical Entity Relationship Diagram

In the next stage we consider how this information will be implemented in the database. For this we assign a data type and size to each attribute, and we indicate if it is a primary key or foreign key. We also replaced spaces between words for underscores and checked that none of the relationships are many to many. Figure 3 shows the Physical entity relationship diagram with the information for all the attributes. 

![Physical ERD](https://github.com/ACV1904/Databases/blob/main/figures/ERD-CW-phys.png)
*Figure 3. Physical ERD*


**3.	Normalisation Tables**

Normalisation is a process used in real world to understand and organise the structure of the database. Usually, the original tables are messy and suffer from data duplication or incompatibility. To fix these problems, we must break the original table on several tables following the rules in Table 2. 

*Table 2. Database normalisation rules.*

| First normal form                                                 | Second normal form                                                                    | Third normal form                                          |
| ----------------------------------------------------------------- | ------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| \-          Eliminate repeating groups in individual tables.      | \-          Create separate tables for sets of values that apply to multiple records. | \-          Eliminate fields that don't depend on the key. |
| \-          Create a separate table for each set of related data. | \-          Relate these tables with a foreign key.                                   |
| \-          Identify each set of related data with a primary key. |                                                                                       |

*Source: Microsoft 365 (2023).*

We will assume that the company was storing their car sales records as in the first table of Figure 4, the unnormalised form (UNF). For the first normalisation (1NF) we separate the information we know would be repeating as the agent’s name and branch information. In the second normalisation (2NF) we separate again the information that would be better organised in an independent table, like the car’s information, we make sure there is a foreign key in each new table to relate to the first one. Finally, in the third normalisation (3NF) we separate the information by entities and eliminate the fields that don’t depend on the key, for instance, the car’s table doesn’t need a car-sales key, because the car sales table would have the car ID as a foreign key. We also check that the tables can’t break any more. After this, we can visualise the resulting tables and their relationships in Figure 5.
I followed the same procedure for the Accessories sales table, and the results are shown in Figure 6 and Figure 7. 

![Cars normalisation table](https://github.com/ACV1904/Databases/blob/main/figures/Tab-Norm-cars.png)
*Figure 4. Car sales table normalisation*

![Cars normalisation ERD](https://github.com/ACV1904/Databases/blob/main/figures/Normalisation-cars.png)
*Figure 5. Entity relationship diagram of Car Sales after normalisation*

![Accessories normalisation table](https://github.com/ACV1904/Databases/blob/main/figures/Tab-Norm-accessories.png)
*Figure 6. Accessories sales table normalisation*

![Accessories normalisation ERD](https://github.com/ACV1904/Databases/blob/main/figures/Normalisation-accessories.png)
*Figure 7. Entity relationship diagram of Accessories Sales after normalisation*

**4.	Database Implementation** 

The implementation of the database was made in MySQL Workbench. It was important to follow an order so the smaller tables, those without foreign keys are created first and the other tables after that. In many cases where errors were made, the tables had to be dropped and the run the cod again until the structure of the database is the designed one.

[Demonstration](https://github.com/ACV1904/Databases/blob/main/Implementation.pdf)




