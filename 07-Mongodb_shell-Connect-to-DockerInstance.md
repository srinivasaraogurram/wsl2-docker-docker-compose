To install MongoDB shell (`mongosh`) on WSL2 Ubuntu and connect to the MongoDB instance running in a Docker container, follow these steps:

### Step 1: Install MongoDB Shell on WSL2 Ubuntu

1. **Import the MongoDB GPG Key**:
   Run the following command to import the MongoDB public GPG key:
   ```bash
   wget -qO - https://pgp.mongodb.com/server-6.0.asc | sudo tee /etc/apt/trusted.gpg.d/mongodb-server.asc
   ```

2. **Create a MongoDB Sources List**:
   Add the MongoDB repository to your `apt` sources list:
   ```bash
   echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu $(lsb_release -cs)/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
   ```

3. **Update Your Package List**:
   After adding the repository, update your package list:
   ```bash
   sudo apt update
   ```

4. **Install MongoDB Shell**:
   Install the MongoDB shell package:
   ```bash
   sudo apt install mongosh
   ```

5. **Verify Installation**:
   Check that `mongosh` was successfully installed by running:
   ```bash
   mongosh --version
   ```

### Step 2: Connect to MongoDB Running in Docker

#### Option 1: Connecting from WSL2 (Ubuntu) to MongoDB Running in Docker on the Same Machine

1. **Start Docker and Docker Compose**:
   Make sure your Docker containers are up and running using:
   ```bash
   docker-compose up -d
   ```

2. **Find the IP Address of the Docker Host**:
   WSL2 and Docker Desktop share the same network interface, so you can connect to `localhost` to access the Docker containers.

3. **Connect to MongoDB via MongoDB Shell**:
   Run the following command to connect to the MongoDB instance running inside the Docker container:
   ```bash
   mongosh "mongodb://root:example@localhost:27017"
   ```

   This command uses:
   - `localhost` as the hostname, since Docker is running on your local machine.
   - `root` as the username and `example` as the password, which were set in your `docker-compose.yml` file.
   - `27017` as the default MongoDB port.

   **Example of a successful connection:**
   ```bash
   Current Mongosh Log ID: 62a3eabcd1234567890
   Connecting to: mongodb://root:<password>@localhost:27017
   MongoDB server version: 5.x.x
   ```

#### Option 2: Connecting to MongoDB from Other Devices

If you are connecting to MongoDB from another machine or want to expose the MongoDB service to your local network, ensure you are forwarding the ports correctly in your Docker setup.

1. Check that your `docker-compose.yml` maps port `27017` on the host machine (your local machine) to port `27017` inside the container.

```yaml
ports:
  - "27017:27017"
```

2. Ensure that you are connecting using the correct IP address or domain name of the host machine. Replace `localhost` with the host's IP address if connecting from a different machine.

```bash
mongosh "mongodb://root:example@<host-ip>:27017"
```

### Step 3: Test MongoDB Connection

Once connected to MongoDB via the shell, you can test your connection by running some commands:

```bash
show dbs;
use testdb;
db.testCollection.insertOne({ name: "test" });
db.testCollection.find();
```

These commands should run without errors if the connection is successful.

### Summary:

1. **Install MongoDB Shell (`mongosh`)** on WSL2 Ubuntu via MongoDB's official repository.
2. **Start your MongoDB Docker container** using `docker-compose up -d`.
3. **Connect using MongoDB Shell** by running `mongosh "mongodb://root:example@localhost:27017"`, where `localhost` is the Docker host and `root/example` are the credentials.

Let me know if you need further assistance!