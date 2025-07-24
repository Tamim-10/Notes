# Getting Started with MongoDB Atlass

## Lesson 1 : Foundations of MongoDB

Distributed Databases

 - Database is spread across multiple machines. if one machine fails other can provide data.

 - Reduces latency

latency refers to the amount of time it takes to complete a database operation

Document Model - store data in json

Flexible Schema

### Quiz

1. Which of the following best describes a document database?

A database where data is stored in JSON-like documents, allowing for flexible and polymorphic data structures
A document database stores data in JSON-like documents, which can represent complex data structures.

3. What are the primary advantages of MongoDB’s flexible schema? (Select all that apply.)

One of the main advantages of a flexible schema is that it allows each document to have a unique structure, accommodating various data types and fields within a single collection.

A flexible schema is particularly beneficial for handling unstructured or semi-structured data that doesn't fit neatly into a table.

An advantage of a flexible schema is that it permits changes and additions to the schema without requiring predefined structures, making it adaptable to evolving data requirements.

Example

// Document 1
{
  "name": "Alice",
  "email": "alice@example.com"
}

// Document 2 (extra field, different structure)
{
  "name": "Bob",
  "phone": "123-456-7890",
  "preferences": {
    "newsletter": true
  }
}

## Lesson 2: MongoDB Architecture Overview

1️⃣ Cluster
   🔹 A cluster is a group of servers (computers) working together as one system.
   🔹 It ensures that your data is always available, even if one server fails.
   🔹 Used for handling large-scale data and many users at the same time.

    └── 2️⃣ Node
        🔹 A node is a single server (machine) inside the cluster.
        🔹 It stores part of the data and helps respond to requests.
        🔹 Multiple nodes in a cluster share the work.

            └── 3️⃣ Database
                🔹 A database is a container that holds related information.
                🔹 It organizes data by topic or application.
                🔹 Example: A "School" database could store data about students, teachers, and classes.

                    └── 4️⃣ Collection
                        🔹 A collection is like a folder inside a database.
                        🔹 It holds a group of similar data items (called documents).
                        🔹 Example: A "Students" collection contains all student records.

                            └── 5️⃣ Document
                                🔹 A document is a single piece of data in a collection.
                                🔹 It’s written in JSON-like format (key-value pairs).
                                🔹 Example:
                                   {
                                     "name": "Ali Khan",
                                     "age": 20,
                                     "grade": "A"
                                   }

Node - It is One MongoDB Server Instance (mongod process)
It's a single MongoDB database server running on a machine (physical or virtual).

Cluster Types
Replica Sets helps mongodb achieve high availability through a process called replication.
Sharded Cluster supports scaling through a partitioning process called sharding.

CAP Theorm - It impossible to simultaneously gurantee availability , consistency and partition tolerance.
Consistency - Every reads recieves the most recent write or an error
Partition Tolerance - The system continues to function despite communication breakdown between nodes and network partitioning

🔺 What is CAP Theorem?

The CAP Theorem states that in a distributed system, you can only fully achieve two out of three properties:

    Consistency (C): All nodes return the same data at the same time.

    Availability (A): Every request gets a response (even if it’s not the latest data).

    Partition Tolerance (P): The system continues to work even when parts of the network fail.

In simple terms:
You can only pick two: C, A, or P — not all three at once.
🍃 MongoDB & CAP

MongoDB is a distributed NoSQL database, and by default, it supports Partition Tolerance (P). Depending on how you configure read and write concerns, you can choose between:

    CP (Consistency + Partition Tolerance)

    AP (Availability + Partition Tolerance)

📱 Real-Life Examples
1. WhatsApp Chat Example

    CP Mode: Message is shown only when all replicas are updated → consistent but slow.

    AP Mode: Message is shown immediately → fast but may be outdated.

2. Food Delivery App (like Zomato)

    CP: Order is confirmed only when all nodes agree → safe but slower.

    AP: Order is accepted quickly → fast but may overbook if nodes aren’t in sync.

3. E-commerce App (like Amazon) ✅ [Your Preferred Example]
Scenario:

    Only 1 item in stock.

    Two users try to order it at the same time from different locations (Delhi & Mumbai).

In CP Mode:

    MongoDB waits for majority of replicas to confirm the order.

    Only one order will go through.

    The system prevents overselling, but user may see a delay.

In AP Mode:

    MongoDB allows local nodes to accept orders immediately.

    Both users may place the order due to lack of sync.

    This can cause overselling.

🧾 MongoDB Config Examples
Mode	Setting	Effect
CP	writeConcern: "majority"	Strong consistency, slow, safe writes
AP	writeConcern: 1, readConcern: "local"	Fast, available, but may return stale data

mongod = MongoDB Daemon
    Daemon: A background service or process that runs on your server.
    When you start MongoDB, you are essentially launching the mongod process.

Replication - In the context of MongoDB, replication refers to the process of copying data from one server (primary) to one or more other servers (secondaries) to ensure high availability and data redundancy.

🔁 What is MongoDB Replication?

MongoDB uses a feature called a replica set to handle replication.

A replica set is a group of MongoDB servers that maintain the same data set:

    Primary: The main node that handles all write operations.

    Secondary: Nodes that replicate the data from the primary node.

    Arbiter (optional): A node that doesn't store data but helps in elections to choose a new primary if the current one fails.

✅ Why is Replication Important?

    High availability: If the primary server goes down, a secondary can automatically become the new primary.

    Data redundancy: Data is stored on multiple servers.

    Disaster recovery: If a server fails or is corrupted, others still have the data.

    Read scaling (optional): You can direct read operations to secondaries (with some caveats).

📘 MongoDB Replica Set – Text Diagram

                          [ Client / Application ]
                                   │
                                   ▼
                          ┌─────────────────┐
                          │     PRIMARY     │
                          │   Node A:27017  │
                          │  (Read/Write)   │
                          └─────────────────┘
                            ▲           ▲
                            │           │
         Replicates from    │           │   Replicates from
                            │           │
              ┌─────────────┘           └─────────────┐
              ▼                                      ▼
     ┌─────────────────┐                   ┌─────────────────┐
     │   SECONDARY     │                   │   SECONDARY     │
     │   Node B:27018  │                   │   Node C:27019  │
     │  (Read optional)│                   │  (Read optional)│
     └─────────────────┘                   └─────────────────┘

                            (Optional)
                                │
                                ▼
                      ┌─────────────────┐
                      │    ARBITER      │
                      │   Node D:xxxxx  │
                      │ (No data held)  │
                      └─────────────────┘

🔑 Legend:

    Primary: Accepts all writes and replicates data to secondaries.
    Secondaries: Sync data from primary; can serve read queries if configured.
    Arbiter: Takes part in elections to help choose a new primary if needed, but doesn't store data.

CAP Theorem 
High Availability using Replication
Consistency of read write operations
Scaling using sharding

Sharding - In the context of MongoDB, sharding is a way to horizontally scale a database by distributing data across multiple servers or clusters called shards.

What Sharding Means in MongoDB:

    MongoDB stores large datasets by splitting the data into smaller chunks.

    Each chunk is assigned to a shard (a separate MongoDB server or replica set).

    This helps handle more data and more requests than a single server could manage.

How MongoDB Sharding Works:

    Shard Key:
    You choose a shard key, which is a specific field in your documents (like userId or orderId). MongoDB uses this key to decide which shard stores each document.

    Chunks:
    Data is split into chunks based on ranges of shard key values.

    Config Servers:
    MongoDB uses config servers to keep metadata about the cluster and where chunks live.

    Query Routing:
    A mongos query router directs read/write requests to the right shard based on the shard key.

Why Use Sharding in MongoDB?

    To scale out as your data grows beyond the capacity of a single machine.

    To improve write and read throughput by distributing load.

    To support high availability and fault tolerance with replica sets on each shard.

## Example of sharding

You have 1000 users stored in a sharded MongoDB cluster:

    userId is the shard key.

    You want:

        Users with userId 1–500 → stored on Server 1

        Users with userId 501–1000 → stored on Server 2

📐 Architecture Diagram (Sharded MongoDB – 2 Servers, 1000 Users)

                +--------------------------+
                |    Instagram App         |
                |  (Mobile / Web Frontend) |
                +------------+-------------+
                             |
                    API Request (e.g. get user 786)
                             |
                    +--------v--------+
                    |     Mongos      |   <-- Query Router
                    +--------+--------+
                             |
              -------------------------------
              |                             |
       +------v-------+             +-------v-------+
       |   Shard 1    |             |   Shard 2     |
       |  (Server 1)  |             |  (Server 2)    |
       | userId 1–500 |             | userId 501–1000|
       +--------------+             +----------------+
                             |
                   +---------v---------+
                   |  Config Servers   |
                   | (Hold metadata &  |
                   | chunk mappings)   |
                   +------------------+

🔍 What Happens Internally
🔹 Collection Setup

    Collection name: users

    Shard key: { userId: 1 }

MongoDB creates chunks based on userId range:
Chunk Range	Assigned Shard
userId 1–500	Shard 1
userId 501–1000	Shard 2
🔄 Example Query Flow

Query: db.users.find({ userId: 786 })

    Request goes to mongos.

    Mongos checks config servers:

        userId: 786 falls into range 501–1000.

        That range belongs to Shard 2.

    Query is routed to Shard 2.

    Shard 2 returns the user document.

✅ Why This Setup Works Well
Advantage	Explanation
Even distribution	500 users per shard = balanced load
Faster queries	Only relevant shard is queried
Scalability	Add more shards as user base grows
Less pressure per shard	Each server handles only part of the data

### Quiz

1. which of the following statements correctly identifies the relationships among documents, collections, and databases in MongoDB? (Select one.)

A collection is a group of documents, and a database is a group of collections.
In MongoDB, a collection is a group of documents, and a database is a group of collections.

2. Which of the following best describes how replication in MongoDB provides high availability? (Select all that apply.)

 	a. Replication stores multiple copies of data across multiple nodes.

Replication stores multiple copies of data across multiple nodes, ensuring redundancy and high availability.

    c. Replication ensures that the database remains operational even if one node fails.

Replication ensures that the database remains operational even if one node fails by maintaining redundant copies of data.

## Lesson 3: Atlas Overview

MongoDB Atlas, as a database-as-a-service, removes the need for users to manually configure and maintain their database infrastructure, which is a requirement for self-managed options.

Cloud Database associated with all cloud providers.

Automatic Failover
Availability zone

1. What are key differences between MongoDB Atlas as a database-as-a-service and MongoDB's self-managed options? (Select all that apply.)

b. MongoDB Atlas provides automatic failover across regions and availability zones.

MongoDB Atlas provides automatic failover across regions and availability zones, which enhances availability and fault tolerance, a feature not inherently available in self-managed options.

c. MongoDB Atlas includes built-in support for multi-factor authentication (MFA) and role-based access control (RBAC).

MongoDB Atlas includes built-in support for multi-factor authentication (MFA) and role-based access control (RBAC), providing robust security features that are managed by the service.

d. MongoDB Atlas reduces operational overhead by offering a fully managed cloud-based infrastructure.
MongoDB Atlas reduces operational overhead by offering a fully managed cloud-based infrastructure.


2. Which of the following best describes the role of automatic failover in MongoDB Atlas? (Select one.)

c. Automatic failover allows the database to continue operating if one availability zone fails by switching to another zone.
    Automatic failover allows the database to continue operating if one availability zone fails by switching to another zone, ensuring high availability.

