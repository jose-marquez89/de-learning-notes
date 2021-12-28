# Processing, Storing and Organizing Data

## OLTP and OLAP

**Driving Questions**
- how should we organize and manage data?
- consider the different schemas, management options and objects that make up a database
    - schemas: how should my data be logically organized?
    - normalization: should my data have minimal dependency and redundancy?
    - views: what joins will be done most often?
    - access control: should all users of the data have the same level of access?
    - DBMS: how do I pick between SQL and NoSQL?
- this all depends on how you intend to use the data
- there is no one right answer


### Approaches to processing data
- OLTP and OLAP are approaches to processing data
- they help define the way the data structure, storage, flow
- decisions around this should be based on the business use case
- OLTP
    - Online Transaction Processing
    - insert sales from transactions at store
    - day to day operations support
- OLAP
    - Online Analytical Processing
    - for more sophisticated analysis of sales
        - most loyal customers
    - more about analysis in terms of business decisions

### OLAP vs OLTP

Summary
||OLTP|OLAP|
|-|---|---|
|_Purpose_|support daily transactions|report and analyze data|
|_Design_|application oriented|subject-oriented|
|_Data_|up-to-date and operational|consolidated and historical|
|_Size_|snapshot, gigabytes|archive, terabytes|
|_Queries_|simple transactions & frequent updates|complex, aggregate queries and limited updates|
|_Users_|thousands|hundreds|


Reliance
- OLTP and OLAP depend on eachother
- Operational database (OLTP) -> Data Warehouse (OLAP)

#### Key Takeaways
- step back and figure out your business requirements
- difference between OLTP and OLAP
- OLAP? OLTP? There are other approaches, but these two are a good place to start

## Storing Data

### Structuring Data
1. Structured data
- follows a schema
- defined data types and relationships
- e.g. sql tables in a relational database

2. Unstructured data
- no schema
- makes up most of the world's data
- e.g. photos, chat logs, MP3

3. Semi-structured data
- doesn't follow a larger schema
- has an ad hoc self-describing structure
- e.g. NoSQL, JSON, XML

Trade Offs
- Structured data is easier to analyze but unstructured data is easier to scale

#### Beyond Traditional Databases
- Traditional databases - OLTP
- Data warehouses - OLAP
    - optimized for analysis
    - usually read-only
    - data from multiple sources
    - can use MPP or massively parallel processing
    - typically uses dimensional modeling and a denormalized schema
    - Azure SQL data warehouse, Amazon Redshift and Google Big Query are all cloud data warehousing solutions
    - Data marts
        - subset of data warehouses
        - dedicated to a specific topic 
- Data lakes - flexibility, scalability, analysis of big data
    - while traditional data warehouses can store unstructured data, it is usually not cost effective
    - cheaper because it uses object storage as opposed to block or file storage (what's the difference?)
    - allows massive amounts of data to be stored
    - all types of data
    - can be petabytes in size
    - unstructured data permits this kind of size
    - lakes on schema-on-read which means that the schema is created as the data is read
    - schema-on-write is when the schema must be pre-defined as in the case of warehouses and traditional databases
    - need to catalog and organize well or it becomes a "data swamp"
    - it's become possible to analyze data in data lakes using things like Spark and Hadoop
        - this is **big data analytics**
        - all three big providers have a data lake solution

### ETL vs ELT
- ETL
    - data is transformed before loading into storage
- ELT 
    - data is loaded into something like a data lake after being extracted
    - transformed for specific process prior to using it in BI tools, analysis, build a data warehouse, do deep learning etc 

## Database design
What it is it?
- determines how data is stored
    - how is data going to be read and updated?
- uses **database models**, high level specifications for database structure
    - relational databases are most common and popular
        - rows as records
        - columns as attributes
    - other options: NoSQL models, object-oriented, network model
- uses schemas
    - blueprint of the database
    - defines tables, fields, relationships, indices and views
    - schema must be respected when inserting data in to relational databases

### Data Modeling
Data model process (high level)
1. Conceptual data model: entities, relationships and attributes
    - tools: data structure diagrams
2. Logical data model: defines tables, columns, relationships "how does the concept map to actual tables?"
    - tools: database models and schemas
3. Physical data model: describes physical storage
    - tools: partitions, CPU's, indexes, backup systems and table spaces

### Beyond the relational model

Dimensional model
- an adaptation of the relational model
- optimized for OLAP queries: aggregate data, not OLTP
- typically uses a star schema
- tends to be easy to interpret and extend

Elements of dimensional model
- fact tables
    - decided by business use cases
    - holds records of a metric
    - changes regularly
    - connects to dimensions via foreign keys
- dimension tables
    - holds descriptions of attributes
    - doesn't change as often as the fact table


