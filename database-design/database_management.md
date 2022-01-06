# Database Management 

## Database Roles and Access Control
- roles allow you to manage access permissions
- a role is an entity that contains information that:
    - defines the role's priveleges ->
        - can you login?
        - can you create databases?
        - can you write to tables?
    - interacts with the client auth system
        - password
- roles can be assigned to one or more users
- roles are global across a database cluster installation

Creating an empty role
```sql
CREATE ROLE data_analyst;
```

Creating some roles with attributes set
```sql
CREATE ROLE intern WITH PASSWORD 'PasswordForInter' VALID UNTIL '2021-01-01';

CREATE ROLE admin CREATEDB;

-- allow new admin to also create roles
ALTER ROLE admin CREATEROLE;
```

### GRANT and REVOKE priveleges from roles 
You can grant priveleges on objects like views and schemas with `GRANT` and `REVOKE`

Example: You want all your data analysts to have permission to update the `ratings` view:
```sql
GRANT UPDATE ON ratings TO data_analyst;

-- when you don't need it anymore
REVOKE UPDATE ON ratings FROM data_analyst;
```

Available priveleges in PostgreSQL:
- `SELECT`
- `INSERT`
- `UPDATE`
- `DELETE`
- `TRUNCATE`
- `REFERENCES`
- `TRIGGER`
- `CREATE`
- `CONNECT`
- `TEMPORARY`
- `EXECUTE`
- `USAGE`

### Users and groups are both roles
- a role is an entity that can function as a user and/or group
    - user roles
    - group roles

Group role
```sql
CREATE ROLE data_analyst;
```

User role
```sql
-- create a role for the user alex
CREATE ROLE alex WITH PASSWORD 'PasswordForIntern' VALID UNTIL '2021-01-01';

-- grant the analyst role to user alex
GRANT data_analyst TO alex;

-- when no longer needed
REVOKE data_analyst TO alex;
```

Postgres has a set of default roles
|**Role**|**Allowed access**|
|--------|------------------|
|pg_read_all_settings|Read all configuration variables, even those normally visible only to superusers|
|pg_read_all_stats|Read all pg_stat_* views and use various statistics related extensions, even those normally only visible to superusers|
|pg_signal_backend|Send signals to other backends (eg:cancel query, terminate)|
|More...|More...|

### Benefits and Pitfalls of roles
**Benefits**
- Roles live on after users are deleted
- Roles can be created before user accounts
- save DBA time

**Pitfalls**
- sometimes a role will give an individual too much access
    - you need to be very mindful of this

## Table Partitioning
