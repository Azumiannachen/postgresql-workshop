
# Presentation 

* why databases? why not an Excel Spreadsheet?
  * multi-tiers separation
  * multi-users
  * transactional: ACID
   * Atomicity, Consistency, Isolation, Durability 
   * https://en.wikipedia.org/wiki/ACID
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

On Linux: https://wiki.postgresql.org/wiki/Apt

Setup pgadmin3 v1.22.2 (pgadamin4 is a piece of crap for now)

https://www.pgadmin.org/download/macos.php

# Designing our database

* What are our problem domain?
* What are the entities?
* How do these entities relate to each other?
* What are these entities relationship
* grab bag approach to design
* uuid vs sequential ids (https://www.postgresql.org/docs/current/static/uuid-ossp.html)

# Creating our own database in your server

Using pgadmin3, easy to setup and create tables.

# Creating the schema

* creating extensions

```(sql)
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
```

* creating groups, roles

```
CREATE ROLE rngadam WITH PASSWORD 'hello' IN GROUP workshop
```

 https://www.postgresql.org/docs/9.0/static/sql-createrole.html

* creating schemas

```
CREATE SCHEMA workshop
  AUTHORIZATION rngadam;

GRANT ALL ON SCHEMA workshop TO rngadam;
GRANT ALL ON SCHEMA workshop TO public;
```

* creating tables

```
CREATE TABLE public.members
(
  fullname character varying,
  since timestamp with time zone DEFAULT now(),
  id uuid NOT NULL DEFAULT uuid_generate_v4(),
  CONSTRAINT members_pkey PRIMARY KEY (id)
)
WITH (
  OIDS=FALSE
);
ALTER TABLE public.members
  OWNER TO rngadam;
```

* creating records

```(sql)
INSERT INTO members(fullname) VALUES('Ricky Ng-Adam')
```

* creating stored procedures

```(sql)
CREATE OR REPLACE FUNCTION "search_name"("s" TEXT) RETURNS SETOF members AS $$
  SELECT * 
    FROM members 
    WHERE "fullname" ILIKE '%' ||  s || '%'
$$ LANGUAGE SQL;
```

* creating views and materialized views



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

