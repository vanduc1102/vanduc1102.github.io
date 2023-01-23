---
layout: post
title: Create a new user and database in PostgreSQL
date: 2023-01-13 06:14:27 +0700
categories: [database, postgresql]
tags: [database, postgresql]
---

I have an AWS PostgreSQL instance, it is serving an small web app. I am creating a new application and want to use the same instance to save cost. I could share the same username and password to the new application. But I want to apply some certain security to the PostgreSQL instance, so I decided to create a new user and assign the user to a new database. Here are the 2 methods I tried:

## 1. Using terminal

- Connect to database `template1`

```
# psql -d template1 -U postgres
```

- Add a user called `tom`

```
template1=# CREATE USER tom WITH PASSWORD 'myPassword';
```

Tip: Generate password with `NodeJS` in terminal

```
node -e "console.log(crypto.randomBytes(16).toString('base64').replace(/\W/g, ''))"
```

- Add a database called `jerry_db`

```
template1=# CREATE DATABASE jerry_db;
```

- Now grant all privileges on database

```
template1=# GRANT ALL PRIVILEGES ON DATABASE jerry_db to tom;
```

- If you want to disallow all role to create objects in `public` schema

```
REVOKE ALL ON schema public FROM public;
```

- Practically, you will have to grant all privileges of `public` schema to the new user.

```
GRANT ALL ON schema public TO tom;
```

- Type `\q` to quit:

```
template1=# \q
```

## 2. Using DBeaver GUI

- Create a new role with `Is User` with `Inherit` and `Can Login`

```
CREATE ROLE "duke2" WITH
	NOSUPERUSER
	NOCREATEDB
	NOCREATEROLE
	INHERIT
	LOGIN
	NOREPLICATION
	NOBYPASSRLS
	CONNECTION LIMIT -1;
```

- Create a new db, set its owner as the new user.

![create-db-gui](/assets/2023-01/create-db.png)

### References:

1. [https://www.cyberciti.biz/faq/howto-add-postgresql-user-account/](https://www.cyberciti.biz/faq/howto-add-postgresql-user-account/)

1. [https://www.postgresql.org/docs/current/sql-createrole.html](https://www.postgresql.org/docs/current/sql-createrole.html)

1. [https://tableplus.com/blog/2018/04/postgresql-how-to-grant-access-to-users.html](https://tableplus.com/blog/2018/04/postgresql-how-to-grant-access-to-users.html)
1. [https://dba.stackexchange.com/a/35317/113250](https://dba.stackexchange.com/a/35317/113250)
