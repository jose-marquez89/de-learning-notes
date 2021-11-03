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