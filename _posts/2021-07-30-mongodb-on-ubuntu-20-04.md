---
layout: post
ads: true
comments: true
published: true
title: mongodb on Ubuntu 20.04
categories:
  - linux
  - program
  - tooltips
tags:
  - mongodb
---
### Ref
- https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/

### Install

- Install

```bash
$ wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -
$ echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list

$ sudo apt-get update
$ sudo apt-get install -y mongodb-org
```

- Config data directory

```bash
$ sudo vim /etc/mongod.conf

# Where and how to store data.
storage:
#  dbPath: /var/lib/mongodb
  dbPath: <YOUR_PATH>
  journal:
    enabled: true
```

### Start

```bash
$ sudo systemctl daemon-reload
$ sudo systemctl start mongod
$ sudo systemctl status mongod
```

### Backup/restore

- mongodump

```bash
$ mongodump --help
Usage:
  mongodump <options> <connection-string>

Export the content of a running server into .bson files.

Specify a database with -d and a collection with -c to only dump that database or collection.

Connection strings must begin with mongodb:// or mongodb+srv://.

See http://docs.mongodb.com/database-tools/mongodump/ for more information.
...
namespace options:
  -d, --db=<database-name>                                  database to use
  -c, --collection=<collection-name>                        collection to use


$ mongodump -d <database-name>	# output in dump folder
```

- mongorestore

```bash
$ mongorestore --help
Usage:
  mongorestore <options> <connection-string> <directory or file to restore>

Restore backups generated with mongodump to a running server.


namespace options:
  -d, --db=<database-name>                                  database to use
  -c, --collection=<collection-name>                        collection to use


$ mongorestore -d <database-name> dump/<database-name>
```

- mongosh (the "mongo" shell has been superseded by "mongosh")

	- https://docs.mongodb.com/mongodb-shell/run-commands/
    
```bash
$ mongosh --help

  $ mongosh [options] [db address] [file names (ending in .js or .mongodb)]

  Options:
        --shell                                Run the shell after executing files
        --nodb                                 Don't connect to mongod on startup - no 'db address' [arg] expected
        --norc                                 Will not run the '.mongoshrc.js' file on start up
        --eval [arg]                           Evaluate javascript
        --retryWrites                          Automatically retry write operations upon transient network errors

  DB Address Examples:

        foo                                    Foo database on local machine
        192.168.0.5/foo                        Foo database on 192.168.0.5 machine
        192.168.0.5:9999/foo                   Foo database on 192.168.0.5 machine on port 9999
        mongodb://192.168.0.5:9999/foo         Connection string URI can also be used

  File Names:

        A list of files to run. Files must end in .js and will exit after unless --shell is specified.

  Examples:

        Start mongosh using 'ships' database on specified connection string:
        $ mongosh mongodb://192.168.0.5:9999/ships

  For more information on usage: https://docs.mongodb.com/mongodb-shell.


### Some usage
$ mongosh
  Connecting to: mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000

test> show dbs
  admin           41 kB
  config        73.7 kB
  fdl_database  9.39 MB
  local         73.7 kB
  test           270 kB

test> use fdl_database
  switched to db fdl_database
  
fdl_database> show collections
  fdl_item
  fdl_post
fdl_database>
```
