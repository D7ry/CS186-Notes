# Queries
## Basic Clauses

>FROM - Identify table  
WHERE - Filter rows  
SELECT - Filter columns  
GROUP BY - Aggregate  
HAVING - Filter groups  
DISTINCT - Filter duplicates

## Joint Queries
From clause to take in multiple tables
### Cross Product
<p>Joint queries takes every row and combine it with every other row to create a whole new table(terribly inefficient), regardless of contents of the tables.</p>

**Using WHERE clause to filter out unwanted rows**.  

Example

```
SELECT Sailors.sid, Reserves.sname, r.bid  
FROM Sailors s, Reserves r  
WHERE s.sid = r.sid
```

### Range Variable

```
SELECT Sailors.sid, Reserves.name
```

Sailors and Reserves are range variables used to distinguish between same column names in different tables.

### Alias
Use **AS** to create an alias for a table.

```
SELECT S.sid  
FROM `Sailors` as `S`
```

Sailor has been mapped to S as an alias.

```
SELECT x.sname, x.age,  
y.sname AS sname2,  
y.age AS age2
```

A self-joining table can use aliases to distinguish between the two tables.

## Join Variants
### Inner Join
Syntax:  

```
SELECT s.sid, s.name,  
FROM Sailors s INNER JOIN Reserves r  
ON s.sid = r.sid  
```
This is similar to the WHERE clause filtering.

### Natural Join
A natural join is a join that automatically matches columns with the same name.  
i.e. the ON clause is automatically computed in the background.  

Syntax:

```
FROM Sailors s `NATURAL JOIN` Reserves r
```

### Left/Right/Full Outer Join
Synatx is similar to inner join.  
All rows from the left/right table are included, even if there is no match in the leftright table.

Example:
```
SELECT r.sid, b.bid, b.bname  
FROM Reserve2 r RIGHT OUTER JOIN Boats2 b  
ON r.bid = b.bid
```

Boats2 will have its rows included even if there is no match in Reserve2.

Full outer join is simply another variant that preserves the row from either table as long as one table has a match.

The columns of rows of which the tables that are unmatched will be NULL.

## Arithmetic Expressions
Arithmetic expressions can be used in the SELECT and FROM clauses. 

## String Comparisons And Pattern Matching
String comparisons can be used in the WHERE clause.

The **LIKE** operator is used to match a string pattern.

```
SELECT ...  
WHERE S>name `LIKE` 'B_%'
```
A more modern approach is to use REGEX, as in:

```
WHERE S.sname ~ 'B.*'
```

## Combining Predicates
The **AND** and **OR** operators can be used to combine predicates.

```
WHERE B.color = red OR B.color = green
```

Conditional expressions can be further nested.

## Sets
Set: a collection of distinct elements.  
Example: Tuples within a relation.

### Set Semantics

**UNION**: the set of all elements in either set.  
**INTERSECT**: the set of all elements in both sets.  
**EXCEPT**: the set of all elements in the first set but not the second set.

### Multiset Semantics

**ALL**: Cardinality of the set.  
**UNION ALL**: sum of cardinalities  
**INTERSECT ALL**: minimum of cardinalities  
**EXCEPT ALL**: cardinality of the first set - the cardinality of the second set.

## Nested Queries

A query can have a sub-query, usually as a part ot the WHERE clause.

Usual clauses:  
>IN  
EXISTS  
NOT IN  
NOT EXISTS  
ANY  
ALL

**Correlated Subqueries** is recomputed for each tuple of the outer query.

## ARGMAX
Here's a buggy implementation:

```
SELECT S.*, MAX(S.rating)  
FROM Sailors S
```

This confuses the engine as there only exists one MAX but there are just as many
S.* as there are sailors.

The correct implementation is:
```SELECT *  
FROM Sailors S  
WHERE S.rating >= ALL  
(SELECT S2.rating  
FROM Sailors S2)
```

or a simpler way:
```
SELECT *  
FROM Sailors S  
ORDERED BY rating DESC  
LIMIT 1;
```

However, the above query only returns one row, which isn't always deterministic.

## Named Queries

Named queries are like macros in programming languages.  
Syntax:

```
CREATE VIEW *view_name*  
AS *select_statement* (<- expands to a SELECT query)
```
## Null Values
Rules:
- Null values being in any comparator means the result is NULL.
- If WHERE clause is NULL, the result won't be included.
- Null values must be checked explicitly with IS NULL or IS NOT NULL.
- Null column values are ignored by aggregated functions.

Boolean Logic for Nulls:  
>F&&N = F  
T||N = T  
T&&N = N  
F||N = N  
N||N = N  
N&&N = N  
