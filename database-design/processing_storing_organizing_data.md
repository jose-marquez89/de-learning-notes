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
TODO