---
layout: default
---

# Lesson 4 - constraints and indexes

We've already briefly touched on constraints in lesson 1 when we added
a unique constraint to the product_no column in our products table.

In relational databases we have lots of flexibility to apply
constraints on data as it is inserted and updated. Constraints can
take the form of types, or restrictions on types, and also express
relationships between columns. There are two main benefits to
constraints:

* contribute to the *integrity* of your data
* they provide valuable information to the query optimiser

Constraints can be applied to columns and also tables.

## Basic constraints

Null is currently a valid value for our columns in the products
table. Let's fix that by adding a constraint:

If we were creating the table from scratch it's quite simple:

```sql
CREATE TABLE products (
    product_no integer NOT NULL,
    name text NOT NULL,
    price numeric NOT NULL
);
```

We just add 'not null' to the column definitions.

As it is, the table already exists so we need to modify it.

>**Exercise:** alter the products table to add the not null constraint
>to each column.

*Hint: use `\h alter table` to see valid syntax. Or look at the doc
page for the command on the Postgres website.*

What other types of constraint are there?

* CHECK constraints (using boolean operators)
* UNIQUE
* REFERENCES
* and more

See the definition of `CREATE TABLE` for available options.

>**Exercise:** Add a check constraint to the products table to ensure
>the price is always > 0.

>**Exercise:** Add a 'discounted price' column, and add a table
>constraint to ensure the discounted price is always less than the
>regular price.

>**Exercise:** Add a primary key constraint to the product_no column
>on the products table.

>**Exercise:** Add a foreign key (REFERENCES) constraint to the orders
>table to ensure the product_no always refers to a valid product_no in
>the products table! This is sometimes known as 'referential
>integrity'.

Note: you should be familiar now with reading syntax expressions in
the docs or via \h. For example, you'll see that constraints can
optionally have a name, but it's not required and this is expressed
as: `[ CONSTRAINT constraint_name ]` in the docs.

## Primary keys

Earlier, we added a primary key constraint to the product table.

A primary key constraint defines the unique identifier for a row. A
natural result of this is that a key can be more than one column.

Functionally, the constraint is equivalent to `UNIQUE NOT NULL`, with
the additional restriction that there can only be one primary key
constraint per table.

Primary keys are heavily related to schema design, which we'll look at
in a later lesson.

## Performance question

>**Question:** How do you think Postgres ensures constraints are met?
>Think about the price constraints above, and then also unique/primary
>key or reference constraint. :0

*Hint: use `\d+ products` to see the current table definition as this
will help you here.*
