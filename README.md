## My Rails 4.0 and Postgres Template (for quick spike)
This is my minimal start template that I use for spike new idea using Rails 4
and Postgres.

Requirements:

- Postgres database with admin user `postgres`.

### Setup the initial postgres database if required (see below)

```shell
# connect to `postgres` database as `postgres` user
sudo su - postgres psql postgres
```

From inside `psql` session try to create the role for our app

```sql.txt
-- list the current databases
postgres=# \l

-- drop databases if we need to
postgres=# drop database if exists blogs_development;
postgres=# drop database if exists blogs_test;

-- list existing role and drop if any

postgres=# \dg
                              List of roles
  Role name  |                   Attributes                   | Member of
-------------+------------------------------------------------+-----------
 blogs_admin | Superuser, Create role, Create DB, Replication | {}
 postgres    | Superuser, Create role, Create DB, Replication | {}

-- drop them if we need to
postgres=# drop role if exists blogs_admin;

-- create the role or user for our apps (for simplicity we will make this superuser)
-- note: we crate the user 'blogs_adin' with password 'password'
postgres=# create role blogs_admin with superuser createdb createrole inherit replication login password 'password'
```

We should now have the right user that can be used with Rails project

### Update the database configuration of rails project

- edit the `config/database.yml`
```database.yml
development:
  adapter: postgresql
  encoding: unicode
  database: blogs_development
  pool: 5
  username: blogs_admin
  password: password

test:
  adapter: postgresql
  encoding: unicode
  database: blogs_test
  pool: 5
  username: blogs_admin
  password: password

production:
  adapter: postgresql
  encoding: unicode
  database: blogs_production
  pool: 5
  username: blogs_admin
  password: password
```

- Run the migration

```shell
bundle exec rake db:setup
bundle exec rake db:migrate
```

- start the rails console

```shell
rails s
```

- open your browser to `http://localhost:3000` to see the apps

### TODO:

- add starter gems for `rspec`, `guard`, or `spring`, `pry`, etc.

### Useful links

[http://www.postgresql.org/docs/9.1/static/sql-createrole.html](http://www.postgresql.org/docs/9.1/static/sql-createrole.html)
