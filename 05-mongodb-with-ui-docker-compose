create mongodb and mongodb ui using docker-compose, give the steps to run and connect using mongodb shell to mongodb instance running on docker using docker-compose
To set up a MongoDB instance and MongoDB UI using Docker Compose, and then connect to MongoDB using the MongoDB shell, follow the steps below:

### Step 1: Create a `docker-compose.yml` File

1. Create a new directory for your project:
   ```bash
   mkdir mongodb-docker
   cd mongodb-docker
   ```

2. Create a `docker-compose.yml` file inside the directory with the following content:

```yaml
version: '3.8'
services:
  mongodb:
    image: mongo:latest
    container_name: mongodb
    restart: always
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example

  mongo-express:
    image: mongo-express:latest
    container_name: mongo-express
    restart: always
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: root
      ME_CONFIG_MONGODB_ADMINPASSWORD: example
      ME_CONFIG_MONGODB_SERVER: mongodb
```

### Explanation of `docker-compose.yml`:
- **mongodb service**: Runs the MongoDB database with the default port `27017`. It sets up a root user with the username `root` and password `example`.
- **mongo-express service**: Runs Mongo Express, a web-based MongoDB administration tool. It will be accessible via port `8081` and connects to the `mongodb` service with the root credentials.

### Step 2: Start the Docker Containers

1. Run the following command to start the containers:
   ```bash
   docker-compose up -d
   ```

2. Docker will download the required images and start the containers in detached mode. You can verify that the containers are running using:
   ```bash
   docker-compose ps
   ```

### Step 3: Access Mongo Express (UI)

Once the containers are running, you can access the MongoDB UI (Mongo Express) by opening your browser and going to:
```
http://localhost:8081
```

You will see the Mongo Express interface where you can manage your MongoDB databases visually.

### Step 4: Connect to MongoDB Using MongoDB Shell

1. **Open MongoDB Shell**:
   If you have MongoDB shell (e.g., `mongosh`) installed on your local machine, you can connect to the MongoDB instance running inside the Docker container.

2. **Connect using the MongoDB shell**:
   Run the following command to connect to the MongoDB instance:
   ```bash
   mongosh "mongodb://root:example@localhost:27017"
   ```
   This will connect to the MongoDB instance using the root username and password defined in the `docker-compose.yml` file (`root` and `example`).

3. Once connected, you can run MongoDB commands inside the shell, for example:
   ```bash
   show dbs;
   use testdb;
   db.testCollection.insertOne({ name: "test" });
   db.testCollection.find();
   ```

### Step 5: Stopping the Containers

To stop the running containers, use:
```bash
docker-compose down
```

This will stop and remove the containers created by the `docker-compose up` command.

### Full Workflow

1. **Create and configure** the `docker-compose.yml`.
2. **Run** `docker-compose up -d` to start MongoDB and Mongo Express.
3. **Access** Mongo Express via `http://localhost:8081` to interact with MongoDB using a GUI.
4. **Connect** to MongoDB using the MongoDB shell (`mongosh "mongodb://root:example@localhost:27017"`).
5. **Stop** the containers with `docker-compose down` when done.

Let me know if you need further assistance!
