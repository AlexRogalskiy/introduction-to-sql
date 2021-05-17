# JOIN

The `JOIN` clause allows you to combine the data from 2 or more thables into one result set.

As we will be selecting from multiple columns, we would need to include the list of the columns that we want to select from after the `FROM` clause separated by comma.

In this chapter we will go over the following `JOIN` types:

* `CROSS` Join
* `INNER` Join
* `LEFT` Join
* `RIGHT` Join

Before we get started let's create a new database and 2 tables that we are going to work with:

* We are going to call the database `demo_joins`:

```
CREATE DATABASE demo_joins;
```

* Then switch to the new database:

```
USE demo_joins;
```

* Then the first table will be called `users` and it will only have 2 columns: `id` and `username`:

```
CREATE TABLE users
(
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(255) NOT NULL
);
```

* Then let's create a second table called `posts`, and to keep things simple we will have three two columns: `id`, `user_id` and `title`:

```
CREATE TABLE posts
(
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    title VARCHAR(255) NOT NULL
);
```

> The `user_id` column would be used to reference the ID of the user that the post belongs to. It is going to be a one to many relation, e.g. one user could have many post:

![One to many relation](https://imgur.com/ipIjCaL.png)

* Now let's add some data into the two tables first by creating a few users:

```
INSERT INTO users
  ( username )
VALUES
  ('bobby'),
  ('devdojo'),
  ('tony'),
  ('greisi');
```

* And finally add some posts:

```
INSERT INTO posts
  ( user_id, title )
VALUES
  ('1', 'Hello World!'),
  ('2', 'Getting started with SQL'),
  ('3', 'SQL is awesome'),
  ('2', 'MySQL is up!'),
  ('1', 'SQL - structured query language');
```

Now that we've got our tables and demo data ready, let's go ahead and learn how to use joins.

## Cross join

The `CROSS` join allows you to basically put the result of two tables next to each other without specifing any `WHERE` conditions. This makes the `CROSS` join the simplest one but it also not of much use in a real life scenario.

So if we were to select all of the users and all of the posts side by side we will use the following query:

```
SELECT * FROM users CROSS JOIN posts;
```

The output will be all of your users and all of the posts side by side:

```
+----+----------+----+---------+---------------------------------+
| id | username | id | user_id | title                           |
+----+----------+----+---------+---------------------------------+
|  4 | greisi   |  1 |       1 | Hello World!                    |
|  3 | tony     |  1 |       1 | Hello World!                    |
|  2 | devdojo  |  1 |       1 | Hello World!                    |
|  1 | bobby    |  1 |       1 | Hello World!                    |
|  4 | greisi   |  2 |       2 | Getting started with SQL        |
|  3 | tony     |  2 |       2 | Getting started with SQL        |
|  2 | devdojo  |  2 |       2 | Getting started with SQL        |
|  1 | bobby    |  2 |       2 | Getting started with SQL        |
|  4 | greisi   |  3 |       3 | SQL is awesome                  |
|  3 | tony     |  3 |       3 | SQL is awesome                  |
|  2 | devdojo  |  3 |       3 | SQL is awesome                  |
|  1 | bobby    |  3 |       3 | SQL is awesome                  |
|  4 | greisi   |  4 |       2 | MySQL is up!                    |
|  3 | tony     |  4 |       2 | MySQL is up!                    |
|  2 | devdojo  |  4 |       2 | MySQL is up!                    |
|  1 | bobby    |  4 |       2 | MySQL is up!                    |
|  4 | greisi   |  5 |       1 | SQL - structured query language |
|  3 | tony     |  5 |       1 | SQL - structured query language |
|  2 | devdojo  |  5 |       1 | SQL - structured query language |
|  1 | bobby    |  5 |       1 | SQL - structured query language |
+----+----------+----+---------+---------------------------------+
```

As mentioned above, in a real life scenario you will highly unlikely run a `CROSS` join for whole two tables, you would most likey use one of the ollowing joins instead combined with a specific condition.

## Inner join

The `INNER` join is used to join join two tables, however unlike the `CROSS` join, it is based on a condition. By using an `INNER` join you can match the first table to the second one.

As we have a one to many relationship, a best practice would be to use a primary key for the posts `id` column and a foreign key for the `user_id`, that way we can 'link' or relate the users table to the posts table. However this is beyond the scope of this SQL basics eBook, though I might extend it in the future and add more chapters.

As an example and to make things a bit clearer, let's say that you wanted to get all of your users and the posts associated with each user. The query that we would use will look like this:

```
SELECT * FROM users
INNER JOIN posts
ON users.id = posts.user_id;
```

Rundown of the query:

* `SELECT * FROM users`: this is a standard select that we've covered many time sin the previous chapters.
* `INNER JOIN posts`: then we specify the second table which table we want to join the result set with
* `ON users.id = posts.user_id`: finally we specify the logic on how we want the data in these two tables to be merged together. The `user.id` is the `id` column of the `user` table whcih is also the primary ID, and `posts.user_id` is the foreign key in the email address table referring to the ID column in the users table.

The output will be the following, associating each user with their post based on the `user_id` column:

```
+----+----------+----+---------+---------------------------------+
| id | username | id | user_id | title                           |
+----+----------+----+---------+---------------------------------+
|  1 | bobby    |  1 |       1 | Hello World!                    |
|  2 | devdojo  |  2 |       2 | Getting started with SQL        |
|  3 | tony     |  3 |       3 | SQL is awesome                  |
|  2 | devdojo  |  4 |       2 | MySQL is up!                    |
|  1 | bobby    |  5 |       1 | SQL - structured query language |
+----+----------+----+---------+---------------------------------+
```

The main thing that you need to keep in mind here are the `INNER JOIN` and `ON` clauses.

## Left join

## Right join

## Conclusion

For more information you could take a look at the official documentation [here](https://dev.mysql.com/doc/refman/8.0/en/join.html).