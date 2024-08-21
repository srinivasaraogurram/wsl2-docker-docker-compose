To persist data in MongoDB when running in a Docker container using Docker Compose, you can configure a **volume**. This volume will store the MongoDB data on your host machine, allowing it to persist even if the container is stopped or removed.

### Step 1: Modify the `docker-compose.yml` to Include Volumes

Add a volume to your `docker-compose.yml` file to persist MongoDB data. Here's how you can do it:

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
    volumes:
      - mongodb_data:/data/db  # Persist MongoDB data

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

volumes:
  mongodb_data:
    driver: local
```

### Explanation of Changes:

1. **Volumes Section under `mongodb` Service:**
   - `volumes: - mongodb_data:/data/db`: This maps the container’s `/data/db` directory (where MongoDB stores its data) to a named volume called `mongodb_data`. This volume will persist on the host machine.

2. **Top-Level `volumes` Key:**
   - The `volumes:` section at the bottom defines the named volume `mongodb_data`. Docker will create and manage this volume on your host machine.

### Step 2: Start the Containers

Run the following command to start the containers:
```bash
docker-compose up -d
```

Docker will create the named volume (`mongodb_data`) if it doesn’t already exist and use it to store MongoDB’s data.

### Step 3: Verify the Volume

1. **Check the Volume in Docker:**
   List all Docker volumes to confirm that `mongodb_data` has been created:
   ```bash
   docker volume ls
   ```
   You should see `mongodb_data` listed as one of the volumes.

2. **Inspect the Volume:**
   You can inspect the volume to see its details:
   ```bash
   docker volume inspect mongodb_data
   ```

### Step 4: Test Data Persistence

1. **Insert Data into MongoDB:**
   Connect to the MongoDB instance and insert some data:
   ```bash
   mongosh "mongodb://root:example@localhost:27017"
   ```

   Insert a document into a collection:
   ```bash
   use testdb;
   db.testCollection.insertOne({ name: "persistent test" });
   ```

2. **Stop and Remove the MongoDB Container:**
   Stop and remove the MongoDB container:
   ```bash
   docker-compose down
   ```

3. **Start the Containers Again:**
   Bring the containers back up:
   ```bash
   docker-compose up -d
   ```

4. **Verify the Data:**
   After restarting, connect to MongoDB again and check if the data is still there:
   ```bash
   mongosh "mongodb://root:example@localhost:27017"
   use testdb;
   db.testCollection.find();
   ```

You should see the data that was inserted before you stopped the container, confirming that the data was persisted.

### Summary:

1. Modify the `docker-compose.yml` file to include a volume mapping to `/data/db`.
2. Use `docker-compose up -d` to start the containers with the persistent volume.
3. Verify that the volume persists data by stopping and restarting the containers.

Let me know if you need further assistance!