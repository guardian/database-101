---
layout: default
---

# Lesson 2 - more queries

So far we've only done very basic queries. SQL gives us a lot more
options here though.

## Setup

Assuming you have created a 'legrandfromage' database.

Clean the `products` table to remove it using the DROP command:

```sql
DROP TABLE products;
```

then recreate it:

```sql
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric
);
```

(Dropping and recreating tends to be quicker than deleting for bigger
tables.)

Insert some data:

```sql
INSERT INTO products (product_no, name, price) VALUES
    (1, 'Edam', 9.99),
    (2, 'Brie', 1.99),
    (3, 'Camembert', 2.99),
    (4, 'Roquefort', 2.99),
    (5, 'Cheddah', 2.99);
```

## Subqueries

SQL frequently allows you to nest one or more queries inside
another. This is mostly useful when joining tables, but can also be
useful with a single table.

Imagine we want to find all cheeses where the price is the same as the
price of Camembert.

>**Exercise:** write and execute this query.

You should get a list of Camembert, Roquefort, and Cheddah.

You probably used a simple select statement with a where clause
checking price.

But what if the price of Cheddah changes frequently and you don't know
it up front?

One solution is a subquery:

```sql
SELECT *
FROM products
WHERE price = (
    SELECT price FROM products WHERE name = 'Cheddah'
) ORDER BY name;
```

There are lots of opportunities in SQL to use subqueries in the place
of constants or lists. Typically though, we use them with joins.

Add the following table:

```sql
CREATE TABLE orders (
    product_no integer,
    date timestamp
);

```

And add some data:

```sql
INSERT INTO orders (product_no, date) VALUES
    (1, now()),
    (2, now()),
    (2, now() - interval '1 hour');
```

*Note: we haven't applied any constraints here so it's possible to add
invalid product_no's. We'll look at constraints in more detail in the
next lesson to fix this though. If you are feeling enthusiastic you
could add a foreign key constraint now though.*

>**Exercise!** Using a subquery, select all products where there is a
>matching order.

>HINT: the subquery will return multiple results, so use 'IN' rather
>than '='. Alternatively, you can achieve this slightly differently
>using 'EXISTS' which wraps a subquery.

Points will be lost if your result set contains duplicates!

## Aggregations

Sometimes you want to compute statistics. For example, getting the
average price of items, or the minimum price.

The docs on these are
[here](https://www.postgresql.org/docs/current/static/functions-aggregate.html).

>**Exercise:** find the number of items in the products table

>**Exercise:** find the average price of items in the products table

>**Exercise:** find the min and max price *in the same query*

## Peeking under the hood

Up until now we've taken advantage of the magical ('declarative')
nature of SQL. But how does Postgres actually execute our
instructions? There's actually a lot of subtlety to this, but a key
tool to inspecting things is the `EXPLAIN` command, which can be
prefixed to any statement:

```sql
EXPLAIN ANALYZE select * from products;
```

>**Exercise:** experiment with explain/analysing some queries,
>including some aggregate ones. Add in some where clauses and a
>limit/offset too.

Note that the *work done* and time taken by the query are not
necessarily linked to the size of the result set.

Don't worry about understanding the output too much for now - we'll
explain it all fully in a later week when we look at performance in
detail. But if you're feeling adventurous you might want to google
some of the things, such as 'Seq Scan' to get ahead.
