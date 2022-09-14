# SQL

## SELECT

```sql
SELECT name FROM financial;
# Insert a comma between each of columns, excluding the last one.
SELECT name, id FROM financial;
SELECT * FROM financial;
SELECT DISTINCT ancestor FROM financial_path; -- DISTINCT key word applies to all columns following it.
SELECT name FROM financial LIMIT 5; /* LIMIT sets the maximum number of  rows that will be return */
SELECT name FROM financial LIMIT 5 OFFSET 5; -- OFFSET sets the starting position
```

## ORDER BY

```sql
SELECT name from financial ORDER BY description; -- at end
SELECT debit, credit, amount FROM financial_transaction ORDER BY debit, amount;
SELECT debit, credit, amount FROM financial_transaction ORDER BY debit DESC, amount DESC; -- DESC only applies to one column before it
```

## WHERE

```sql
SELECT debit, credit, amount FROM financial_transaction WHERE amount >= 1000;
SELECT debit, credit, amount FROM financial_transaction WHERE debit <> '2'; -- also can use '!=', when comparing a value to a column of string type, you should enclose the value in single quote 'value'
SELECT debit, credit, amount FROM financial_transaction WHERE amount BETWEEN 500 AND 1000;
SELECT name, id FROM financial WHERE description IS NULL;
```

## AND, OR, IN, NOT

```sql
SELECT amount, debit, credit FROM financial_transaction WHERE amount >=500 AND debit <=6;
SELECT amount, debit, credit FROM financial_transaction WHERE amount >=500 OR debit <=6;
SELECT amount, debit, credit FROM financial_transaction WHERE (debit = 4 OR debit = 2) AND amount >= 200;
SELECT amount, debit, credit FROM financial_transaction WHERE debit IN ('2', '3');
SELECT amount, debit, credit FROM financial_transaction WHERE NOT debit IN ('2', '3');
SELECT amount, debit, credit FROM financial_transaction WHERE debit NOT IN ('2', '3') AND credit IN ('3','4');
```

## LIKE, \_

```sql
SELECT name, description FROM financial WHERE description LIKE 'n%'; -- caseinsensitive
SELECT name, description FROM financial WHERE description LIKE 'no_e k'; -- caseinsensitive
```

## ||

```sql
SELECT name || ' ' || description FROM financial;
SELECT TRIM(name) || ' ' || TRIM(description) FROM financial;
SELECT TRIM(name) || ' ' || TRIM(description) AS new_column FROM financial;
```

## other

```sql
.mode column -- enable column headers
SELECT date(); -- select current date
SELECT name, lower(name), upper(description) FROM financial;
SELECT date('now','start of month', '+1 month', '-1 day');
SELECT date('now','start of year', '+9 month', '+12 day');
```

## aggregate

```sql
SELECT AVG(amount) AS avg_amount FROM financial_transaction WHERE debit = 2;
SELECT COUNT(*) FROM financial_transaction;
SELECT MAX(amount) FROM financial_transaction;
SELECT MIN(amount) FROM financial_transaction;
SELECT SUM(ft.amount) AS total_amount FROM financial_transaction AS ft WHERE debit = 2;
```

## GROUP BY, HAVING

1. ```sql
   SELECT debit, AVG(amount) AS avg_amount
   from financial_transaction
   GROUP BY debit
   HAVING avg_amount >= 700;
   ```

2. ```sql
   SELECT credit, MAX(amount) AS max_amount
   from financial_transaction;

   SELECT credit, MAX(amount) AS max_amount
   from financial_transaction
   GROUP BY credit
   HAVING max_amount >= 500
   ORDER BY max_amount;
   ```

## inner join/equi join

```sql
SELECT f.id, f.name, ft.debit, ft.credit, ft.amount, ft.description, ft.document
FROM financial AS f, financial_transaction AS ft
WHERE f.id = ft.debit
ORDER BY f.id;

SELECT f.id, f.name, ft.credit, ft.amount, ft.description, ft.document
FROM financial AS f
INNER JOIN financial_transaction AS ft ON f.id = ft.credit
ORDER BY f.id;

SELECT f.id, COUNT(ft.debit) AS count_debit
FROM financial AS f
INNER JOIN financial_transaction AS ft ON f.id = ft.debit
GROUP BY f.id
ORDER by count_debit DESC;
```

## left out join

```sql
SELECT f.id, f.name, ft.credit, ft.amount, ft.description, ft.document
FROM financial AS f
LEFT OUTER JOIN financial_transaction AS ft ON f.id = ft.credit
ORDER BY f.id;

SELECT f.id, COUNT(ft.debit) AS count_debit
FROM financial AS f
LEFT OUTER JOIN financial_transaction AS ft ON f.id = ft.debit
GROUP BY f.id
ORDER by count_debit DESC;
```

## UNION

```sql
SELECT debit, credit FROM financial_transaction
WHERE debit in ('2','3')
UNION
SELECT debit, credit FROM financial_transaction
WHERE credit in ('2','3')
ORDER BY debit;

SELECT debit, credit FROM financial_transaction
WHERE debit in ('2','3')
UNION ALL
SELECT debit, credit FROM financial_transaction
WHERE credit in ('2','3')
ORDER BY debit, credit;
```

## INSERT

```sql
INSERT INTO financial(name) VALUES('abc');

INSERT INTO financial_transaction(debit, credit, amount)
SELECT debit, credit, amount
FROM financial_transaction
WHERE amount >=500;

CREATE TABLE copy_trans AS SELECT * FROM financial_transaction;
```

## UPDATE

```sql
UPDATE financial
SET description = 'Node abc'
WHERE id = 15;

UPDATE financial
SET description = null
WHERE id = 11;
```

## DELETE

```sql
DELETE FROM financial
WHERE id = 15;
```
