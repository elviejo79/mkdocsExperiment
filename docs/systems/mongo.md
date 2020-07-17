# MongoDB

MongoDB is a NoSQL document storage as such it is different to Relational Databases (RDBs) like MySQL or postgreSQL.

## Installation

Under debian it is in the default package repository:

```bash
apt-get install -y mongodb
```

This should also install the `mongodb-server` and `mongodb-clients` on your system.

## Documents

The data within mongo is stored as documents in JSON format. They are organized in collections, which is the same as tables in a relational database.

## Shell

To manage the documents you may use the contained shell:

```bash
mongo [<dbname>]
```

This will give you an interactive shell to administrate. It's a JavaScript based shell.

### Commands

In the following table you find some common command examples.

| Command                           | Usage                               |
| --------------------------------- | ----------------------------------- |
| `db`                              | show current db name                |
| `db.<collection>.find().pretty()` | list all documents and pretty print |
| `db.<collection>.insert(<json>)`  | insert document                     |


## Management

### Database

To create a new database you only have to access it:

```bash
> use mydb
switched to db mydb
```

And to get the name of the current database use the `db` command.

### Users

Using `db.createUser()` new users can be added:

```js
> db.createUser({
...   user: "alex",
...   pwd: "test123",
...   roles: ["readWrite", "dbAdmin"]
... });
Successfully added user: { "user" : "alex", "roles" : [ "readWrite", "dbAdmin" ] }
```

### Collections

To create a collection, use the `db.createCollection()` method. It takes one parameter: the name of the collection.

```js
> db.createCollection("persons");
{ "ok" : 1 }
```

The success message will be ok with the count of affected items (or created collections, in this case).

{!docs/abbreviations.txt!}
