# Better data quality with constraints

## Integrity Constraints
1. Attribute constraints: data types on columns
2. Key constraints: primary keys
3. Referential integrity constraints: enforced through foreign keys

Why should you care about constraints?
- they give data structure
- they help with data consistency and thus quality, which is a business advantage
- SQL dialects can help you enforce data constraints 

Type casting to help maintain consistency
```sql
-- This will produce an error
CREATE TABLE weather (
    temperature integer,
    wind_speed text);
SELECT temperature * wind_speed AS wind_chill
FROM weather;

-- You should type cast to avoid errors
SELECT temperature * CAST(wind_speed AS integer) AS wind_chill;
)
```

## Working with data types
- Data types define the domain of a column
- Enforce consistent storage of values

### Most common data types
- `text` char strings of any length
- `varchar` max of `n` characters
- `char` fixed length string of `n` characters
- `boolean` can only take three states -> `TRUE`, `FALSE` and `NULL`
- `date`, `time` and `timestamp`: various formats for time and date calculations
- `numeric` arbitrary precision numbers like `3.1457`
- `integer` whole numbers in the range of `-2147483648` and `+2147483647` 
    - `bigint` for larger numbers

### Specifying types upon table creation
```sql
CREATE TABLE students (
    ssn integer,
    student_name varchar(64),
    dob date,
    average_grade numeric(3, 2), -- precision of 3, scale of 2 e.g. 5.54
    tuition_paid boolean
);
```

### Altering types after table creation
```sql
ALTER TABLE students
ALTER COLUMN student_name
TYPE varchar(128);
```

What if you needed to truncate the average_grade value?
```sql
ALTER TABLE students
ALTER COLUMN average_grade
TYPE integer
-- Turns 5.54 into 6 rather than 5 before conversion
USING ROUND(average_grade)
```
What if a value is too long? _Note: it's best not too truncate values in your database._
```sql
-- Basically "retain a substring of every value and throw away the rest"
-- This will make things fit and avoid "value too long" errors
ALTER TABLE table_name
ALTER COLUMN column_name
TYPE varchar(x)
USING SUBSTRING(column_name, FROM 1 FOR x)
```

## The not-null and unique constraints
- won't allow `NULL` values
- you won't be able to insert them in the future

What does `NULL` mean?
- unknown
- doesn't exist
- doesn't apply to column

Example
```sql
CREATE TABLE students (
    ssn integer not null,
    last_name varchar(64) not null,
    home_phone integer,
    office_phone integer
);
```
A student may not have a home phone number and may simply not have an office. These values can be `NULL`.
Two `NULL` values may not have the same meaning, this implies `NULL != NULL`

If your table has already been created
```sql
-- set not null
ALTER TABLE students
ALTER COLUMN home_phone
SET NOT NULL;

-- drop not null
ALTER TABLE students
ALTER COLUMN ssn
DROP NOT NULL;
```

### The unique constraint
Upon table creation
```sql
CREATE TABLE table_name (
    column_name UNIQUE
);
```
After table creation
```sql
ALTER TABLE table_name
ADD CONSTRAINT some_name UNIQUE(column_name)
```