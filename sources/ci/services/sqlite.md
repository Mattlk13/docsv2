
#SQLite

SQLite is a software library that implements a self-contained, serverless, zero-configuration, transactional SQL database engine. You can use SQLite, if you do not want to test your code behaviour with other databases.

All Shippable official images come pre-installed with SQLite3.

For example, you can add SQLite to your bundle for Ruby projects by specifying the following in your Gemfile:

```
gem "sqlite3", :platform => :ruby
```

If you're using ActiveRecord, you can add the following to your config/database.yml:

```
test:
  adapter: sqlite3
  database: ":memory:"
  timeout: 500
```
More on SQLite3 can be found on [official SQLite website](https://www.sqlite.org/).
