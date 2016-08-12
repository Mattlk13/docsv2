
#RethinkDB

RethinkDB 2.0.1 is pre-installed on all Shippable Official images. However, we do not start it by default since not every build needs RethinkDB.

To start RethinkDB, include the following in your shippable.yml:

```
services:
  - rethinkdb
```

When started, RethinkDB binds to the default localhost 127.0.0.1. 

###Custom startup command 

To customize the startup command, you should define the SHIPPABLE_RETHINKDB_CMD environment variable in your yml. 

For example, the following yml snippet overrides the default startup command for RethinkDB:

```
env:
  global:
    - SHIPPABLE_RETHINKDB_CMD="<command>"
```


