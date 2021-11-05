# Uniquely Identify Records With Key Constraints

## Keys and Superkeys
A key uniquely identifies a record
- normally a table contains only unique records
- superkey can be a combination of all attributes
    - if you remove an attribute (read "column" or "feature") and can still uniquely identify records, you still have a superkey
- a minimal superkey is one where you can't remove any more attributes or you'll lose uniqueness
- a minimal superkey can be a _candidate_ key, which is called such because you _choose_ a key from the candidates

There's a very basic way of finding out what qualifies for a key in an existing, populated table:

    Count the distinct records for all possible combinations of columns. If the resulting number x equals the number of all rows in the table for a combination, you have discovered a superkey.

    Then remove one column after another until you can no longer remove columns without seeing the number x decrease. If that is the case, you have discovered a (candidate) key.
