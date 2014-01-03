## My Rails 4.0 and Postgres Template (for quick spike)

This is my minimal template that I use for spike new idea using Rails 4
and Postgres.

Requirements:

- Postgres database with admin user postgres

```sh
# connect to `postgres` database as `postgres` user (admin)
sudo su - postgres psql postgres
```
- Useful command from inside `psql` session

```text
\?  - list help
\l  - list all databases
\dg - list all roles
```

- From inside `psql` session try to create the role for our app

```text
postgres=# drop database if exists blogs_development;
postgres=# drop database if exists blogs_test;
```

- Create a user (or role) `blogs_admin` with password `password` with `superuser` access.

```text
create role blogs_admin with superuser createdb createrole inherit replication login password 'password'
```

-  Update the database configuration of rails project

```yaml
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

```sh
bundle exec rake db:setup
bundle exec rake db:migrate
```

- start the rails console

```sh
rails s
```

- open your browser to `http://localhost:3000` to see the apps

### TODO:

- add starter gems for `rspec`, `guard`, or `spring`, `pry`, etc.

### Useful links

[http://www.postgresql.org/docs/9.1/static/sql-createrole.html](http://www.postgresql.org/docs/9.1/static/sql-createrole.html)

