To insert 10,000 records into a MongoDB instance running in a Docker container, you can use a script to bulk insert the data into a MongoDB collection. Below is an example of how you can achieve this using the MongoDB shell (`mongosh`) or using Node.js.

### Option 1: Insert 10,000 Records Using MongoDB Shell (`mongosh`)

You can use a script within the MongoDB shell to insert 10,000 records into a collection.

#### Script to Insert 10,000 Records in MongoDB Shell
1. Open the MongoDB shell (`mongosh`) and connect to your Docker container:
   ```bash
   mongosh "mongodb://root:example@localhost:27017"
   ```

2. Use the following script to create a collection and insert 10,000 documents:

```javascript
use mydatabase;

// Create a collection called 'mycollection'
db.mycollection.drop();  // Optional: drop the collection if it already exists

const bulk = db.mycollection.initializeUnorderedBulkOp();

for (let i = 0; i < 10000; i++) {
  bulk.insert({
    name: `User${i}`,
    age: Math.floor(Math.random() * 60) + 18,
    email: `user${i}@example.com`,
    created_at: new Date(),
  });
}

const result = bulk.execute();
print(`Inserted ${result.nInserted} documents.`);
```

### Explanation:
- **`initializeUnorderedBulkOp`**: This allows for bulk insertion, which is more efficient when inserting a large number of records.
- The loop generates 10,000 random records with fields like `name`, `age`, `email`, and `created_at`.

### Option 2: Insert 10,000 Records Using Node.js

You can also insert data using a Node.js script with the MongoDB driver.

#### Step 1: Set Up the Project

1. Create a directory for your project:
   ```bash
   mkdir mongodb-insert
   cd mongodb-insert
   ```

2. Initialize a Node.js project:
   ```bash
   npm init -y
   ```

3. Install the MongoDB Node.js driver:
   ```bash
   npm install mongodb
   ```

#### Step 2: Create the Node.js Script

Create a file named `insertData.js` with the following content:

```javascript
const { MongoClient } = require('mongodb');

async function main() {
  const uri = "mongodb://root:example@localhost:27017";
  const client = new MongoClient(uri);

  try {
    await client.connect();

    const database = client.db('mydatabase');
    const collection = database.collection('mycollection');

    // Prepare 10,000 documents to insert
    const documents = [];
    for (let i = 0; i < 10000; i++) {
      documents.push({
        name: `User${i}`,
        age: Math.floor(Math.random() * 60) + 18,
        email: `user${i}@example.com`,
        created_at: new Date(),
      });
    }

    // Insert the documents in bulk
    const result = await collection.insertMany(documents);
    console.log(`Inserted ${result.insertedCount} documents.`);
  } finally {
    await client.close();
  }
}

main().catch(console.error);
```

### Explanation:
- **MongoClient**: This is used to connect to MongoDB.
- **Bulk Insertion**: The script generates 10,000 records in memory and inserts them all at once using `insertMany`.

#### Step 3: Run the Script

Run the Node.js script to insert 10,000 records into MongoDB:

```bash
node insertData.js
```

This will connect to your MongoDB Docker instance running on `localhost:27017` and insert 10,000 records into the `mycollection` collection of the `mydatabase` database.

### Option 3: Using Python (Optional)

You can also use Python with the `pymongo` driver to insert 10,000 records. If you'd prefer a Python example, let me know, and I can provide that as well.

### Verifying the Data

After inserting the data, you can check the collection to confirm the records were inserted:

1. Connect to MongoDB using the shell:
   ```bash
   mongosh "mongodb://root:example@localhost:27017"
   ```

2. Query the collection:
   ```javascript
   use mydatabase;
   db.mycollection.countDocuments();
   ```

This will return `10000`, indicating that the documents were successfully inserted.

Let me know if you need further assistance with any of the steps!