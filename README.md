
# Presentation 

* why databases? why not an Excel Spreadsheet?
  * multi-tier separation
* ACID
  * Atomicity, Consistency, Isolation, Durability 
  * https://en.wikipedia.org/wiki/ACID
  * concept of transactions
* why SQL? what can you do with it?
* why the relational model?
* why PostgreSQL instead of other databases?
  * datatypes (JSON)
  * GIS (Geographical Information System search)
* Stored Procedures (SQL, PL/SQL, Python)

# SQL Exercises

SQL Exercises: https://pgexercises.com/

# Setup database server locally

```(bash)
brew install postgresql
```

Setup pgadmin3 v1.22.2 (pgadamin4 is a piece of crap for now)

https://www.pgadmin.org/download/macos.php

# Designing our database

* What are our problem domain?
* What are the entities?
* How do these entities relate to each other?
* What are these entities relationship
* grab bag approach to design

# Creating our own database in your server

Using pgadmin3, easy to setup and create tables.

# Creating an API (postgrest)

```(bash)
brew install postgrest
```

Postgrest:

https://github.com/begriffs/postgrest


Run Postgresql database:

```
pg_ctl -D /usr/local/var/postgres start
```

Run:

```
postgrest postgrest.conf
```

Content of postrest.conf:

```
db-uri = "postgres://localhost:5432/workshop"
db-schema = "public"
db-anon-role = "rngadam"
db-pool = 10
```

# version control your schema