## Lesson 4: Deploying an Atlas Cluster

1. You are deploying your first MongoDB Atlas cluster and want to choose the most suitable option for loading a sample dataset and experimenting with simple CRUD operations. Which of the following cluster types should you consider? (Select all that apply.)

    a. M10
    Incorrect.
    M10 is a dedicated cluster tier that’s more suitable for handling large datasets and production deployments rather than for experimentation with simple CRUD operations.

    b. Serverless
    Correct.
    The serverless tier operates on a “pay for what you use” model, making it suitable for experimenting with simple CRUD operations.

    c. M0
    Correct.
    M0 is a free cluster tier that’s ideal for loading a sample dataset and experimenting with simple CRUD operations. It can be upgraded to a serverless or dedicated tier with additional capabilities if needed.

    d. Dedicated
    Incorrect.
    Dedicated clusters, at the M10 level and higher, are recommended for production deployments and high-traffic applications, rather than for experimentation with simple CRUD operations.

2. You have created an Atlas account and want to organize your resources in a way that supports development, testing, and production environments. Which of the following actions should you take? (Select all that apply.)

Correct Answer
    a. Create separate projects for development, testing, and production to isolate the environments.
    Creating separate projects for development, testing, and production helps isolate resources.

    b. Name your organization descriptively to reflect its overall purpose.
    Naming your organization descriptively to reflect its overall purpose helps to clarify its role, which can in turn help streamline management of it.

## Lesson 5: Exploring the Atlas UI

1. You’ve just upgraded from an M0 to M10 cluster. Which of the following features are you now able to use that you did not have access to previously? (Select all that apply.)

    a. Real-Time panel
    The Real-Time performance panel is a feature available on M10 and above clusters. It provides insight into metrics such as network traffic, database operations, and hardware statistics.

    c. Performance Advisor
    The Performance Advisor is available on M10 and above clusters. It offers guidance on improving cluster performance by identifying slow queries and suggesting new indexes to improve query performance.

    d. Online Archive
    The Online Archive is available on M10 and above clusters. It optimizes storage while allowing queries on infrequently accessed data.

2.  Your data is stored in an M10 cluster, and you are working in the MongoDB Atlas UI. Which of the following information can you see in the Real-Time performance panel? (Select all that apply.)

    a. Current operations running on the cluster
    Correct.
    The Real-Time performance panel in the MongoDB Atlas UI shows the current operations running on the cluster, providing insight 
    into what processes are actively using resources.

    b. Summary of cluster performance over the past month
    Incorrect.
    The Real-Time performance panel focuses on current and recent performance metrics, not historical data over extended periods, such as the past month.

    c. Hardware statistics
    Correct.
    The Real-Time performance panel provides real-time metrics for CPU, disk IOPS and memory usage, allowing you to monitor the current state of your cluster's hardware resources.

    d. Network traffic
    Correct.
    The Real-Time performance panel reports network traffic information, such as the number of current connections and number of inbound and outbound bytes per second.

# MongoDB and the Document Model

## Lesson 1: Overview of the Document Model

1. What is considered a best practice when naming fields in MongoDB documents? (Select one.)

    c. Using descriptive, unique names.
    Correct.
    Descriptive, unique field names in MongoDB documents help make the data more understandable and maintainable.

2. What is a key characteristic of MongoDB's document model that allows for handling polymorphic data, or data of different shapes and types? (Select one.)

c. Flexible schema
    Correct.
    A key characteristic of MongoDB's document model that allows for handling polymorphic data is its flexible schema, Which allows documents within a collection to have different structures.

    a. Maximum document size of 16 MB
    Incorrect.
    MongoDB documents do have a maximum document size of 16 MB, but this limit does not have an impact on its ability to handle polymorphic data.

    b. Similarity to JSON objects
    Incorrect.
    MongoDB documents are similar to JSON objects, which allows for efficient data storage and traversal. However, this trait is not directly related to its ability to handle polymorphic data.

    d. Single data type storage
    Incorrect.
    MongoDB allows for storing diverse data types within a single document.

## Lesson 2: Data Types in MongoDB


## 📌 Data Types in MongoDB

MongoDB supports a variety of data types to handle different kinds of data:

| **Data Type**     | **Type Name**    | **Example**                               | **Use Case**                              |
|------------------|------------------|--------------------------------------------|-------------------------------------------|
| String           | `String`         | `"name": "Alice"`                          | Store text                                 |
| Integer          | `Int32` / `Int64`| `"age": 30`                                 | Counters, ages                             |
| Double           | `Double`         | `"price": 19.99`                            | Prices, decimals                           |
| Boolean          | `Boolean`        | `"isActive": true`                          | Flags, status                              |
| Date             | `Date`           | `"createdAt": ISODate(...)`                | Timestamps, creation date                  |
| ObjectId         | `ObjectId`       | `"_id": ObjectId("...")`                   | Unique document ID                         |
| Embedded Document| `Document`       | `"address": { "city": "NY" }`              | Nested info (like address)                |
| Array            | `Array`          | `"tags": ["tech", "mongo"]`                | Lists, tags                                |
| Null             | `Null`           | `"middleName": null`                       | Missing values                             |
| Binary Data      | `BinData`        | `"file": BinData(...)`                     | Images, files                              |
| JavaScript Code  | `JavaScript`     | `"fn": function() { return true; }`        | Store small JS functions                   |
| Regular Expression| `Regex`         | `"pattern": /abc/i`                        | Pattern matching                           |
| Timestamp        | `Timestamp`      | `"ts": Timestamp(...)`                     | Internal Mongo usage (e.g. oplog)         |
| Decimal128       | `Decimal128`     | `"amount": NumberDecimal("123.45")`        | High-precision financial/scientific data  |



# JSON vs BSON — Developer-Focused Comparison

