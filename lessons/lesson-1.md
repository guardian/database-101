---
layout: default
---

# Lesson 1 - adding some data

*Requirements: a local installation of Postgres, version 10 is recommended.*

## Creating a database

To start with we need a database to use:

```sql
create database legrandfromage;
```

The use it:

```sql
\c legrandfromage
```

## And now a table

Let's create a table called 'products' in our new database with 3
columns:

```sql
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric
);
```

Note, each column is associated with a type. You can view a list of
Postgres data types here:

## Adding some data

The `INSERT` command is used to add data:

```sql
INSERT INTO products VALUES (1, 'Edam', 9.99);
```

A more verbose, but clearer, form is:

```sql
INSERT INTO products (product_no, name, price) VALUES (1, 'Edam', 9.99);
```

Note, the advantage of names over positions here is that the order
doesn't matter and it's clearer to read for someone unfamiliar with
the table.

>**Exercise:** add some more rows to the table for your favourite products!

>**Exercise:** find out how to add multiple rows in a single command
>and do it!

## Make yourself proud by querying the data

The 'SELECT' command is used to retrieve data:

```sql
SELECT * FROM products LIMIT 1;
```

>**Exercise:** select only the product_no and price columns

>**Exercise:** find out how to alias the price column to 'dollahs' in
>the result and do it!

## Update the data

>**Exercise:** Using the `UPDATE` command and your newly gained
>knowledge of Postgres and the docs, update a record.

## Delete a row

The 'DELETE' command is used to delete data:

```sql

```
