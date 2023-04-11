# MongoDB replica set

This project show how to use a transaction in MongoDB using Mongoose

## Prerequisites

<hr>

### Using Docker

To make this work, you must have Docker and Docker-compose installed on your computer.
Node.js 12+ is also required.

### Create the replica set

```shell
./dbstart.sh
```

### Run the code

```shell
yarn start
```

### Connect to the primary docker instance

```shell
docker exec -it mongo1 mongo
```

### Shutdown the replica set

```shell
docker-compose down
```

<hr>
<hr>

### Using Local Instances

1. Create three empty folders (serverA, serverB and serverC)

2. Run the following command

- (change port and folder and log file name for all 3 servers)
- `mongod --port 27003 --dbpath /Users/raktim/Downloads/servers/serverC --logpath /servers/serverC/mongod3.log --replSet rs0 --fork`

3. Check mongo instance by running this command: `ps -ef | grep mongo`

4. Log into serverA by
   `mongo --port 27001` (It should appear like this >> rs0.PRIMARY)

5.

```
cfg ={
    "_id": "rs0",
    "members": [
        {
            "_id": 0,
            "host": "localhost:27001",
            "priority": 3
        },
        {
            "_id": 1,
            "host": "localhost:27002",
            "priority": 2
        },
        {
            "_id": 2,
            "host": "localhost:27003",
            "priority": 1
        }
    ]
}
```

6. Initiate connection using the command `rs.initiate(cfg);`

7. check status like `rs.status(), rs.isMaster().primary, rs.isMaster().me`

8. `show dbs` operation will fail as we havent enable master slave configuration.
   Run `rs.secondaryOk()` to enable master slave configuration on secondary nodes.

9. Now, We are ready to run the Application.
<hr>

## Start the application

1. Change the mongoDB connection string in the app.ts file (port and replSet name)
2. Run code yarn start to start the app.ts

- This is run 3 commands
- 1. Connect to the database
- 2. Seed the database
- 3. Execute success transaction / fail transaction

3. Check the database using any NOSQL monitoring tools(like mongoDB compass) using the connection URI for detailed transactions