| Feature / Aspect          | **JSON**                                             | **BSON**                                                                   |
| ------------------------- | ---------------------------------------------------- | -------------------------------------------------------------------------- |
| **Definition**            | Human-readable, text-based data format (RFC 8259)    | Binary representation of JSON-like documents with type metadata            |
| **Format**                | UTF-8 encoded plain text                             | Binary (non-human-readable)                                                |
| **Primary Use Case**      | API communication, config files, logs, web data      | Internal data storage and transmission in MongoDB                          |
| **Parsing Speed**         | Slower – string parsing required                     | Faster – binary format is machine-optimized                                |
| **Size Efficiency**       | More compact for simple structures                   | More overhead due to length prefixes and type indicators                   |
| **Type Support**          | Basic: string, number, boolean, null, array, object  | Rich: all JSON + `Date`, `Binary`, `Decimal128`, `ObjectId`, `Regex`, etc. |
| **Precision Handling**    | Numbers as floats (limited precision)                | Exact numeric types: `Int32`, `Int64`, `Decimal128`                        |
| **Extensibility**         | Limited (doesn't support custom types)               | Highly extensible – supports complex types natively                        |
| **Readability**           | ✅ Human-readable                                     | ❌ Not human-readable                                                       |
| **Interoperability**      | Widely used across programming languages & platforms | MongoDB-specific (though open-source and language-supported)               |
| **Data Integrity**        | No inherent type enforcement                         | Type safety via metadata (each field has a type tag)                       |
| **Storage in MongoDB**    | Accepted as input/output (converted on the fly)      | Native storage format for MongoDB                                          |
| **Use Over Network**      | Ideal for REST APIs                                  | Used for MongoDB internal replication and sharding                         |
| **Example Size (Simple)** | Smaller (e.g. 100 bytes)                             | Larger (e.g. 140–160 bytes due to metadata)                                |

### Quiz

### 1. Which of the following statements about the `_id` field in MongoDB is true? (Select all that apply.)

* **a. It provides a unique identifier for each document.**
  ✅ **Correct.**
  The `_id` field in MongoDB is a unique field that’s used to identify each document in a collection. It ensures that each document has a distinct identifier and allows for efficient querying and data retrieval.

* **b. It must be defined explicitly by the user when inserting a document.**
  ❌ **Incorrect.**
  The `_id` field does not have to be explicitly defined by the user. If it’s not provided during document insertion, MongoDB will automatically generate a unique `ObjectId` for the `_id` field.

* **c. If omitted during document insertion, MongoDB generates an ObjectId automatically.**
  ✅ **Correct.**
  The `_id` field in MongoDB is necessary for each document and must be unique. If it’s not provided during document insertion, MongoDB will automatically create an `ObjectId` for the `_id` field.

* **d. It is required for every document in MongoDB.**
  ✅ **Correct.**
  Every document in MongoDB must have an `_id` field. If it’s not provided during document insertion, MongoDB will automatically create an `ObjectId` for it.

---

### 2. Which of the following data types is used for high-precision calculations and financial data in BSON? (Select one.)

* **Decimal128**
  ✅ **Correct.**
  In BSON, the `Decimal128` data type is used for high-precision calculations and financial data.

---


## Lesson 3: Managing Databases, Collections, and Documents in Atlas



1. In Atlas, how do you perform a simple query to find specific data? (Select one.)



Correct Answer



    a. Select the Browse Collections option.



    Incorrect.



    The Browse Collections option allows you to navigate your collections, but to perform a simple query in Atlas, you need to enter field-value pairs directly in the filter bar on the Collections tab.

    b. View your data in table format.



    Incorrect.



    Viewing data in table format in Atlas helps to visualize your documents, but to perform a simple query, you must enter field-value pairs in the filter bar on the Collections tab.

    c. It isn't possible to perform a simple query in Atlas.



    Incorrect.



    It is possible to perform simple queries directly in Atlas. To find specific data, you must enter field-value pairs in the filter bar on the Collections tab.

    d. Enter field-value pairs in the filter bar on the Collections tab.



    Correct.



    To perform a simple query in Atlas to find specific data, you must enter field-value pairs directly in the filter bar on the Collections tab.



##### Lesson 4: Data Relationships



1. In our movies collection, which is an example of a one-to-many relationship? (Select one.)



Correct Answer



    a. Movies and the theaters that are showing them.

    b. A movie and the studio that released it.

    c. A movie and its cast members.



    Correct.



    A movie typically has many cast members. This represents a one-to-many relationship.

    d. Theaters and the movies they are showing.



2\. In the context of a movie database application, which of the following is an example of an entity? (Select one.)



Correct Answer



    a. The rating of a movie.

    b. A document representing a movie tracked in the database application.



    Correct.



    A movie is a distinct object in the database application. We would want to have documents per movie that we keep together in a collection.



##### Lesson 5: Embedding and Referencing



1. What is the typical approach for modeling a one-to-one relationship in MongoDB, where one entity is related to exactly one other entity? (Select one.)



Correct Answer



    a. Embedding



    Correct.



    To model a one-to-one relationship in MongoDB, the typical approach is to embed the data. Embedding keeps related data together within the parent document, simplifying queries and allowing all related information to be stored and retrieved in a single document.

    b. Referencing



    Incorrect.



    Referencing is not the preferred approach for modeling a one-to-one relationship, as it involves storing related data in separate documents, which can increase complexity.

    c. Flattening



    Incorrect.



    Flattening refers to reducing nested data structures and is not specifically related to modeling a one-to-one relationship in MongoDB.

    d. Normalizing



    Incorrect.



✅ One-to-One Relationship in MongoDB: Summary

📌 Preferred Approach:



a. Embedding

🧠 Why?



    Keeps related data in the same document.



    Simplifies reads and writes.



    Ideal when both entities are always accessed together.



🔧 Example: User and Profile

✅ Embedded Model (One-to-One)



{

  "\_id": ObjectId("..."),

  "username": "johndoe",

  "email": "john@example.com",

  "profile": {

    "fullName": "John Doe",

    "age": 30,

    "bio": "Software developer."

  }

}



    User has one Profile.



    Profile is embedded inside the User document.



🧠 Use Case



Use embedding when:



    The related data is small.



    The relationship is tightly coupled.



    You want to retrieve everything in a single query.





   2.You are designing a MongoDB schema for a movie database. There are approximately 40 movie genres represented in the database. A movie will typically belong to a few genres, and each genre could include thousands of movies.



If you want to include the genre information for each movie, while also being able to query for all movies within a genre, how should you model the relationship between movies and genres from each side? (Select all that apply.)



Correct Answer



    a. Embed the genres directly within the movie document.



    Correct.



    Embedding genres within the movie document is suitable for this many-to-many relationship. Since each movie will likely not have more than a few genres, the genres can be directly embedded without fear of bloating the documents.



d. Reference the movies in each genre document using an array of movie IDs.



    Correct.



    Since each genre contains thousands of movies, referencing the movies in each genre document using an array of movie IDs is suitable for this many-to-many relationship. The document sizes can remain stable with the need for a single additional query to retrieve all of the results while allowing for independent updates to movie information.



    b. Reference the genres in the movie document using an array of genre IDs.



    Incorrect.



    Referencing the genres in the movie document using an array of genre IDs is suitable for some many-to-many relationships, but does not provide any benefit in this scenario. You would need to run a second query for just a few results, leading to slower application performance. Embedding would provide the same results in a single query.

    c. Embed the movie documents within each genre document.



    Incorrect.



    Embedding movies within each genre document is not suitable for this many-to-many relationship. Potentially thousands of movies belong to a given genre, and an array of thousands of movies can quickly consume too much space, leading to bloated documents.

 



✅ Recommended Approach



    ✅ Embed genre names inside the movie document (since each movie has only a few genres).



    ✅ Reference movie IDs in each genre document (since each genre has thousands of movies).



🎬 Movie Document (Genres Embedded)



{

  "\_id": ObjectId("64a1e1c7f1e5c8b5fca1d123"),

  "title": "Inception",

  "year": 2010,

  "genres": \["Sci-Fi", "Thriller", "Action"],

  "director": "Christopher Nolan"

}



    ✅ Easy to query movies by genre:



db.movies.find({ genres: "Sci-Fi" })



🧾 Genre Document (Movie IDs Referenced)



{

  "\_id": "Sci-Fi",

  "description": "Science Fiction movies with futuristic or speculative themes.",

  "movie\_ids": \[

    ObjectId("64a1e1c7f1e5c8b5fca1d123"),  // Inception

    ObjectId("64a1e1c7f1e5c8b5fca1d124"),  // The Matrix

    ObjectId("64a1e1c7f1e5c8b5fca1d125")   // Interstellar

    // ... potentially thousands more

  ]

}



    ✅ Easy to query all movies in the Sci-Fi genre:



const genre = db.genres.findOne({ \_id: "Sci-Fi" });

const movies = db.movies.find({ \_id: { $in: genre.movie\_ids } });



💡 Why This Works Well

Movie Side (Embedding)	Genre Side (Referencing)

Small number of genres → ideal for embedding.	Large number of movies per genre → ideal for referencing.

Simple genre-based filtering.	Easy to look up all movies in a genre using $in query.

Efficient and fast reads.	Keeps genre documents from bloating too much.







### Connecting to a MongoDB Database Using the MongoDB Shell



##### Lesson 1: Using MongoDB Connection Strings



1. What is the primary purpose of a MongoDB connection string? (Select one.)



Correct Answer



    a. To perform queries on a MongoDB cluster



    Incorrect.



    The connection string does not perform queries. Its primary purpose is to establish the connection to the cluster, not to perform the queries themselves.

    b. To connect to a MongoDB cluster and specify connection options



    Correct.



    A MongoDB connection string is used to connect to a MongoDB cluster and includes various connection-specific options. It specifies the details required for the client to establish a connection, such as the host address, port, username, password, and authentication database, and other optional settings.

    c. To update records in a MongoDB database



    Incorrect.



    The connection string does not perform updates. It establishes the database connection necessary to perform various operations, including updates.

    d. To manage user permissions in a MongoDB cluster



    Incorrect.



    Managing user permissions is an administrative task separate from the role of a connection string.



2 .Which of the following are basic components of a standard MongoDB connection string? (Select two.)



a. Hostname of the MongoDB cluster

d. Username and password for authentication



###### ✅ Example Connection String



mongodb+srv://username:password@cluster0.mongodb.net/myDatabase?retryWrites=true\&w=majority



🧩 Breakdown of Components

Part	Description

mongodb+srv://	Protocol. +srv tells the driver to use DNS to fetch server info (used with MongoDB Atlas). Use mongodb:// for non-SRV format.

username	Your MongoDB database username.

password	The password for the database user.

@cluster0.mongodb.net	Cluster address (hostname) provided by MongoDB Atlas or your server.

/myDatabase	The default database to connect to once authenticated.

?retryWrites=true	Optional query parameters — enables automatic retry of failed write operations.

\&w=majority	Write concern — ensures the write is acknowledged by the majority of nodes in the replica set.



🧪 Localhost Example (Without SRV)



mongodb://localhost:27017/myDatabase



| localhost | MongoDB is running locally. |

| 27017 | Default MongoDB port. |

| myDatabase| Name of the database to connect to. |





##### Lesson 2: Installing and Connecting to the MongoDB Shell



1. You want to connect to a local MongoDB instance using mongosh. The default port 27017 is already in use by another process. Which of the following best describes how you can connect to the MongoDB instance on a different port? (Select one.)



c. Use the mongosh command with the --port option followed by the specific port number.



Correct.



Using the --port option followed by the specific port number with the mongosh command allows you to connect to a MongoDB instance that’s running on a non-default port.



2\. You want to connect to your Atlas cluster using mongosh. Where can you find your connection string? (Select one.)



d. In the Atlas UI, by selecting the cluster and clicking on the "Connect" button



Correct.



In the Atlas UI, you can select your cluster and click the Connect button to retrieve the connection string necessary for connecting with mongosh.



##### Lesson 3: Troubleshooting Connection Errors



1. You are attempting to connect to your MongoDB Atlas cluster from a new location and encounter the following error:



MongoServerSelectionError: connection <monitor> to 34.239.188.169:27017 closed



How can you best fix this error while maintaining a strong security posture?



b. Add your current IP address in the Network Access panel in Atlas.



Correct.



To address this network access error, you need to check the Network Access panel in Atlas to ensure that your current IP address is on the access list. If it is not, you need to add it to gain access to the Atlas cluster.



d. Open the cluster to connections from any IP address in the Network Access panel in Atlas.



Incorrect.



While opening connections from anywhere would allow you to connect to the cluster, it poses significant security risks. MongoDB warns against this practice.





2\. You attempted to log into your MongoDB Atlas cluster with the username atlasAdmin but received the following error message:



MongoServerError: bad auth : Authentication failed.



How can you fix this error?



a. Check that you’re connecting to the correct database deployment.



Correct.



Even if you entered the correct username and password, if you receive an authentication error, you should confirm that you are connecting to the correct database deployment.

b. Update your IP address in the Network Access panel.



Incorrect.



Updating the IP address in the Network Access panel would resolve a network access error, not an authentication error.

c. Check that your username and password are spelled correctly in your connection string.



Correct.



Often, a simple misspelling of login credentials will result in an authentication error.

d. Ensure that the atlasAdmin user exists in the Database Access list in Atlas.



Correct.



Authentication errors can occur if the specified user does not exist in the Database Access list in Atlas. You should verify that the atlasAdmin user has been created and has the necessary permissions.





##### Lesson 4: Using the MongoDB Shell



1. When using the MongoDB Shell to interact with data, which of the following best describes how you can use a JavaScript function within a session? (Select all that apply.)



a. ✔️ Write and call JavaScript functions directly in a mongosh session.

d. ✔️ Load a file containing the function into the MongoDB Shell by using the load() method.



mongosh "mongodb+srv://cluster0.ey6uzuo.mongodb.net/messagesDB" --apiVersion 1 --username tamim



function getUsers() {

  return db.users.find({}).toArray();

}

getUsers()



function getPosts() {

  return db.posts.find({}).toArray();

}

getPosts()



db.users.aggregate(\[

  { $count: "totalUsers" }

])



db.users.aggregate(\[

  { $group: { \_id: "$status", count: { $sum: 1 } } }

])







### MongoDB Compass (Elective)







1. Which of the following tasks can you accomplish in MongoDB Compass?



    a.

    Create aggregation pipelines with the Aggregation Pipeline Builder.

    b.

    Run commands from the embedded MongoDB Shell.

    c.

    Import data from CSV or JSON files.

    d.

    Query data directly in the Compass query bar.

    e.

    All of the above.



Answer : All of the above.



2\. In Compass, you can export queries to the following supported languages: Java, Node, C#, Python 3, Ruby, Go, Rust, and PHP.



True



### Connecting to MongoDB in Node.js





##### Lesson 1: Using MongoDB Node.js Client Libraries



1 . What is the principal responsibility of a MongoDB driver? (Select one.)



b. Establishes secure connections to a MongoDB cluster and executes database operations on behalf of client applications.



Correct!



MongoDB drivers establish secure connections to a MongoDB cluster and execute database operations on behalf of client applications.





##### Lesson 2: Connecting to an Atlas Cluster in Node.js Applications



1. How many `MongoClient` instances should your application have when connecting to a single Atlas cluster? (Select one.)



b. 1



Correct!



You need only one `MongoClient` instance per Atlas cluster for your application. Having more than one `MongoClient` instance for a single Atlas cluster in your application will increase costs and negatively impact the performance of your database.



2\. Which component(s) of the connection string do you need to replace when pasting a connection string from Atlas into a code file? (Select all that apply.)



b. Password



Correct.

 

Collection names are not part of connection strings. What component(s) of the connection string in Atlas are given as placeholder values that you need to replace?

d. Username



Correct.



You must replace the <username> placeholder with your username for the Atlas cluster if the username was not automatically added to the connection string.



##### Lesson 3: Troubleshooting a MongoDB Connection in Node.js Applications



1. How can you fix the following error in a Node.js application? (Select one.)



MongoServerSelectionError: connect ECONNREFUSED 127.0.0.1:27017

    at Timeout.\_onTimeout (/Users/admin/dev/node-mongodb-native/lib/sdam/topology.js:293:38)

    at listOnTimeout (node:internal/timers:559:17)

    at processTimers (node:internal/timers:502:7) {

  reason: TopologyDescription {

    type: 'Unknown',

    servers: Map(1) { 'localhost:27017' => \[ServerDescription] },

    stale: false,

    compatible: true,

    heartbeatFrequencyMS: 10000,

    localThresholdMS: 15,

    logicalSessionTimeoutMinutes: undefined

  },

  code: undefined,

  \[Symbol(errorLabels)]: Set(0) {}

}



a. Add your IP address in the Network Access panel.



Correct!



You need to check the Network Access panel to ensure that your desired IP address is on the access list. If it isn't, you will experience a connection timeout.



2\. How can you fix the following error in a Node.js application? (Select all that apply.)



<pre>MongoServerError: bad auth : Authentication failed.</pre>



Correct Answer



    a. Update database access with the correct user credentials.



    Correct!



    You may have to update or add a database user if you are experiencing an authentication error.

    b. Update the IP address that you are connecting from.



    Incorrect.



    You might have to update your IP address in the Network Access panel if you're experiencing a network access error. This is not a network access error.

    c. Check if your username and password are spelled correctly in your connection string.



    Correct!



    A simple misspelling of login credentials will result in an authentication error.



### MongoDB CRUD Operations: Insert and Find Documents



##### Lesson 1: Inserting Documents in a MongoDB Collection



1. What methods are available in MongoDB for inserting a single document? (Select one.)



a. .insertOne()



Correct! The insertOne() method is a valid method that's included in the MongoDB Shell to insert a single document.



2\. What methods are available in MongoDB for inserting multiple documents? (Select one.)

 	a. .InsertDocument()



Incorrect. The InsertDocument() method is not a valid MongoDB method to insert documents. What method can you use to insert a document?



##### Lesson 2: Finding Documents in a MongoDB Collection



1. What methods are available in MongoDB for finding documents? (Select one.)



a. .find()



Correct! The find() method is a valid method that's included in the MongoDB Shell to find documents.



2\. You are searching for data on a small area in downtown Chicago with the following zip codes:



“60601”

“60602”

“60603”

“60604”

“60605”

“60606”



b. { zip: { $in : \[ "60601", "60602", "60603", "60604", "60605", "60606"] } }



Correct! The $in operator returns documents that contain the values specified in the array. This query will return the specified Chicago zip codes.



##### Lesson 3: Finding Documents by Using Comparison Operators





1. Your company is conducting research on the customer experience and is focused on identifying unsatisfied customers. You need to find all customers with a satisfaction rating of 1 or 2.



Which of the following query documents would return all customers with a satisfaction rating of 1 or 2? (Select one.)



d. { "customer.satisfaction" : { $lte : 2}}



Correct! This query will return all documents with a customer satisfaction rating of less than or equal to 2



2\. Your company wants to offer a special discount for customers who are 65 or older. Your task is to find the records for these customers. Which of the following queries would return documents for all customers 65 or older? (Select all that apply.)



b. { "customer.age" : { $gt : 64 }}



Correct! This query would return all documents where the customer age is greater than 64, which includes those 65 and older.

c. { "customer.age" : { $gte : 65 }}



Correct! This query would return all documents where the customer age is greater than or equal to 65.



##### Lesson 3: Finding Documents by Using Comparison Operators



1. Which of the following operators can be used to find a subdocument that matches specific criteria in an array?



b. $elemMatch



Correct! The $elemMatch operator is a valid operator that's included in the MongoDB Shell to find a subdocument that matches specific criteria in an array.





2\. What will the following query return? (Select one.)



db.books.find({ genre: "Historical" })





##### Lesson 5: Finding Documents by Using Logical Operators



1. What will the following query return? (Select one.)



db.routes.find({

  $and: \[

    { $or: \[{ dst\_airport: "IST" }, { src\_airport: "IST" }] },

    { $or: \[{ stops: 0 }, { airline.name: "Turkish Airlines"}] },

  ]

})



Correct Answer



    a. All flights departing from or landing at the Istanbul airport (IST) that are nonstop or operated by Turkish Airlines.



    Correct! This query will return all flights that either depart from or land at the Istanbul airport that are nonstop or operated by Turkish Airlines.



Lesson 5: Finding Documents by Using Logical Operators



### MongoDB CRUD Operations: Replace and Delete Documents



##### Lesson 1: Replacing a Document in MongoDB



1. Which of the following statements regarding the `replaceOne()` method for the MongoDB Shell (`mongosh`) are true? (Select all that apply.)



Correct Answer



    a. This method is used to replace a single document that matches the filter document.



    Correct! The `replaceOne()` method is used to replace a single document that matches the filter document.

    b. This method accepts a filter document, a replacement document, and an optional options document.



    Correct! The `replaceOne()` method accepts a filter document, a replacement document, and an optional options document.

    c. This method can replace multiple documents in a collection.



    Incorrect. The `replaceOne()` method is used to replace a single document that matches the filter document. Which of the other statements describe `replaceOne()`?

    d. This method returns a document containing an acknowledgement of the operation, a matched count, modified count, and an upserted ID (if applicable).



    Correct! The `replaceOne()` method returns a document containing an acknowledgement of the operation, a matched count, modified count, and an upserted ID (if applicable).



2\. you want to replace the following document from the birds collection with a new document that contains additional information on recent sightings, the scientific name of each species, and wingspan. What field should you use in the filter document to ensure that this specific document is replaced? (Select one.)



{ \_id: ObjectId("6286809e2f3fa87b7d86dccd") },

  {

    common\_name: "Morning Dove",

    habitat: \["urban areas", "farms", "grassland"],

    diet: \["seeds"]

  }



Correct Answer



    a. { \_id: ObjectId("6286809e2f3fa87b7d86dccd") }



    Correct! Including the \_id field as the filter document ensures that you’ll replace this specific document by using replaceOne().





##### Lesson 2: Updating MongoDB Documents by Using updateOne()





1 . You want to add an element to the items array field in the sales collection. To do this, what should you include in the update document? (Select one.)





A.



 { $set: { items:\[{ “name”: "tablet", “price”: 200}] } }



B.



 { $update: { items:\[{ “name”: "tablet", “price”: 200}] } }



C.



 { $push: { items:\[{ “name”: "tablet", “price”: 200}] } }



D.



 { $upsert: { items:\[{ “name”: "tablet", “price”: 200}] } }



Correct Answer



    a. Option A



    Incorrect. The $set operator replaces the value of a field with the specified value. This code example would replace the value of the items field. It would not add an element to the existing array.

    b. Option B



    Incorrect. This syntax is invalid. $update is not a MongoDB operator.

    c. Option C



    Correct! The $push operator adds an element to an array field. In this example, you will add an array element for a tablet.

    d. Option D



    Incorrect. This syntax is invalid. The upsert option can add a document to a collection if it does not already exist. upsert cannot be used to update the value of a field.





2\. Air France has recently passed inspection. In the following document, you need to update the results field from Fail to Pass. To do this, what should you include in your update document? (Select one.)



{

  \_id: ObjectId("56d61033a378eccde8a837f9"),

  id: '31041-2015-ENFO',

  certificate\_number: 3045325,

  business\_name: 'AIR FRANCE',

  date: 'Jun  9 2015',

  result: 'Fail',

  sector: 'Travel Agency - 440',

  address: {

    city: 'JAMAICA',

    zip: 11430,

    street: 'JFK INTL AIRPORT BLVD',

    number: 1

  }

}





A.



 { $set: {result: ‘Pass’} }



B.



 { $upsert: {result: ‘Pass’} }



C.



 { $insert: {result: ‘Pass’} }



D.



 { $push: {result: ‘Pass’} }



Correct Answer



    a. Option A



    Correct! The $set operator replaces the value of a field with the specified value, so using this update document would update the result field to 'Pass'.



### Lesson 3: Updating MongoDB Documents by Using findAndModify()



1. Using the zips collection, you write the following query. This query updates the population, which is stored in the pop field, in one zip code in Santa Fe, New Mexico. What will be returned? (Select one.)



db.zips.findAndModify({

  query: { \_id: ObjectId("5c8eccc1caa187d17ca72ee7") },

  update: { $set: { pop: 40000 } },

  new: true,

})



Correct Answer



    a. The updated document, which contains a population of 40000



    Correct! When the new option is set to true, findAndModify() returns the updated document. This query will return the updated document with a population of 40000.



2\. What would happen if you ran the following query on the zips collection? Note that there is currently no document for the city of Taos. (Select one.)



db.zips.findAndModify({

  query: { zip: 87571 },

  update: { $set: { city: "TAOS", state: "NM", pop: 40000 } },

  upsert: true,

  new: true,

})



Correct Answer



    a. A new document would be inserted because the new option is set to true.



    Incorrect. When the new option is set to true, the updated version of a document is returned, regardless of whether that document is new or existing.

    b. A new document would be inserted because the upsert option is set to true.



    Correct! When the upsert option is set to true, a new document will be inserted if one does not already exist. For existing documents, the upsert option will cause the document to be updated.

    c. You would receive an error, because you cannot insert a new document when using the findAndModify() method.



    Incorrect. If you use findAndModify() to insert a new document without including the upsert option, you will receive an error or a null response,and the document will not be inserted. In this example, the document is inserted because the upsert option is set to true.



##### Lesson 4: Updating MongoDB Documents by Using updateMany()





1. Three computer science classes, with the class\_ids of 377, 259, and 350, have earned 100 extra credit points by competing in a hackathon. You need to update the database so that all students who are in these classes receive extra credit points. Note that you will use the grades collection, which is in the sample\_training database.



Which of the following queries will accomplish this goal? (Select one).



b. Option B



Correct! Using db.collection.updateMany() enables you to update multiple documents at once.





##### Lesson 5: Deleting Documents in MongoDB



1 . United Airlines is the only airline that has a route from the Denver Airport (DEN) to the Northwest Arkansas Airport (XNA). It has decided to cancel this route due to low ridership.



Which of the following queries will delete the route? (Select one.)



Note that these documents are contained in the routes collection in the sample\_training database.



d. Option D



Correct! The db.collection.deleteOne() method is used to delete a single document. The filter document contains the src\_airport field with a value of “DEN” to specify a flight departing from Denver, and a dst\_airport field with a value of “XNA” to specify a flight landing in the Northwest Arkansas Airport.





2\. Air Berlin has filed for bankruptcy and ceased operations. You need to update the routes collection to delete all documents that contain an airline name of Air Berlin. Which of the following queries should you use? (Select one.)



c. Option C



Correct! This query will delete all documents that contain an airline name of Air Berlin.



### MongoDB CRUD Operations: Modifying Query Results



##### Lesson 1: Sorting and Limiting Query Results in MongoDB





1. Using the inspections collection within the sample\_training database, you need to find all inspections that were passed. Your manager has requested that you organize this data by the certificate number in ascending order. Which query should you use? (Select one).



A.



db.inspections.find( { result : "Pass" }).sort( {certificate\_number: 1});



B.



db.inspections.find( { result : "Pass" }).sort( {certificate\_number: - 1});



C.



db.inspections.find( { result : "Pass" }, { sort: { certificate\_number: 1 }})



D.



db.inspections.find( { result : "Pass" }, { sort: { certificate\_number: - 1 }})



Correct Answer



    a. Option A



    Correct! This query will return documents for businesses that passed inspection ordered by the certificate number in ascending order.



2\. You are considering creating a new membership tier for your bike sharing service for users who take long trips. Using the trips collection within the sample-training database, you need to find the trips, taken by subscribers, with the longest trip duration. Return the top 5 results in descending order. Which query should you use? (Select one.)



D. db.trips.find( { usertype: "Subscriber"}).sort( { tripduration:  - 1 }).limit(5)



##### Lesson 2: Returning Specific Data from a Query in MongoDB



1. Which of the following statements are true about a projection document? (Select all that apply.)



Correct Answer



    a. We can include fields in our results by setting their values to 1 in the projection document.



    Correct! We can include fields in our results by setting their values to 1 in the projection document.

    b. We can exclude fields from our results by setting their values to 0 in the projection document.



    Correct! We can exclude fields from our results by setting their values to 0 in the projection document.

    c. We can either include or exclude fields in the results, but not both. The \_id field is the exception to this rule.



    Correct! We can either include or exclude fields in the results, but not both. However, the \_id field is the exception to this rule.



2\. If we don’t want to return the \_id field, we can add it to the projection document and set it to which of the following values? (Select all that apply.)



Correct Answer



    a. 0



    Correct! Setting the \_id field value to 0 will exclude that field from the results.



##### Lesson 3: Counting Documents in a MongoDB Collection



1. Which of the following statements are true about the countDocuments() collection method? (Select all that apply.)



Correct Answer



    a. The method takes a query parameter, which selects the documents to be counted.



    Correct! The correct syntax for .countDocuments() is db.collection.countDocuments(<query>).

    b. We can use the method to count all documents in a collection.



    Correct! We use countDocuments() with an empty document in the query parameter to count all documents in a collection.



2\. What can we expect to be returned by running db.inspections.countDocuments({})? (Select one.)



c. This command returns the total number of documents in the inspections collection.



Correct! We use countDocuments() with an empty document in the query parameter to return the total number of documents in a collection.





Lesson 3: Counting Documents in a MongoDB Collection







1. How is BSON converted into JSON when using the MongoDB driver for Node.js? (Select one.)



a. BSON-encoded documents are converted automatically by the driver, allowing you to use the data immediately in your application.



✅ Explanation:



MongoDB stores data in BSON (Binary JSON), a binary-encoded serialization format that extends JSON with additional data types (like ObjectId, Date, etc.).



When you're using the MongoDB Node.js driver, the conversion from BSON to JSON happens automatically when data is retrieved from the database.





What makes BSON documents different from JSON documents? (Select all that apply.)



Correct Answer



    a. BSON documents are binary encoded.



    Correct!



    BSON documents are binary-encoded serialized documents. This means that they are stored in a binary format, which is more compact than text-based JSON documents. This makes them more efficient for storage and transmission.

    b. BSON documents are text encoded.



    Incorrect.



    BSON documents are encoded in binary, which means that they are not easily human-readable. BSON is easier for machines to read than JSON and is therefore more efficient.

    c. BSON documents allow for more data types than JSON documents.



    Correct!



    BSON documents allow for more data types than JSON documents. This is because BSON is a superset of JSON, which means that it can represent all the data types that JSON can represent, plus additional data types.

    d. BSON documents are more difficult for machines to read than JSON documents.



    Incorrect.



    BSON documents are more efficient for machines to read than JSON documents. BSON is a binary format, which is more compact than text-based JSON documents. This makes them more efficient for storage and transmission.



✅ Correct answers:

a. BSON documents are binary encoded.

c. BSON documents allow for more data types than JSON documents.

✅ Explanation of Each Option:

a. BSON documents are binary encoded.



✔️ True



    BSON stands for Binary JSON.



    It is a binary-encoded serialization format used internally by MongoDB.



    Being binary makes it more efficient for machine parsing and storage.



##### Lesson 2: Inserting a Document in Node.js Applications

Lesson 3: Querying a MongoDB Collection in Node.js Applications

1.You need to insert a new restaurant into the restaurants collection. The new restaurant document is stored in the variable named hyderabadiBiryani. A reference to the collection is stored in a variable called restaurants. Which of the following expressions will insert the new document into the restaurants collection? (Select one.)

A.

let result = await restaurants.insertOne(hyderabadiBiryani)

Correct!

This expression will insert a single document, stored in the hyderabadiBiryani variable, into the restaurants collection.

2. You need to find all zip codes for the city of Tulsa, Oklahoma. Which query should you use, given the following? (Select one.)

const dbname = "sample\_training"

const collection\_name = "zips"

const zipsCollection = client.db(dbname).collection(collection\_name)

//  Query
const tulsaDocuments = {"city": "TULSA"}

//  Select an expression that selects the documents matching the query constraint in the 'zips' collection.
let result =



A.

let result = await zipsCollection.find(tulsaDocuments)

Correct!

This expression will return a cursor instance of the documents for the city of Tulsa.



Lesson 4: Updating Documents in Node.js Applications



Zvents recently hired 15 new employees, bringing the total number of employees to 70. You need to update the companies collection within the sample\_training database, so the number\_of\_employees field is set to 70. Which query should you use, given the following? (Select one.)

const dbname = "sample\_training"
const collection\_name = "companies"

const companiesCollection = client.db(dbname).collection(collection\_name)

//  Filter
let documentToUpdate = {"name": "Zvents"}

//  Update
let updateEmployees = {"$set": {"number\_of\_employees": 70 }}

//  Select an expression that updates the number of employees.

let result = await companiesCollection.updateOne(documentToUpdate, updateEmployees)

Option D

Correct!

This expression will update the number of employees at Zvent to 70.



2. The companies collection is missing data on the initial public offerings (IPO) for Linkedin and Facebook. Given the Node.js file below, select the expression that sets the ipo field to true for the two companies. (Select one).

const dbname = "sample\_training"
const collection\_name = "companies"

const companiesCollection = client.db(dbname).collection(collection\_name)

//  Filter document
const selectCompanies = {"name": { "$in" : \["Facebook", "LinkedIn"]}}

//  Update document
const setIpo = {"$set": {"ipo": true}}

//  Select an expression that sets the value of "ipo" to true for Facebook and Linkedin



b. Option B

Correct!

This expression will set ipo to true for Facebook and Linkedin.



Lesson 5: Deleting Documents in Node.js Applications

1. Use this dataset to answer the question that follows:

{
"account\_id": "MDB156014571",
"account\_holder": "Adelen Værnes",
"account\_type": "savings",
"balance": 1519.62,
"transfers\_complete": \[
"TR670287839",
"TR679752211",
"TR854525844",
"TR762109284"
]
},
{
"account\_id": "MDB190468049",
"account\_holder": "Louis Lewis",
"account\_type": "savings",
"balance": 4155.67,
"transfers\_complete": \[
"TR859060098",
"TR729044189",
"TR126484922",
"TR617907396",
"TR598541455"
]
},
{
"account\_id": "MDB870205338",
"account\_holder": "Juan Perez",
"account\_type": "checking",
"balance": 1907.8,
"transfers\_complete": \[
"TR432759196",
"TR797654953",
"TR391563093",
"TR464853424",
"TR922604241"
]
},

Question: When you run the following command, what is the result? (Select one.)

const filter = { account\_type: "checking" }
db.accounts.deleteMany(filter)

Correct Answer

    a. Louis Lewis and Adelen Værnes are the only two documents left in the collection.

&nbsp;   Correct!

    The filter matches every `account\\\_type` `checking` and deletes them from the collection.










2. A bank has identified a fraudulent account with the account\_id of MDB905411541. Given the Node.js file below, which expression should you use to remove this account? (Select one.)

const dbname = "bank"
const collection\_name = "accounts"

const accountsCollection = client.db(dbname).collection(collection\_name)

//  Filter by ObjectId
let fraudAccount = {"account\_id":"MDB905411541"}

//  Select an expression that deletes the target account.



d. Option D

Correct!

This expression uses the deleteOne() method and will delete the target account.



Lesson 6: Creating MongoDB Transactions in Node.js Applications

✅ Example 1: Bank Transaction
🏦 Use Case:

A user transfers ₹1000 from Account A to Account B.
This must happen as one atomic transaction — if any part fails, the money must not move at all.
💻 Code:

const session = client.startSession();

try {
session.startTransaction();

// Deduct ₹1000 from Account A
await accountsCollection.updateOne(
{ account\_id: "ACC001" },
{ $inc: { balance: -1000 } },
{ session }
);

// Add ₹1000 to Account B
await accountsCollection.updateOne(
{ account\_id: "ACC002" },
{ $inc: { balance: 1000 } },
{ session }
);

await session.commitTransaction();
console.log("✅ ₹1000 transferred successfully");

} catch (err) {
await session.abortTransaction();
console.log("❌ Transaction failed, rollback done:", err.message);
} finally {
session.endSession();
}

✅ Example 2: E-Commerce Order + Inventory
🛒 Use Case:

When a customer places an order:

    The product stock should be reduced.

&nbsp;   An order record should be created.





If any part fails, the order should not go through.
💻 Code:

const session = client.startSession();

try {
session.startTransaction();

// Reduce stock from inventory
await inventoryCollection.updateOne(
{ productId: "P123", stock: { $gte: 1 } },
{ $inc: { stock: -1 } },
{ session }
);

// Insert new order
await orderCollection.insertOne(
{
userId: "U456",
productId: "P123",
quantity: 1,
status: "Pending"
},
{ session }
);

await session.commitTransaction();
console.log("✅ Order placed successfully");

} catch (err) {
await session.abortTransaction();
console.log("❌ Order failed, rollback done");
} finally {
session.endSession();
}

✅ Example 3: Exam Submission System
🎓 Use Case:

When a student submits an exam:

    The result is recorded in the database.

&nbsp;   The student status is updated to "Completed".





If anything fails, the student should reattempt the submission.
💻 Code:

const session = client.startSession();

try {
session.startTransaction();

// Insert exam result
await resultsCollection.insertOne(
{
studentId: "S789",
subject: "Math",
marks: 92
},
{ session }
);

// Update student status
await studentCollection.updateOne(
{ studentId: "S789" },
{ $set: { examStatus: "Completed" } },
{ session }
);

await session.commitTransaction();
console.log("✅ Exam submitted and status updated");

} catch (err) {
await session.abortTransaction();
console.log("❌ Submission failed, please try again");
} finally {
session.endSession();
}

🧠 Revision Points for MongoDB Transactions:
Step	What to Do	Command/Concept
1️⃣	Start a session	client.startSession()
2️⃣	Start a transaction	session.startTransaction()
3️⃣	Perform DB operations (insert/update/delete)	Use { session } in each operation
4️⃣	Commit the transaction (if success)	session.commitTransaction()
5️⃣	Abort transaction (if error)	session.abortTransaction()
6️⃣	End the session	session.endSession()



Use this code to answer the question that follows:

const session = client.startSession();

try {
await session.withTransaction(async () => {
await collection.deleteOne({ \_id: 1 }, { session });
await collection.deleteOne({ \_id: 2 }, { session });
await collection.updateOne({ \_id: 3 }, { $set: { name: "New Name" } }, { session });
await collection.insertOne({ \_id: 4, name: "New Document" }, { session });
});
} catch (error) {
console.log(error);
} finally {
await session.endSession();
}

Question: If one of the operations fails, what will happen to the other operations in the transaction? (Select one.)

Correct Answer

    a. The operations that did not fail will not be committed.

&nbsp;   Correct!

    If one of the operations fails, the operations that did not fail will not be committed. The entire transaction will be canceled, and no operations will be committed.










2. Which method starts the transaction, runs the callback, and then commits or cancels the transaction? (Select one.)

📘 Explanation:

withTransaction() is a helper method provided by MongoDB’s Node.js driver that:

    Starts a transaction

&nbsp;   Runs a callback function (where you perform your operations)

    Automatically commits the transaction if everything goes well

    Automatically aborts (rolls back) the transaction if an error is thrown













🔌 What Is the MongoDB Node.js Driver?

The MongoDB Node.js driver is a library (mongodb package) that allows you to interact with MongoDB databases from a Node.js application. It provides methods to connect, query, insert, update, delete, and manage transactions, indexes, and aggregation pipelines.

You install it with:

npm install mongodb

✅ 1. Connecting to MongoDB

const { MongoClient } = require('mongodb');
const uri = "mongodb://localhost:27017";
const client = new MongoClient(uri);

await client.connect();

📁 2. Accessing Database and Collections

const db = client.db("myDatabase");
const usersCollection = db.collection("users");

🔧 3. CRUD Operations
➕ Insert

await usersCollection.insertOne({ name: "Alice", age: 25 });
await usersCollection.insertMany(\[{ name: "Bob" }, { name: "Charlie" }]);

🔍 Find

const user = await usersCollection.findOne({ name: "Alice" });
const users = await usersCollection.find({ age: { $gte: 20 } }).toArray();

✏️ Update

await usersCollection.updateOne({ name: "Alice" }, { $set: { age: 26 } });

❌ Delete

await usersCollection.deleteOne({ name: "Alice" });

🔄 4. Transactions with the Node.js Driver
🛠️ Manual Transaction

const session = client.startSession();

try {
session.startTransaction();

await usersCollection.updateOne(
{ name: "Alice" },
{ $inc: { balance: -500 } },
{ session }
);

await usersCollection.updateOne(
{ name: "Bob" },
{ $inc: { balance: 500 } },
{ session }
);

await session.commitTransaction();
} catch (e) {
await session.abortTransaction();
} finally {
await session.endSession();
}

✅ withTransaction() — Recommended Method

const session = client.startSession();

try {
await session.withTransaction(async () => {
await usersCollection.updateOne(
{ name: "Alice" },
{ $inc: { balance: -500 } },
{ session }
);

    await usersCollection.updateOne(
{ name: "Bob" },
{ $inc: { balance: 500 } },
{ session }
);

});
} finally {
await session.endSession();
}

🟢 Why use withTransaction()?

    It simplifies transaction logic.

&nbsp;   Automatically commits on success.

    Rolls back on any error.

    Cleaner and less error-prone.





🧠 Summary of Key Methods
Method	Purpose
MongoClient()	Connect to MongoDB
db()	Select a database
collection()	Select a collection
insertOne(), insertMany()	Insert documents
find(), findOne()	Query documents
updateOne(), updateMany()	Update documents
deleteOne(), deleteMany()	Delete documents
startSession()	Start a new session
startTransaction()	Begin a transaction
commitTransaction()	Commit the current transaction
abortTransaction()	Rollback the transaction
withTransaction()	Simplified transaction handling
endSession()	End the session



MongoDB Indexes



Lesson 1: Using MongoDB Indexes in Collections

1. Which of the following statements about indexes are correct?

a. Indexes are data structures that improve performance, support efficient equality matches and range-based query operations, and can return sorted results.

Correct.

Indexes are data structures that improve performance, support efficient equality matches and range-based query operations, and can return sorted results. Indexes achieve this by allowing MongoDB to perform only the work necessary to return the data that is requested, rather than scanning the entire collection.
b. Indexes are automatically created based on usage patterns.

Incorrect.

While users can create indexes on the data that's used most often to help improve the performance of a slow query, indexes are not automatically created based on usage patterns. However, MongoDB Atlas provides recommendations about which indexes to create or drop.
c. Indexes are used to make querying faster for users. One of the easiest ways to improve the performance of a slow query is create indexes on the data that is used most often.

Correct.

Indexes help make querying faster for users by only scanning the indexes to find the data that is requested.
d. When using an index, MongoDB reads every document in a collection to check if it matches the query that's being run.

Incorrect.

When indexes are available, MongoDB does not need to scan the entire collection to return the data that is requested by a query. Instead, MongoDB will only scan the indexes to find the data that is requested.



Single field index: A single field index is an index on a single field of a document. MongoDB creates a single field index on the \_id field by default, but additional indexes may be needed for other fields as well. A single field index can also be a multikey index if it operates on an array field.

Multikey index: A multikey index is an index on an array field. Each element in the array gets an index key, which supports efficient querying against array fields. Both single field and compound indexes can have an array field, so there are both multikey single field indexes and multikey compound indexes.

Lesson 2: Creating a Single Field Index in MongoDB



1. What is a single field index? (Select one.)

Correct Answer

    a. An index that supports efficient querying against one field

&nbsp;   Correct. 

    A single field index is an index that supports efficient querying against a single field. By default, all collections have a single field index on the \\\_id field, but users can define additional indexes that support important queries. A single field index is also a multikey index if the value of the field is an array.










1. You have a collection of customer details. The following is a sample document from the collection:

{
"\_id": { "$oid": "5ca4bbcea2dd94ee58162a6a" },
"username": "hillrachel",
"name": "Katherine David",
"address": "55711 Janet Plaza Apt. 865\\nChristinachester, CT 62716",
"birthdate": { "$date": { "$numberLong": "582848134000" } },
"email": "timothy78@hotmail.com",
"Accounts": \[
{ "$numberInt": "462501" },
{ "$numberInt": "228290" },
{ "$numberInt": "968786" },
{ "$numberInt": "515844" },
{ "$numberInt": "377292" }
],
"tier\_and\_details": {}
}

You create a single field index on the email field, with the unique constraint set to true:

db.customers.createIndex({email:1}, {unique:true})

What would happen if you attempt to insert a new document with an email that already exists in the collection? (Select one.)

a. The new document will be inserted and replace the old document in the collection.

Incorrect.

That is not how the unique constraint operates. Unique indexes ensure that indexed fields do not store duplicate values. The new document will not be inserted in this example because the email address already exists in another document in the collection.



Lesson 3: Creating a Multikey Index in MongoDB



What is a multikey index

1. b. An index where one of the indexed fields contains an array

Correct.

Multikey indexes support efficient queries against array fields by creating an index key for each element in the array. This allows MongoDB to search for the index key of each element in the array rather than scan the entire array, which results in dramatic performance gains in your queries.

What is the maximum number of array fields per multikey index? (Select one.)

Correct Answer

    a. 1

&nbsp;   Correct! The maximum number of array fields per multikey index is 1. If an index has multiple fields, only one of them can be an array.










Lesson 4: Working with Compound Indexes in MongoDB





What is a compound index? (Select one.)

Correct Answer

    a. An index that supports queries that combine the field name and the value into one string

&nbsp;   Incorrect. 

    An index that combines the field name and the value into a single string does not exist MongoDB. Reconsider the fields that compound indexes support queries against.
    b. An index that supports queries against unknown or arbitrary fields

    Incorrect.

    An index that support queries against unknown or arbitrary fields is a wildcard index. Wildcard indexes were introduced in MongoDB 4.2. They allow users to query against fields that are not explicitly defined in the collection. Reconsider the fields that compound indexes support queries against.
    c. An index that contains references to multiple fields within a document

    Correct!

    A compound index is an index that contains references to multiple fields within a document. Compound indexes are created by adding a comma-separated list of fields and their corresponding sort order to the index definition.
    d. An index that supports queries that are run on two collections at the same time

    Incorrect. 

    An index that supports running queries on two collections simultaneously doesn't currently exist in MongoDB. Reconsider the fields that compound indexes support queries against.










2. What is the recommended order of fields in a compound index? (Select one.)

c. Equality, Sort, Range

Correct!

The recommended order of indexed fields in a compound index is Equality, Sort, and Range. Optimized queries use the first field in the index, Equality, to determine which documents match the query. The second field in the index, Sort, is used to determine the order of the documents. The third field, Range, is used to determine which documents to include in the result set.



Lesson 5: Deleting MongoDB Indexes



1. What are the ramifications of deleting an index that is supporting a query? (Select one.)

Correct Answer

    a. The performance of the query will improve.

&nbsp;   Incorrect. 

    Deleting an index that is supporting a query won’t improve the performance. However, it might cause MongoDB to have to scan the entire database to find the documents that match the query.
    b. The performance of the query will be negatively affected.

    Correct! 

    The performance of the query will be negatively affected by the deletion of the only index that is currently supporting that query. Indexes generally improve the performance and time efficiency of queries by reducing the number of times that the database needs to be accessed.










2. You have a collection of customer details. The following is a sample document from this collection:

{
"\_id": { "$oid": "5ca4bbcea2dd94ee58162a6a" },
"username": "hillrachel",
"name": "Katherine David",
"address": "55711 Janet Plaza Apt. 865\\nChristinachester, CT 62716",
"birthdate": { "$date": { "$numberLong": "582848134000" } },
"email": "timothy78@hotmail.com",
"Accounts": \[
{ "$numberInt": "462501" },
{ "$numberInt": "228290" },
{ "$numberInt": "968786" },
{ "$numberInt": "515844" },
{ "$numberInt": "377292" }
],
"tier\_and\_details": {}
}

You have an index on the email field. Here’s the command you used to create the index:

db.customers.createIndex({email:1})

Before deleting it, you want to assess the impact of removing this index on the performance of the query. To do this, which command should you use? (Select one.)

d. hideIndex()

Correct.

The hideIndex() command hides an index. By hiding an index, you'll be able to assess the impact of removing the index on query performance. MongoDB does not use hidden indexes in queries but continues to update their keys. This allows you to assess if removing the index affects the query performance and unhide the index if needed. Unhiding an index is faster than recreating it. In this example, you would use the command db.customers.hideIndex({email:1}).



📚 MongoDB Indexes – Full Summary (Revision Notes)
🔍 What is an Index?

    Index is a data structure that helps MongoDB quickly find documents without scanning the entire collection.

&nbsp;   Works just like catalogs, maps, or glossary pages in real life.





🎯 Why Use Indexes?

    ✅ Improves query performance

&nbsp;   ✅ Reduces the number of documents scanned

    ✅ Helps with sorting, filtering, and range queries

    ✅ Essential for scaling applications





🏗️ Types of Indexes:
Type	Description	Example
Single Field	Index on one field	{ email: 1 }
Compound	Index on multiple fields	{ email: 1, age: -1 }
Multikey	Index on an array field	{ tags: 1 }
Text Index	For full-text search	{ content: "text" }
Hashed Index	For sharding or hashing-based queries	{ userId: "hashed" }
⚙️ How Queries Work With \& Without Indexes
Without Index (COLLSCAN)	With Index (IXSCAN)
Full collection scan	Only index entries are scanned
All fields in each document read	Only indexed field used
Slow, resource-heavy	Fast and efficient
🛠️ Useful MongoDB Commands
Task	Command Example
Create index	db.collection.createIndex({ email: 1 })
View all indexes	db.collection.getIndexes()
Drop index	db.collection.dropIndex({ email: 1 })
Explain query performance	db.collection.explain("executionStats").find({...})
Hide index (test impact)	db.collection.hideIndex({ email: 1 })
Unhide index	db.collection.unhideIndex({ email: 1 })
🧠 Index Field Order (Compound Index Best Practice)

👉 Equality → Sort → Range

Example:

db.orders.createIndex({ status: 1, customerId: 1, orderDate: -1 })

📖 Real-Life Analogies

    📚 Library Catalog → Find books faster

&nbsp;   🛒 Supermarket Map → Go directly to the product aisle

    📖 Book Index Page → Jump to the correct page instantly





🌍 Real-World Use Cases
Application Type	Query Example	Index Needed
E-commerce	find({ category, price }).sort({ rating })	{ category, price, rating }
Hospital Management	find({ aadhar })	{ aadhar: 1 }
Banking	find({ userId }).sort({ date })	{ userId: 1, date: -1 }
Blog / News App	find({ $text: { $search: "MongoDB Index" } })	{ content: "text" }
Logistics	find({ location, status })	{ location: 1, status: 1 }
⚠️ Important Points to Remember

    Only one array field allowed in a compound multikey index

&nbsp;   Index order matters in compound indexes

    Deleting an index may hurt performance but not break the query

    Use hideIndex() to test the impact without actually deleting










MongoDB Aggregation

Lesson 1: Introduction to MongoDB Aggregation

1. Which of the following tasks cannot be completed with an aggregation pipeline? (Select one.)

Correct Answer

    a. Filtering for relevant pieces of data

&nbsp;   Incorrect. 

    You can filter for relevant pieces of data by using aggregation, but can you change the documents in the database?
    b. Finding data from outside sources

    Correct! 

    You cannot use aggregation to find data from outside sources.










2. Which command performs an aggregation operation by using an aggregation pipeline? (Select one.)

d. aggregate()

Correct!

aggregate() performs an aggregation operation by using an aggregation pipeline. aggregate() takes an array of aggregation stages to form the pipeline. An aggregation function is written as db.collection.aggregate()



Lesson 2: Using $match and $group Stages in a MongoDB Aggregation Pipeline

1. You have a collection that contains zip codes in the United States. Here’s a sample document from this collection:

\_id: ObjectId('5c8eccc1caa187d17ca6eea2')
city: "EVERGREEN"
zip: "36401"
loc: Object
y: 31.458009
x: 86.925771
pop: 8556
state: "AL"

What will the output of this aggregation pipeline be? (Select one.)

db.zips.aggregate(\[
{
$match: { "state": "CA" }
},
{
$group: { "\_id": "$zip" }
}
])

b. One document for each zip code located in California (CA)

Correct!

First, the $match stage finds all documents where the state is California (CA). Next, the $group stage groups documents according to a group key. The output of this stage is one document for each unique value of this group key. In this example, the output is one document for each zip code because the group key is the zip code.

2. Instructions: Select an answer choice and then click "See Results" to submit.

You have a collection that contains zip codes in the United States. Here’s a sample document from this collection:

\_id: ObjectId('5c8eccc1caa187d17ca6eea2')
city: "EVERGREEN"
zip: "36401"
loc: Object
y: 31.458009
x: 86.925771
pop: 8556
state: "AL"

What will the output of this aggregation pipeline be? (Select one.)

db.zips.aggregate(\[
{
$match: { "state": "TX" }
},
{
$group: { "\_id": "$city" }
}
])

Correct Answer

    a. One document for each city located in Texas (TX)

&nbsp;   Correct!

    First, the $match stage finds all documents where the state is Texas (TX). Next, the $group stage groups documents according to a group key. The output of this stage is one document for each unique value of this group key. In this example, the output is one document for each city because the group key is the city.










Lesson 3: Using $sort and $limit Stages in a MongoDB Aggregation Pipeline



1. You have a collection that contains zip codes in the United States. Here’s a sample document from this collection:

\_id: ObjectId('5c8eccc1caa187d17ca6eea2')
city: "EVERGREEN"
zip: "36401"
loc: Object
y: 31.458009
x: 86.925771
pop: 8556
state: "AL"

What will the output of this aggregation pipeline be? (Select one.)

db.zips.aggregate(\[
{
$group: { "\_id": "$pop" }
},
{
$sort: { \_id: -1 }
}
])

Correct Answer

    a. One document for the population of each zip code, sorted in random order

&nbsp;   Incorrect.

    While the $group stage groups documents according to a group key, the population of each zip code in this example, this aggregation pipeline will not return the documents in random order. What does the $sort stage do?





2. You have a collection that contains all the zip codes in the United States. Here’s a sample document from this collection:

\_id: ObjectId('5c8eccc1caa187d17ca6eea2')
city: "EVERGREEN"
zip: "36401"
loc: Object
y: 31.458009
x: 86.925771
pop: 8556
state: "AL"

What will the output of this aggregation pipeline be? (Select one.)

db.zips.aggregate(\[
{
$group: { "\_id": "$pop" }
},
{
$sort: { \_id: -1 }
},
{
$limit: { 10 }
}
])

c. 10 documents, each containing the population of a zip code as the \_id, sorted by population in descending order

Correct!

The sort order of the $sort stage is -1, which means that the $sort stage will sort documents in descending order based on population. The $limit stage limits the number of documents that are returned to 10.



Lesson 4: Using $project, $count, and $set Stages in a MongoDB Aggregation Pipeline

1.What is the main difference between $set and $project? (Select one.)

Correct Answer

    a. $set changes the values of fields. $project can show and hide fields, but it can't set values of fields.

&nbsp;   Incorrect.

    $set and $project can both create new fields as well as set and change values.
    b. $set can only create new fields, and $project can only modify existing fields.

    Incorrect.

    $set and $project can both create new fields as well as set and change values.
    c. $project, $set, and $addFields are all interchangable.

    Incorrect.

    $set is an alias for $addField, but $set and $project have a key difference.
    d. $set is used to create or change values of new or existing fields. $project can be used to create or change the value of fields, but it can also be used to specify which fields to show in the documents in the aggregation pipeline.

    Correct!

    $set and $project can both create and assign values to fields, but only $project can be used to reshape the data.










2. What does the $count stage return? (Select one.)

Correct Answer

    a. A single document with one field that contains the value set to the number of documents at this stage in the aggregation pipeline.

&nbsp;   Correct!

    The $count stage returns a document with a field named in the parameters with the value set to the number of documents at this stage in the aggregation pipeline.










Lesson 5: Using the $out Stage in a MongoDB Aggregation Pipeline

1. What does the $out stage do? (Select one).

c. Creates a new collection that contains the documents in this stage of the aggregation pipeline.

Correct!

The $out stage outputs the documents in the aggregation pipeline to a new collection.

2. What happens if you set the $out stage to output to a collection that already exists? (Select one.)

Correct Answer

    a. The existing collection is erased and replaced with the outputted documents.

&nbsp;   Correct!

    Users must be careful not to overwrite any collections unintentionally when specifying the name of the collection to output the documents to.





📊 MongoDB Aggregation – Summary
🔍 What is Aggregation?

    Aggregation is used to process multiple documents and return calculated results.

&nbsp;   Think of it as data analysis inside the database (like GROUP BY in SQL).

    It uses a pipeline of stages, where each stage transforms the data.





🏗️ Aggregation Pipeline

Structure:

db.collection.aggregate(\[
{ stage1 },
{ stage2 },
...
])

Each stage performs a specific operation.
⚙️ Common Aggregation Stages
Stage	Purpose
$match	Filter documents (like find)
$group	Group by field and calculate ($sum, $avg)
$sort	Sort results
$project	Select or reshape fields
$lookup	Join with another collection (like SQL JOIN)
$unwind	Flatten arrays into individual docs
$limit	Limit the number of results
$out	Write result into a new (or existing) collection
🔗 Real-World Use Cases
Use Case	What It Does
E-commerce dashboard	Total sales per month or city
Hospital system	Avg age of patients per disease
Banking	Recent transactions per user
Blog or search feature	Full-text search with $text and $match
Analytics/Reporting	Most popular products, top customers
🧠 Key Points to Remember

    Aggregation is like Excel formulas or SQL reports, but inside MongoDB.

&nbsp;   $match → Filters

    $group → Aggregates (sum, count, avg, etc.)

    $lookup → Joins

    $out → Writes results (overwrites collection if it exists)

    Order of stages matters in pipelines





✅ Example Pipeline:

db.orders.aggregate(\[
{ $match: { status: "delivered" } },
{ $group: { \_id: "$city", total: { $sum: 1 } } },
{ $sort: { total: -1 } }
])

📌 Output: Total delivered orders per city, sorted in descending order.



MongoDB Aggregation with Node.js

Lesson 1: Building a MongoDB Aggregation Pipeline in Node.js Applications

1. What is the aggregation framework used for? (Select one.)

Correct Answer

    a. Processing data records and returning computed results

&nbsp;   Correct! 

    Aggregation operations process data records and return computed results. When working with data in MongoDB, you may have to quickly run complex operations that involve multiple stages to gather metrics for your project. Generating reports and displaying useful metadata are just two major use cases where MongoDB aggregation operations can be incredibly useful, powerful, and flexible.





2 . Which component(s) of an aggregation pipeline do documents pass through for processing in sequence? (Select one.)

Correct Answer

    a. Aggregation stages

&nbsp;   Correct! 

    An aggregation pipeline consists of one or more stages that process documents. Each stage performs an operation on the input documents. For example, a stage can filter documents, group documents, or calculate values.










Lesson 2: Using MongoDB Aggregation Stages with Node.js: $match and $group



1. You want to find airbnb listings for an entire home or apartment, grouped by the number of bedrooms and the average price. Given the sample document from the listingsAndReviews collection below, which of the following options should you use as the first pipeline stage? (Select one.)

{
"\_id": "10116256",
"listing\_url": "https://www.airbnb.com/rooms/10116256",
"name": "GOLF ROYAL RESIDENCE SUİTES(2+1)-2",
"summary": "A BIG BED ROOM WITH A BIG SALOON INCLUDING A NICE BALAKON TO HAVE SOME FRESH AIR . OUR RESIDENCE SITUATED AT THE CENTRE OF THE IMPORTANT MARKETS SUCH AS NİŞANTAŞİ,OSMANBEY AND TAKSIM SQUARE,",

....
"room\_type": "Entire home/apt",
...
"bedrooms": 2,
...
"price": {
"$numberDecimal": "997.00"
}}

A.

{"$group": {"\_id": "$bedrooms", "avg\_price": {"$avg": "$price"}}

B.

{"$match": {"room\_type": "Entire home/apt"}}

C.

{"$group": {"room\_type": "Entire home/apt"}}

D.

{"$match": {"bedrooms": "price"}}

Correct Answer

    a. Option A

&nbsp;   Incorrect.

    First, you should use a $match stage to match all documents with a room type of "Entire home/apt."
    b. Option B

    Correct!

    First, you'll use a $match stage to match all documents with a room type of "Entire home/apt."





2. You are building an aggregation pipeline to find airbnb listings for an entire home or apartment, grouped by the number of bedrooms and the average price.

You have successfully built the first $match stage, as seen below. Which option below will allow you to complete the pipeline in a second stage? (Select one.)

const pipeline = \[
// Match all documents for an entire home or apt
{"$match": {"room\_type": "Entire home/apt"}},
// Stage 2: Group by number of bedrooms and average price

]

A.



{"$group": {"\_id": "$bedrooms", "avg\_price": {"$avg": "$price"}}}



Lesson 3: Using MongoDB Aggregation Stages with Node.js: $sort and $project



1.You've written an aggregation pipeline that matches all airbnb listings for entire homes, groups by number of bedrooms and determines the average price. Your final step in this pipeline is to return the results in ascending order, so the lowest average price is returned first. Given the code below, which option should you use to complete the sort stage? (Select one.)

const pipeline = \[
// Match all documents for an entire home or apt
{"$match": {"room\_type": "Entire home/apt"}},
// Stage 2: Group by number of bedrooms and average price
{"$group": {"\_id": "$bedrooms", "avg\_price": {"$avg": "$price"}}},
// Stage 3: Return results in ascending order



]

🔹 Sample Data

\[
{ "room\_type": "Entire home/apt", "bedrooms": 1, "price": 100 },
{ "room\_type": "Entire home/apt", "bedrooms": 2, "price": 150 },
{ "room\_type": "Entire home/apt", "bedrooms": 1, "price": 120 },
{ "room\_type": "Entire home/apt", "bedrooms": 3, "price": 300 },
{ "room\_type": "Entire home/apt", "bedrooms": 2, "price": 130 },
{ "room\_type": "Private room",      "bedrooms": 1, "price": 80 }
]

Answer
🔹 Stage 1: $match

{ "$match": { "room\_type": "Entire home/apt" } }

This filters the documents to:

\[
{ "bedrooms": 1, "price": 100 },
{ "bedrooms": 2, "price": 150 },
{ "bedrooms": 1, "price": 120 },
{ "bedrooms": 3, "price": 300 },
{ "bedrooms": 2, "price": 130 }
]

Notice: The private room is excluded.
🔹 Stage 2: $group

{
"$group": {
"\_id": "$bedrooms",
"avg\_price": { "$avg": "$price" }
}
}

Now, we group by bedrooms and compute average price:

\[
{ "\_id": 1, "avg\_price": 110 },  // (100 + 120) / 2
{ "\_id": 2, "avg\_price": 140 },  // (150 + 130) / 2
{ "\_id": 3, "avg\_price": 300 }   // Only one listing
]

🔹 Stage 3: $sort

{ "$sort": { "avg\_price": 1 } }

This sorts the results in ascending order of average price:

\[
{ "\_id": 1, "avg\_price": 110 },
{ "\_id": 2, "avg\_price": 140 },
{ "\_id": 3, "avg\_price": 300 }
]

✅ Final Answer:

To sort the results so that the lowest average price is first, use:

{ "$sort": { "avg\_price": 1 } }



2. You've identified that you want to stay in an entire home with 3 bedrooms. In this new pipeline, you need to add one stage that does the following:

   return the description and listing\_url fields

   calculate a new total\_price field that adds the price and cleaning fee

   suppress the \_id field

   Which option below will complete the second stage? (Select one.)

   

   const pipeline = \[
   // Match all documents for an entire home or apt
   {"$match": {"room\_type": "Entire home/apt", "bedrooms": 3}},
   // Stage 2: Return description and listing\_url fields, calculated total\_price field, and suppress the \_id field

   

   ]

   A.

   {"$project": {"description": 1,"listing\_url": 1,"\_id": 0,"total\_price": {"$sum": \["$price", "$cleaning\_fee"]}}}

   B.

   {"$project": { description: 0,listing\_url: 0}}

   C.

   {"$return": {description: 1,listing\_url: 1,\_id: 0,total\_price: {$sum: \['$price', '$cleaning\_fee']}}}

   D.

   {"$return": { description:0 ,listing\_url: 0}}

   Correct Answer

       a. Option A

   Correct!

          This $project stage returns the description and listing\\\_url. It calculates a new field, total\\\_price that sums the price and cleaning fee. Finally, it excluded the \\\_id field from the output.

      

      
   

   

   

   MongoDB ACID Transactions

   

   Atomicity: Guarantees that every transaction is "all or nothing" when committing data to a database. For example, we don't want money to be taken from an account but not successfully added to another.

   Consistency: Guarantees that the data written to the database is consistent with database constraints. For example, if an account balance cannot be less than 0, a transaction would fail before violating this constraint.

   Isolation: Guarantees that each transaction that is run concurrently leaves the database in the same state as if the transactions were run sequentially. In other words, multiple transactions can happen at the same time without affecting the outcome of the other transactions.

   Durability : This property guarantees that data is never lost.

   Which of the following is the best definition of an ACID transaction?

   

   c. A group of database operations that must happen all together or not at all.

   Correct!

   An ACID transaction involves a group of database operations that must happen together successfully or not at all.

   

1. Which of the following scenarios require the use of an ACID transaction? (Select all that apply.)

   Correct Answer

       a. Updating a bank database to reflect the transfer of money from Customer A's bank account into Customer B's bank account.

   Correct! 

          In the case of a bank account transfer, the database operations MUST happen atomically. This scenario requires the use of ACID properties to guarantee that all operations happen successfully and securely, or that they don't happen at all.
          b. Updating inventory and shopping cart records when a customer adds an item to their online shopping cart in an ecommerce app.

          Correct! 

          In most scenarios involving the transfer of value, inventory, or ownership of goods, database operations MUST happen atomically. This scenario requires the use of ACID properties to guarantee that both the inventory and shopping cart records are updated together or not at all.

      

      
   

   

   

   Lesson 2: ACID Transactions in MongoDB

   

   1 . Which of the following statements are true about multi-document transactions in MongoDB? (Select all that apply.)

   Correct Answer

       a. Database operations that affect more than one document, like .updateMany(), are not inherently atomic in MongoDB and must be completed by using a multi-document transaction in order to have ACID properties.

   Correct! 

          A multi-document must be used to ensure that database operations that affect more than one document in a MongoDB database have ACID properties.
          b. Multi-document transactions should be treated as a precise tool that is used only in certain scenarios.

          Correct! 

          Multi-document transactions can incur a performance cost. It's important to follow best practices and use multi-document transactions only when ACID properties are essential.
          c. Using a multi-document transaction with a MongoDB database ensures that the database will be in a consistent state after running a set of operations on multiple documents.

          Correct! 

          Using a multi-document transaction guarantees that operations have ACID properties and, therefore, that the database will be in a consistent state once the transaction is complete.

      

      
   

   

   

1. Nadia needs to update customer account balances across multiple collections in MongoDB. It's important that the database operations used in this transaction adhere to ACID properties. Should Nadia use a transaction in this scenario? (Select one.)

   b. Nadia does need to use a transaction in this scenario because multi-document operations are NOT inherently atomic in MongoDB.

   Correct!

   Nadia should use a multi-document transaction in this scenario. Unlike single-document operations, multi-document operations are not inherently atomic in MongoDB. Because the group of database operations will be performed on multiple documents and require ACID properties, they should use a multi-document transaction.

   

   Lesson 3: Using Transactions in MongoDB

   1.You are creating a transaction that does the following:

       Inserts a new savings account into the accounts collection for an existing customer

   Funds the new savings account with $200 from their checking account
      
   

   

   The transaction looks like this:

   const session = db.getMongo().startSession()

   session.startTransaction()

   const account = session.getDatabase('bank').getCollection('accounts')

   account.insertOne({
   account\_id: "MDB361428849",
   account\_holder: "Donna Wood",
   account\_type: "savings",
   balance: 200.0,
   transfers\_complete: \[],
   last\_updated: new Date()
   })
   account.updateOne( { account\_id: "MDB919841472" }, {$inc: { balance: -200.00 }})

   

   What method should you use to complete this transaction? (Select one.)

   b. commitTransaction()

   Correct!

   We use .commitTransaction() to commit a transaction.session.commitTransaction() should be placed at the end of the transaction.

   Example

   const { MongoClient } = require('mongodb');

   async function transferFunds() {
   const client = new MongoClient('mongodb://localhost:27017'); // 🔗 MongoDB connection string

   // 🔁 Session start karte hain
   const session = client.startSession();

   try {
   // 📡 MongoDB server se connect karte hain
   await client.connect();

       const db = client.db('bank');
   const accounts = db.collection('accounts');

   // 🔄 Transaction shuru karte hain
          session.startTransaction();

          // 1️⃣ Pehle checking account se $200 deduct karo
          const deductResult = await accounts.updateOne(
            { account\\\_id: "MDB919841472" },
            { $inc: { balance: -200 } },
            { session }
          );

          // ❗ Check karo ke deduct hua bhi ya nahi
          if (deductResult.modifiedCount !== 1) {
            throw new Error("Failed to deduct from checking account");
          }

          // 2️⃣ Ab naya savings account insert karo jisme $200 credit ho
          await accounts.insertOne(
            {
              account\\\_id: "MDB361428849",
              account\\\_holder: "Donna Wood",
              account\\\_type: "savings",
              balance: 200.0,
              transfers\\\_complete: \\\[],
              last\\\_updated: new Date()
            },
            { session }
          );

          // ✅ Sab kuch sahi hai to commit karo
          await session.commitTransaction();
          console.log("Transaction successful: $200 transferred and savings account created.");
      
   

   

   } catch (error) {
   // ❌ Agar koi error aaye to rollback karo
   await session.abortTransaction();
   console.error("Transaction failed:", error.message);
   } finally {
   // 🧹 Session aur connection dono close karo
   await session.endSession();
   await client.close();
   }
   }

   // ⏱ Function call (real execution)
   transferFunds();

   

1. Which of the following commands will output to the shell if they are successful?

   Correct Answer

       a. .startTransaction()

   Incorrect. .startTransaction() will not give any output if it's successful.
          b. .commitTransaction()

          Correct! If it's successful, .commitTransaction() will output a message to the shell that resembles the following:

          {
            ok: 1,
            '$clusterTime': {
              clusterTime: Timestamp({ t: 1647437775, i: 5 }),
              signature: {
                hash: Binary(Buffer.from("b0d88d5a96372efb9af22021cdd59021741ddb5c", "hex"), 0),
                keyId: Long("7019511514256113665")
              }
            },
            operationTime: Timestamp({ t: 1647437775, i: 5 })
          }

          c. .abortTransaction()

          Incorrect. .abortTransaction() will not give any output if it's successful.

      
   
# TERMS
Polymorphic data structures in MongoDB mean that a single collection can store documents of different shapes or schemas.

// Student user
{
  "type": "student",
  "name": "Alice",
  "grade": "A",
  "courses": ["Math", "Science"]
}

// Teacher user
{
  "type": "teacher",
  "name": "Mr. Smith",
  "subject": "Math",
  "yearsOfExperience": 10
}

| **Type**                 | **Definition**                                                       | **Examples**                       |
| ------------------------ | -------------------------------------------------------------------- | ---------------------------------- |
| **Structured Data**      | Data organized in a fixed format (like rows and columns).            | SQL databases, Excel sheets        |
| **Semi-Structured Data** | Data with some structure but flexible and not rigid like SQL tables. | JSON, XML, CSV, MongoDB            |
| **Unstructured Data**    | Data with no predefined format or structure.                         | Images, videos, emails, plain text |
| **Metadata**             | Data that describes other data (data about data).                    | File name, size, creation date     |
| **Quantitative Data**    | Numerical data that can be measured or counted.                      | Height, weight, number of items    |
| **Qualitative Data**     | Descriptive data that classifies or labels characteristics.          | Color, gender, satisfaction level  |


               ┌────────────────────┐
               │     Types of Data  │
               └────────────────────┘
                        ↓
      ┌────────────┬────────────┬─────────────┐
      │ Structured │ Semi-      │ Unstructured│
      │  Data      │ Structured │    Data     │
      └────────────┴────────────┴─────────────┘
            ↓             ↓             ↓
         Tables       JSON, XML       Text, Images
         SQL DBs      NoSQL DBs       Audio, Video

        ┌────────────────────────┐
        │ Other Classifications: │
        └────────────────────────┘
           ↓                     ↓
  Quantitative Data       Qualitative Data
  (Numbers, e.g. age)      (Labels, e.g. color)
