(Setup mongoDB)[https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-red-hat/]

* The configuration of mongodb located at the path `etc/mongod.conf` (where we can config log, data file, network, ...)
* By default access control is not enabled anyone can read and write to database

* Some command: 

```bash
use database_name
```
* Any subsequent operation are on the `database_name`
* To list all database available use the command

```bash
db
```
* The mongoshell use javascript interface command.

* To create a collection use commmand
```bash
db.createCollection("persons")
```

* To list collection
```bash
show collections
```

* To insert collection run the command
```bash
db.persons.insert({
    "name": "Test
})
```

