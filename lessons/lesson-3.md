---
layout: default
---

# Lesson 3 - transactions and types

Last lesson was about sub queries and aggregations, today we'll have a look at transactions and types, specifically the ones provided by Postgres.

## Setup

Assuming you still have the 'legrandfromage' database.

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

*Note: Dropping and recreating tends to be quicker than deleting for bigger
tables, but can you find another way to delete all the rows from the table while keeping its structure?*

Insert some data:

```sql
INSERT INTO products (product_no, name, price) VALUES
    (1, 'Edam', 9.99),
    (2, 'Brie', 1.99),
    (3, 'Camembert', 2.99),
    (4, 'Roquefort', 2.99),
    (5, 'Cheddah', 2.99);
```

## Transaction

Transactions are a mechanism introduced by databases to deal with concurrency.

### The problem
What would happen if a program A was updating the table `products` in order to apply a discount on all prices while another program B was placing an order, therefore calculating a total price based on the same table.

Assuming program A updates the table line by line as it computes some business logic, it is very likely program B will start a computation of the total price on an inconsistent set of data.

The situation gets even worse if program B was also trying to update the `products` table.
Alternatively, if program A crashes mid-way through, some products may still remain on their old prices and the discount would have been only partially applied to the products.

### The solution
To solve the problem, transactions are introduced as a way to group multiple operations into a single atomic operation. It either goes all the way through or never happened.

During the existence of a transaction, other accesses to the database will either see the data as it was before the transaction, or will be put on hold until the transaction is completed.

### Exercises
>**Exercise:** Start a transaction, then update one or two lines of the `products` table. Validate the transaction, select all the lines can you see your changes?

>**Exercise:** Now start a new transaction, then update again one or two lines of the `products` table. This time, rollback the transaction, select all the lines can you see your changes?

>**Exercise:** Open two connections A and B to your database (in pgadmin, open the query editor twice). Starting a transaction from connection A insert one new product and update an existing product. After each insertion of update, select all the rows from the connection B.

>Tip: here's the Postgres documentation about transaction: https://www.Postgresql.org/docs/current/static/tutorial-transactions.html

From the connection B, you should only see changes once the transaction from connection B was validated. This is called transaction isolation.

>**Exercise:** Open two connections A and B to your database (in pgadmin, open the query editor twice). Starting a transaction from connection A, update an existing product. Starting a transaction from connection B, update the same product.
Validate transaction A. What did you observe?

You should have seen connection B blocked. This is due to two transaction trying to update the same resource. This is again an effect of the transaction isolation.

*Note: it is important to remember that transactions can be blocking and aren't free, they do impact performance and can sometime lead to dead locks if they aren't used carefully. Like any powerful feature it is always a good idea to only use it if and only if you need it.*

Twiddling your thumbs? Did you know there was many possible transaction isolation types? They all have different properties and the one we saw today is the strictest one: Serializable. You can read more about transaction isolation here: https://www.Postgresql.org/docs/current/static/transaction-iso.html

## Data types

Using types is a great way to ensure your data is consistent and clean. Picking the right type at the design time may save you lots of trouble down the line _and I speak from experience_.
Postgres comes with many different types pre-defined, some are expected, some a bit more exotic.

 - text
 - numeric
 - integer
 - money
 - date
 - timestamp (with or without timezone)
 - inet (ip address)
 - ...

Amongst the lesser known yet extremely useful are UUID and Json. We will have a look at these as they are quite broadly used and could be useful for almost any project at the Guardian. To read more about Postgres' types you can find the documentation [here](https://www.postgresql.org/docs/current/static/datatype.html)

### UUID

A UUID is a standardized type of unique identifiers. They are supported by a very wide range of programming languages and are versatile. They are essentially very (*very*) big random numbers, with a very (*very*) low probability of [collision](https://en.wikipedia.org/wiki/Universally_unique_identifier#Collisions). Postgres comes with support for that type, and additional functions in a separate module.

>**Exercise:** Can you drop and recreate our `products` table and setting the `product_no` as a UUID?

Here's the solution
```sql
CREATE TABLE products (
    product_no uuid,
    name text,
    price numeric
);
```

Now we have the table with the right type, we need to find a way to generate UUIDs in order to insert some data.
Postgres come with native support for UUID, but does not provide all the functions by default. Functions to generate and manipulate UUIDs are available within two modules `uuid-ossp` and `pgcrypto`. Let's use the `uuid-ossp` module:

```sql
CREATE EXTENSION "uuid-ossp";
```

Now let's generate a random uuid!
```sql
SELECT uuid_generate_v4();
```

>**Exercise:** Transform the following statement such that you insert valid data in the database:
```sql
INSERT INTO products (product_no, name, price) VALUES
    (1, 'Edam', 9.99),
    (2, 'Brie', 1.99),
    (3, 'Camembert', 2.99),
    (4, 'Roquefort', 2.99),
    (5, 'Cheddah', 2.99);
```
> select all the lines, you should now see a UUID in identifying each of your product.

*Hint, the UUID documentation can be found [here](https://www.postgresql.org/docs/current/static/datatype-uuid.html) and [here](https://www.postgresql.org/docs/current/static/uuid-ossp.html)*


### Json

It could _a priori_ sound quite odd to start using a type like json in a relational database like Postgres especially as Json is quite open and loosely typed.

However if your application requires both the rigidity of a typed and constrained database, and the openness of Json to store your data, you can get the best of both world by using a json column.

Postgres comes with native support for Json and it will ensure the json you are inserting or updating is valid.

>**Exercise:** Modify the `products` table by adding a new column `metadata`. Bonus point if you manage to do it without dropping and recreating the table.

>**Exercise:** Insert a new product "maroilles" and set the `metadata` to `'{"comment": "this cheese is the smelliest of all"}'`

>**Exercise:** Now query the list of product names and their optional comments

### Bonus: make your own enum type

If data consistency and type safety are important for your application why not integrating type safety all the way down to the database? Without going all out, a quick win can be to define enums.

You would ensure that there are no typos and no case issue when your data is inserted, preventing future bugs from happening.

>**Exercise:** Create a type named `smelliness` that describe how smelly a cheese can be.

>**Exercise:** Add the smelliness column to the `products` table in order to be able to fully describe the product. Insert or update a few rows to ensure it works.

*Hint: the documentation for enums can be found [here](https://www.postgresql.org/docs/current/static/datatype-enum.html)*
