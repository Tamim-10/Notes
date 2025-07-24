#### What is GraphQL?



GraphQL is a query language for APIs and a runtime to execute those queries. It was developed by Facebook and released as an open-source project.



&nbsp;   Instead of multiple endpoints like in REST APIs, GraphQL exposes a single endpoint.

&nbsp;   The client can specify exactly what data it needs, nothing more, nothing less.

&nbsp;   The server responds with precisely the requested data.

&nbsp;   This leads to more efficient data fetching, especially useful for complex or nested data.



#### Key Features of GraphQL



&nbsp;   Flexible queries: Clients define the shape of the response.



&nbsp;   Single endpoint: One URL to get any data you want.



&nbsp;   Strongly typed schema: The API defines types and relationships.



&nbsp;   Real-time updates: Supports subscriptions for live data.



#### Why is GraphQL used?  



GraphQL is designed to solve some common problems faced by REST APIs:



1\. Over-fetching and under-fetching of data



&nbsp;   Over-fetching: REST APIs often return more data than the client actually needs. This wastes bandwidth and processing.



&nbsp;   Under-fetching: Sometimes you need data from multiple endpoints, so the client has to make several requests.



GraphQL fixes this by letting the client specify exactly what data it wants in a single query.



2\. Single endpoint



&nbsp;   REST APIs usually have multiple endpoints (e.g., /users, /users/1/posts, /posts/5/comments).



&nbsp;   GraphQL uses one endpoint (e.g., /graphql) to access all related data, making API management simpler.



3\. Faster development and better flexibility



&nbsp;   Frontend developers can get the data they need without waiting for backend changes.



&nbsp;   The schema acts as a contract, making API usage clear and self-documenting.



Example: Over-fetching in REST vs Efficient fetching in GraphQL



Imagine a mobile app showing a user's profile with their name, age, and the titles of their recent posts.

REST Approach



To get all that data, the client might need to:



&nbsp;   Call /users/1 to get user info:



{

&nbsp; "id": 1,

&nbsp; "name": "Alice",

&nbsp; "age": 30,

&nbsp; "email": "alice@example.com",

&nbsp; "address": "123 Main St",

&nbsp; "phone": "555-1234"

}



&nbsp;   But the app only needs name and age, so it’s over-fetching the email, address, and phone.



&nbsp;   Then call /users/1/posts to get their posts:



\[

&nbsp; { "id": 101, "title": "GraphQL Intro", "content": "..." },

&nbsp; { "id": 102, "title": "Advanced GraphQL", "content": "..." }

]



&nbsp;   This requires two separate requests.



GraphQL Approach



With GraphQL, the client sends a single query:



{

&nbsp; user(id: "1") {

&nbsp;   name

&nbsp;   age

&nbsp;   posts {

&nbsp;     title

&nbsp;   }

&nbsp; }

}



The server responds with exactly what’s requested:



{

&nbsp; "data": {

&nbsp;   "user": {

&nbsp;     "name": "Alice",

&nbsp;     "age": 30,

&nbsp;     "posts": \[

&nbsp;       { "title": "GraphQL Intro" },

&nbsp;       { "title": "Advanced GraphQL" }

&nbsp;     ]

&nbsp;   }

&nbsp; }

}



&nbsp;   No extra data is sent (no email, address, phone).



&nbsp;   One request instead of two.



&nbsp;   Efficient and flexible!



#### What problem does GraphQL solve?



###### 1\. Over-fetching Data

Problem:



With REST APIs, sometimes you get more data than you need. The endpoint returns a fixed structure, including fields the client never uses.

Example:



Suppose you have a REST API endpoint:



GET /users/123



Response:



{

&nbsp; "id": "123",

&nbsp; "name": "Alice",

&nbsp; "email": "alice@example.com",

&nbsp; "address": {

&nbsp;   "street": "123 Main St",

&nbsp;   "city": "Wonderland",

&nbsp;   "zip": "12345"

&nbsp; },

&nbsp; "profilePicture": "url-to-image",

&nbsp; "createdAt": "2023-01-01"

}



If your app only needs the user’s name, you’re forced to download everything, which wastes bandwidth and processing time.

How GraphQL solves this:



You ask for only the data you need in a single query:



{

&nbsp; user(id: "123") {

&nbsp;   name

&nbsp; }

}



Response:



{

&nbsp; "data": {

&nbsp;   "user": {

&nbsp;     "name": "Alice"

&nbsp;   }

&nbsp; }

}



No extra data, only what you asked for.



###### 2\. Under-fetching Data

Problem:



Sometimes, one REST API call doesn’t give you enough data. You must make multiple requests to gather related info.

Example:



You want a user's name and their posts’ titles.



REST calls:



&nbsp;   GET /users/123 → returns user info (no posts)



&nbsp;   GET /users/123/posts → returns the list of posts



Two requests needed, which slows down your app.

How GraphQL solves this:



One query fetches related data together:



{

&nbsp; user(id: "123") {

&nbsp;   name

&nbsp;   posts {

&nbsp;     title

&nbsp;   }

&nbsp; }

}



One request, all needed data.



###### 3\. Multiple Round-Trips (Performance)

Problem:



Multiple REST calls add network overhead and increase latency, especially on mobile or slow networks.



Example: For a social media app, to display a feed with users and their comments, you might need:



&nbsp;   GET /feed (list of post IDs)



&nbsp;   GET /posts/{id} for each post



&nbsp;   GET /comments?postId={id} for each post’s comments



Lots of requests!

How GraphQL solves this:



With GraphQL, you write a single query to fetch the feed with posts and comments:



{

&nbsp; feed {

&nbsp;   id

&nbsp;   content

&nbsp;   comments {

&nbsp;     id

&nbsp;     text

&nbsp;   }

&nbsp; }

}



One query = fewer requests = faster loading.



4\. Rigid API Structure / Versioning

Problem:



REST APIs often version their endpoints when clients need new data fields, e.g.:



&nbsp;   /v1/users/123 → no phone number



&nbsp;   /v2/users/123 → adds phone number



Clients must update to use new endpoints, which complicates maintenance.



How GraphQL solves this:



GraphQL’s flexible query lets clients ask for new fields without changing the API version:



If you want the phone number:



{

&nbsp; user(id: "123") {

&nbsp;   name

&nbsp;   phoneNumber

&nbsp; }

}



The server adds the new field to the schema, but old clients keep working without any version change.



###### Summary Table

Problem	REST 		Behavior				GraphQL 					Solution

Over-fetching		Returns unnecessary data		Clients request exactly what they want

Under-fetching		Need multiple requests			One query for related data

Multiple round-trips	Several network calls			Single efficient query

Rigid versioning	New API versions needed for changes	Flexible schema evolves without versions





#### 🚀 How GraphQL Works Internally (Step by Step)



At a high level, GraphQL goes through four main steps when handling a request:

1\. Client Sends a Query



The client sends a query to the GraphQL server through a single endpoint (usually POST /graphql).

Example Query:



{

&nbsp; user(id: "1") {

&nbsp;   name

&nbsp;   posts {

&nbsp;     title

&nbsp;   }

&nbsp; }

}



2\. GraphQL Server Parses and Validates the Query



Internally, the server does:



&nbsp;   Parsing: Converts the query string into an abstract syntax tree (AST).



&nbsp;   Validation: Checks the query against the schema to ensure:



&nbsp;       All fields exist



&nbsp;       Types are used correctly



&nbsp;       Required arguments are provided



&nbsp;   If the query is invalid, GraphQL returns an error immediately.



3\. Resolver Functions Are Called



Every field in the query is resolved by a corresponding resolver function.



&nbsp;   Resolvers are functions written by the developer that tell the server how to fetch the data.



&nbsp;   They can fetch from databases, REST APIs, services, or hardcoded values.



Example Schema and Resolvers:



// Schema (GraphQL SDL)

type User {

&nbsp; id: ID

&nbsp; name: String

&nbsp; posts: \[Post]

}



type Post {

&nbsp; id: ID

&nbsp; title: String

}



type Query {

&nbsp; user(id: ID!): User

}



// Resolvers (in Node.js using Apollo Server)

const resolvers = {

&nbsp; Query: {

&nbsp;   user: (parent, args, context) => {

&nbsp;     return getUserById(args.id); // fetch from DB or elsewhere

&nbsp;   },

&nbsp; },

&nbsp; User: {

&nbsp;   posts: (parent) => {

&nbsp;     return getPostsByUserId(parent.id); // use parent.id

&nbsp;   },

&nbsp; },

};



&nbsp;   Each resolver gets:



&nbsp;       parent: The result from the parent field



&nbsp;       args: Arguments passed to the field



&nbsp;       context: Shared object (e.g., auth info, DB connection)



&nbsp;       info: Query structure info



4\. Response Is Constructed and Returned



&nbsp;   After resolvers return data, the GraphQL server assembles the result in the same shape as the query.



&nbsp;   The final JSON response is returned to the client.



Final Response:



{

&nbsp; "data": {

&nbsp;   "user": {

&nbsp;     "name": "Alice",

&nbsp;     "posts": \[

&nbsp;       { "title": "GraphQL Basics" },

&nbsp;       { "title": "GraphQL Advanced" }

&nbsp;     ]

&nbsp;   }

&nbsp; }

}



##### 🧠 Internal Workflow Summary



&nbsp;   Client query sent →



&nbsp;   Parsed into AST →



&nbsp;   Validated against schema →



&nbsp;   Resolvers executed →



&nbsp;   Data assembled →



&nbsp;   Returned as JSON



🔁 Bonus: How Nested Queries Work



Resolvers are called recursively based on the depth of the query.



{

&nbsp; user(id: "1") {

&nbsp;   name

&nbsp;   posts {

&nbsp;     title

&nbsp;   }

&nbsp; }

}



&nbsp;   Query.user() resolver is called first.



&nbsp;   Then for each post, User.posts() resolver is called.



&nbsp;   Then Post.title field is resolved (automatically or through custom logic).





#### 🔷 GraphQL (The Specification)



&nbsp;   What it is: A query language for APIs and a runtime for executing those queries with your existing data.



&nbsp;   Created by: Facebook in 2012, released publicly in 2015.



&nbsp;   Purpose: To allow clients to request only the data they need from an API.



&nbsp;   Key Features:



&nbsp;       Strongly typed schema



&nbsp;       Hierarchical queries



&nbsp;       Real-time capabilities (via subscriptions)



&nbsp;       Introspection



&nbsp;   Think of GraphQL as a standard or specification—not tied to any specific implementation or tool.



#### 🔷 Apollo GraphQL (Implementation / Ecosystem)



&nbsp;   What it is: A set of tools and libraries built around GraphQL.



&nbsp;   Created by: Apollo GraphQL, Inc.



&nbsp;   Purpose: To simplify building and consuming GraphQL APIs on the client and server.



&nbsp;   Components:



&nbsp;       Apollo Server: A Node.js GraphQL server that follows the GraphQL spec.



&nbsp;       Apollo Client: A JavaScript library for querying GraphQL from the frontend (e.g., React, Vue).



&nbsp;       Apollo Studio: Cloud-based platform for schema management, query analytics, and observability.



&nbsp;       Apollo Federation: Architecture for composing multiple GraphQL services into a single API (used in microservices).



&nbsp;   Think of Apollo GraphQL as a toolchain built on top of GraphQL.



##### ⚖️ Summary Comparison

##### Feature	GraphQL	Apollo GraphQL

Type	Specification	Implementation / Ecosystem

Maintained by	GraphQL Foundation / Community	Apollo GraphQL, Inc.

Language support	Any (JavaScript, Python, Go, etc)	Mainly JavaScript/TypeScript (Node, React, etc.)

Client support	None	Apollo Client for React, Angular, Vue, etc.

Server support	None	Apollo Server (Node.js)

Federation support	No (by spec)	Yes (Apollo Federation)

Observability/Monitoring	Not included	Available via Apollo Studio

###### ✅ Use GraphQL when...



&nbsp;   You want to follow the GraphQL spec and are choosing between different libraries.



&nbsp;   You’re using a non-JS environment (like Python, Java, Ruby).



###### ✅ Use Apollo GraphQL when...



&nbsp;   You’re working in the JavaScript ecosystem.



&nbsp;   You want batteries-included tooling: caching, dev tools, schema analytics.



&nbsp;   You need to manage large-scale, federated GraphQL APIs.





1\. Which of these are benefits of schema-first design?    



&nbsp;	It reduces the total development time.

&nbsp;	It enables team to work in parallel.

&nbsp;	Correct. Schema-first design accelerates development by making teams more independent.



2\. Which of these accurately describes a graph in GraphQL?

&nbsp;	It is a collection of nodes and object.

&nbsp;	It's a representation of our app's data.



Schema definition language (SDL)



3.Which of the following are valid field types?

&nbsp;	String

&nbsp;	Int

&nbsp;	String!

&nbsp;	\[Int]

&nbsp;	Correct. Good job! These are all valid field types.



4.Define a SpaceCat type with the following fields: name of type String (non null), age of type Int, and missions of type List of Mission



&nbsp;	type SpaceCat{

&nbsp; 	  name:String!

&nbsp; 	  age:Int

&nbsp; 	  missions:\[Mission]

&nbsp;	}



5.Add a block description for the SpaceCat type (triple "double quotes") and a normal description for the name field.

&nbsp;	

"""

I'm a block description

for SpaceCat

"""

type SpaceCat {

&nbsp; "Name of the SpaceCat"

&nbsp; name: String!

&nbsp; age: Int

&nbsp; missions: \[Mission]

}



6.What does an exclamation mark after a field's type indicate?



The field value can't be null.



Correct. An exclamation mark after a field's type marks it as non-nullable.



7.Which of these are always true about the Query type?



It defines what data client can query in our schema

It defines entry point in our schema



Correct. The Query type's fields define which data clients can query from our schema.





8.Create a full schema with: a type Query containing a field spaceCats to fetch a List of SpaceCat. A type SpaceCat with its subfields: id of type ID!, name of type String!, age of type Int and missions of type List of Mission. Finally define the Mission type with its subfields: id of type ID!, name of type String!, and description of type String!



const gql = require("graphql-tag"); 



const typeDefs = gql`

&nbsp; type Query {

&nbsp;   spaceCats: \[SpaceCat]

&nbsp; }



&nbsp; type SpaceCat {

&nbsp;   id: ID!

&nbsp;   name: String!

&nbsp;   age: Int

&nbsp;   missions: \[Mission]

&nbsp; }



&nbsp; type Mission {

&nbsp;   id: ID!

&nbsp;   name: String!

&nbsp;   description: String!

&nbsp; }

`



9\. Which of these are purposes of a GraphQL server?



&nbsp; Receiving incoming GraphQL Queries

&nbsp; Returning populated schema field as a response

&nbsp; Validating graphql queries against our schema



10\. Which of these are true about querying Apollo Server without a connected data source?



&nbsp;	You can enable default mock responses for every schema field

&nbsp;	You can enable custom mocked responses for every schema field



11\. Create a mock object with a type SpaceCat, an id of spacecat\_01, and a title of 'spacecat pioneer'



const mocks = {

&nbsp; // define your mock properties here

&nbsp; SpaceCat:()=>({

&nbsp;     id: () => "spacecat\_01",

&nbsp;     title: () => "spacecat pioneer",

&nbsp; })

}



Apollo Explorer



12\. Which of these are benefits of using the GraphOS Studio Explorer?



&nbsp;	You can step through your schema to discover available types and fields

&nbsp;	You can build and iterate on queries faster.



13 . How do we make Apollo Client available to our app's React components?



&nbsp;	We wrap our React component tree in the Apollo Provider component



Correct. Using ApolloProvider is a convenient way to make Apollo Client available everywhere it's needed  



14\. Create a ListSpaceCats query with a spaceCats query field and its name, age and missions selection set in that order. For the missions field, select name and description.



Answer:

query ListSpaceCats{

&nbsp; spaceCats {

&nbsp;   name

&nbsp;   age

&nbsp;   missions{

&nbsp;     name

&nbsp;     description

&nbsp;   }

&nbsp; }

}



15\. Which of the following are best practices when creating client queries?

&nbsp;	

Answer:

&nbsp;	Include only the field that client requires.

&nbsp;	Test out queries in the GraphOS Studio Explorer and copy them over.

&nbsp;	Assign each query string to a constant with an ALL\_CAPS name.

&nbsp;	Wrap each query in the `gql` template literal



16\. Use the useQuery hook with the SPACECATS query and destructure the loading, error and data properties from the result.



Answer:

// consider the SPACECATS query:

const SPACECATS = gql`

&nbsp; query ListSpaceCats{

&nbsp;   spaceCats{

&nbsp;     name

&nbsp;     age

&nbsp;     missions {

&nbsp;       name

&nbsp;       description

&nbsp;     }

&nbsp;   }

&nbsp; }

`

// define your useQuery hook for the SPACECATS query here



const {loading,error,data} = useQuery(SPACECATS);



17.What is the useQuery hook used for?



Answer : To execute queries in our frontend app



Correct. That's right! The useQuery hook executes the query we pass it and returns loading, error and data properties.



odyssey-lift-off-part2



1\. How does our Catstronauts client send queries to our GraphQL server?

Answer : As string using HTTP POST or GET requests.



2.Which of these are actions that our GraphQL server takes when it receives a request?

Answer:

&nbsp;	It extract the string for the GraphQL query from the request.

&nbsp;	It transforms the GraphQL string to Abstract syntax tree

&nbsp;	It validates the query against the schema

Correct. A GraphQL server does all of these.



2.Which of these are situations where our GraphQL server will throw an error?

Answer:

&nbsp;	A requested field is not defined in the schema

&nbsp;	An incoming graphql query string is removed



3\. Which of these are responsibilities of a resolver function?



Answer :

&nbsp;	Retrieving the correct data from the source such as a database of REST API

&nbsp;	Populating its corresponding field with data.



Correct. A resolver function is responsible for populating the data for a single field in your schema.



4\. When a query executes successfully, which of these is included in the object returned by the GraphQL server?



Answer : A data key containing a result object with the shame shape as the query



Correct. A successful query's result always matches the query's shape.



5 . Which of these are true about data sources?



Answer: 

&nbsp;	Resolver function retrieve data from data sources

&nbsp;	A single GraphQL API can connect to multiple data sources



Apollo RESTDataSource



1\. What makes the N + 1 problem inefficient?

&nbsp;

&nbsp;	Making N calls to the exact same endpoint to fetch the exact same data is very inefficient, especially if N is a large number.!



2.How might a resource cache be useful for our data source?



&nbsp;	It helps manage the mix of different endpoints with different cache policies.

&nbsp;	It helps resolve query fields that have already been fetched much faster.

&nbsp;	It prevents unnecessary REST API calls for data that doesn't get updated frequently.



3.Create a class called SpaceCatsAPI that extends the RESTDataSource class. Its baseURL should be set to https://fake-spacecats-rest-api.cat/. Assume that the RESTDataSource class has already been imported



Answer :

class SpaceCatsAPI extends RESTDataSource{

&nbsp; baseURL= "https://fake-spacecats-rest-api.cat/";

}



4.The SpaceCatsAPI class needs two methods. The first method is called getSpaceCats. This method makes a GET request to the spacecats endpoint and returns the results. The second method is called getMissions. It takes in a catId as an argument and makes a GET request to the spacecats/${catId}/missions endpoint and returns the results. Assume that the RESTDataSource class has already been imported.



Answer :



class SpaceCatsAPI extends RESTDataSource{

&nbsp; baseURL= "https://fake-spacecats-rest-api.cat/";



&nbsp; getSpaceCats(){

&nbsp;   return this.get("spacecats");

&nbsp; }



&nbsp; getMissions(catId){

&nbsp;   return this.get(`spacecats/${catId}/missions`);

&nbsp; }

}



5\. Which of the following are true about resolvers?



Answer :

&nbsp;	A  resolver transforms data into the shape a query requires.

&nbsp;	A  resolver is a function that's responsible for populating the data for a single field in your schema.



6\. What is the contextValue parameter useful for?



tracksForHome: (parent, args, contextValue, info) => {},



Answer :

&nbsp;	Sharing information across all resolvers for an operation

&nbsp;	Accessing the data sources from from a resolver



7\. Write an empty resolver function for the field spaceCats with all four parameters as described above. The function should not return anything. Use arrow function syntax.



Answer :

&nbsp;	const resolvers = {

&nbsp;		Query: {

&nbsp;			spaceCats : (parent, args , contextValue ,info)=>{},

&nbsp;		}

&nbsp;	}



8\. Edit the parameters for the resolver function of the spaceCats field to follow the conventions explained above. You'll need to destructure dataSources from the contextValue parameter. The function should not return anything for now.



Answer :

&nbsp;	const resolvers = {

&nbsp; Query: {

&nbsp;   // edit the parameters below

&nbsp;   spaceCats: (\_, \_\_, {dataSources}) => {}

&nbsp; }

}



Note :

We don't need the first two parameters, so as a convention, we'll name them with underscores: one underscore for the first (parent) and two underscores for the second (args).



For the contextValue, we'll destructure it to access its child object dataSources. And we can omit the fourth parameter, info, as we won't use it.



9\. Create a resolver function for the Query.spaceCats field. Follow the conventions explained above for the four optional parameters. Use the dataSources object to access the spaceCatsAPI.getSpaceCats() method and return the results. Use arrow function syntax.



Answer :



const resolvers = {

&nbsp; Query: {

&nbsp;   spaceCats: (\_, \_\_, {dataSources}) => {

&nbsp;     // write your code here

&nbsp;     return dataSources.spaceCatsAPI.getSpaceCats();

&nbsp;   }

&nbsp; }

};



10 . Which information will be present in the parent argument of our Track.author resolver?



Answer : Track Title

&nbsp;	 Track Description

&nbsp;	 Author Id 

&nbsp;	 Correct. Our Track.author resolver will use a track's authorId to fetch its author details.





11.Create a resolver function for the SpaceCat.missions field. Follow the conventions explained above for the four optional parameters. Use the dataSources object to access the spaceCatsAPI.getMissions() method. It takes a catId argument from the parent and returns the results. Use arrow function syntax.





Answer :



const resolvers = {

&nbsp; Query: {

&nbsp;   spaceCats: (\_, \_\_, {dataSources}) => {

&nbsp;     return dataSources.spaceCatsAPI.getSpaceCats();

&nbsp;   }

&nbsp; },



&nbsp; // write the SpaceCat.missions resolver below

&nbsp; 

&nbsp; SpaceCat:{

&nbsp;   missions : ({catId},\_,{dataSources})=>{

&nbsp;     return dataSources.spaceCatsAPI.getMissions(catId) 

&nbsp;   }

&nbsp; }





};





12\. We have a class called SpaceCatsAPI, which implements a RESTDataSource. We need to access an instance of this class, which we'll call spaceCatsAPI, from our resolvers. Configure the server below so that our resolvers can access this class through the dataSources.spaceCatsAPI property they receive on their contextValue argument. (Watch out, this is case sensitive!)



Answer :



const { ApolloServer } = require("@apollo/server");

const { startStandaloneServer } = require("@apollo/server/standalone");

const typeDefs = require("./schema");

const resolvers = require("./resolvers");

const TrackAPI = require("./datasources/track-api");

&nbsp;

async function startApolloServer() {

&nbsp; const server = new ApolloServer({ typeDefs, resolvers });

&nbsp; const { url } = await startStandaloneServer(server, {

&nbsp;   context: async () => {

&nbsp;     const { cache } = server;

&nbsp;      

&nbsp;     return {

&nbsp;       dataSources: {

&nbsp;         spaceCatsAPI: new SpaceCatsAPI({ cache }),

&nbsp;       },

&nbsp;     };

&nbsp;   },

&nbsp; });



&nbsp; console.log(`

&nbsp;   🚀  Server is running

&nbsp;   📭  Query at ${url}

&nbsp; `);

}

&nbsp;

startApolloServer();





13 . Why do we need to return the dataSources object from the server's context function?



Answer : To gain access to our data sources from each resolver's context value parameter.



Correct. Returning the dataSources object will make our RestDataSource API available to all resolvers from their contextValue parameter.



14\. Which of these have we demonstrated in the Explorer by comparing a query using the RESTDataSource with a query using a simple fetch approach?



Answers:

&nbsp;	Different resolvers can use different methods to access the same REST API.

&nbsp;	We get the same response data with both approaches.

&nbsp;	Thanks to caching in RESTDataSource results are returned faster after the first query.



15\. Within a server response, what keys can you expect to find in the JSON object?



Answers :

&nbsp;	The data key

&nbsp;	The errors key



Example Response On Success

{

&nbsp; "data": {

&nbsp;   "tracksForHome": \[

&nbsp;     {

&nbsp;       "id": "c\_0",

&nbsp;       "title": "Cat-stronomy, an introduction",

&nbsp;       "author": {

&nbsp;         "id": "cat-1",

&nbsp;         "name": "Henri, le Chat Noir",

&nbsp;         "photo": "https://images.unsplash.com/photo-1442291928580-fb5d0856a8f1?ixlib=rb-1.2.1\&q=80\&fm=jpg\&crop=entropy\&cs=tinysrgb\&w=1080\&fit=max\&ixid=eyJhcHBfaWQiOjExNzA0OH0"

&nbsp;       },

&nbsp;       "thumbnail": "https://res.cloudinary.com/apollographql/image/upload/v1730818804/odyssey/lift-off-api/nebula\_cat\_djkt9r\_nzifdj.jpg",

&nbsp;       "length": 2377,

&nbsp;       "modulesCount": 10

&nbsp;     }

&nbsp;   ]

&nbsp; }

}



Example Response On Error



{

&nbsp; "errors": \[

&nbsp;   {

&nbsp;     "message": "Cannot query field \\"nickname\\" on type \\"Track\\".",

&nbsp;     "locations": \[

&nbsp;       {

&nbsp;         "line": 13,

&nbsp;         "column": 5

&nbsp;       }

&nbsp;     ],

&nbsp;     "extensions": {

&nbsp;       "code": "GRAPHQL\_VALIDATION\_FAILED",

&nbsp;       "stacktrace": \[

&nbsp;         "GraphQLError: Cannot query field \\"nickname\\" on type \\"Track\\".",

&nbsp;         "    at Object.Field (C:\\\\Data\\\\MdTamimArshad\\\\GraphQL\\\\ApolloGraphQL\\\\odyssey-lift-off-part2\\\\server\\\\node\_modules\\\\graphql\\\\validation\\\\rules\\\\FieldsOnCorrectTypeRule.js:51:13)",

&nbsp;         "    at Object.enter (C:\\\\Data\\\\MdTamimArshad\\\\GraphQL\\\\ApolloGraphQL\\\\odyssey-lift-off-part2\\\\server\\\\node\_modules\\\\graphql\\\\language\\\\visitor.js:301:32)",

&nbsp;         "    at Object.enter (C:\\\\Data\\\\MdTamimArshad\\\\GraphQL\\\\ApolloGraphQL\\\\odyssey-lift-off-part2\\\\server\\\\node\_modules\\\\graphql\\\\utilities\\\\TypeInfo.js:391:27)",

&nbsp;         "    at visit (C:\\\\Data\\\\MdTamimArshad\\\\GraphQL\\\\ApolloGraphQL\\\\odyssey-lift-off-part2\\\\server\\\\node\_modules\\\\graphql\\\\language\\\\visitor.js:197:21)",

&nbsp;         "    at validate (C:\\\\Data\\\\MdTamimArshad\\\\GraphQL\\\\ApolloGraphQL\\\\odyssey-lift-off-part2\\\\server\\\\node\_modules\\\\graphql\\\\validation\\\\validate.js:91:24)",

&nbsp;         "    at processGraphQLRequest (C:\\\\Data\\\\MdTamimArshad\\\\GraphQL\\\\ApolloGraphQL\\\\odyssey-lift-off-part2\\\\server\\\\node\_modules\\\\@apollo\\\\server\\\\dist\\\\cjs\\\\requestPipeline.js:101:61)",

&nbsp;         "    at process.processTicksAndRejections (node:internal/process/task\_queues:105:5)",

&nbsp;         "    at async internalExecuteOperation (C:\\\\Data\\\\MdTamimArshad\\\\GraphQL\\\\ApolloGraphQL\\\\odyssey-lift-off-part2\\\\server\\\\node\_modules\\\\@apollo\\\\server\\\\dist\\\\cjs\\\\ApolloServer.js:635:16)",

&nbsp;         "    at async runHttpQuery (C:\\\\Data\\\\MdTamimArshad\\\\GraphQL\\\\ApolloGraphQL\\\\odyssey-lift-off-part2\\\\server\\\\node\_modules\\\\@apollo\\\\server\\\\dist\\\\cjs\\\\runHttpQuery.js:135:29)",

&nbsp;         "    at async runPotentiallyBatchedHttpQuery (C:\\\\Data\\\\MdTamimArshad\\\\GraphQL\\\\ApolloGraphQL\\\\odyssey-lift-off-part2\\\\server\\\\node\_modules\\\\@apollo\\\\server\\\\dist\\\\cjs\\\\httpBatching.js:37:16)"

&nbsp;       ]

&nbsp;     }

&nbsp;   }

&nbsp; ]

}



13\. When you query a field that is not in the schema, what error code will the server send?



Answer : Correct. 



GRAPHQL\_VALIDATION\_FAILED error code means the GraphQL operation is not valid against the server's schema. 

Either a field isn't defined, or you might have a typo somewhere in your query!





Lift-off III: Arguments



1\. Which of these are reasons to create a separate Module type?



&nbsp;	A module can belong to more than one track

&nbsp;	A track can contain any number of different modules.



Example

"A Module is a single unit of teaching. Multiple Modules compose a Track"

type Module {

&nbsp; id: ID!

&nbsp; "The Module's title"

&nbsp; title: String!

&nbsp; "The Module's length in minutes"

&nbsp; length: Int

}



"The track's complete array of Modules"

modules: \[Module!]!



2\. Given this schema field: missions: \[Mission!], which of the following statement is true:



Answer : The list item can't be null



Correct. The ! symbol means that the field is non-nullable. The list itself is nullable, but the items inside it (Mission!) are non-nullable.



3\. Where can we add entry points to our schema?



Answer :  In the Query Type 



Correct. The Query type contains the entry points to our schema. In future courses, we'll take a look at two other possible entry points: Mutation and Subscription.



4\. Which of these are reasons to use arguments in a query?



&nbsp;	To provide a user-submitted search term

&nbsp;	To retrieve a specific object

&nbsp;	To transform a field's returned value

&nbsp;	To filter a set of objects

Correct. All of these are valid reasons to use arguments!



Example : track(id: ID!): Track! 



5\. Update the schema below to add two new entry points: 1) Query for a specific spacecat. This field takes an argument id of type non-nullable ID and returns a nullable SpaceCat. 2) Query for a list of missions. This field takes two arguments: to of nullable type String (to specify the mission destination), and scheduled of nullable type Boolean (to filter for scheduled or unscheduled missions). This field returns a nullable array of nullable Missions.



Answer :



&nbsp;  # add the 2 fields here

&nbsp;   spacecat(id:ID!): SpaceCat

&nbsp;   missions(to:String,scheduled:Boolean):\[Mission]



6\. Write a resolver function for the spaceCat field to query a spaceCat by ID. Follow the conventions used above for the four resolver parameters, and use arrow function syntax. Use the dataSources object to access the spaceCatsAPI.getSpaceCat() method, which takes the id from args and returns the results.



// write your resolver here

&nbsp;   spaceCat: (\_,{id},{dataSources})=>{

&nbsp;     return dataSources.spaceCatsAPI.getSpaceCat(id);

&nbsp;   }



Resolver chains



7\. Which of these are reasons why we extracted author-fetching logic to a different resolver in Lift-off II? 

&nbsp;  (Hint: Keep reading if you need a refresher and come back to this question when you're ready!)



Answer :



&nbsp;	To prevent unnecessary REST API calls when a query doesn't ask for author data

&nbsp;	To keep each resolver lightweight and responsible for specific pieces of data

&nbsp;	To keep resolvers more resilient to future changes



8\. Read below



A resolver function populates the data for a 



field

&nbsp;in your schema. The function has four parameters. The first, 



parent

, contains the returned data of the previous function in the 



resolver chain

. The second, args, is an object that contains all the 



arguments

&nbsp;provided to the field. We use the third parameter, 



contextValue

, to access 



data sources

&nbsp;such as a database or REST API. Finally, the fourth parameter, 



info

, contains informational properties about the operation state.





9\. Variables are denoted by the $ symbol. They are used to provide dynamic values for 

&nbsp; arguments to avoid including hardcoded values in a query. Each one's type must match the type specified in the schema.



10\. Build a query called GetMission. This query uses a variable called isScheduled of type nullable Boolean. It retrieves a mission using the scheduled argument set to the isScheduled variable. It retrieves the mission's id and codename.



Answer :

const GET\_MISSION = gql`

&nbsp; query GetMission($isScheduled: Boolean) {

&nbsp;   mission(scheduled: $isScheduled) {

&nbsp;     id

&nbsp;     codename

&nbsp;   }

&nbsp; }

`;



11\. On the client side, where will the query above get the value for the $trackId variable?



Answer : from the route path or browser URL



Correct. The trackId we need is part of the router path when we set it up as /track/:trackId.



12\. We wrap our query string in the gql template literal and then send it to our server with the 

&nbsp;   useQuery hook.



13\. Use the useQuery hook to send the GET\_SPACECAT query to the server. It takes a spaceCatId as a variable. Destructure the loading, error and data properties from the return object of the hook.





// define your useQuery hook for the GET\_SPACECAT query below:



const {loading , error, data} = useQuery(GET\_SPACECAT,{

&nbsp; variables : {spaceCatId}

});



14 . The useQuery hook returns an object with three useful properties that we use in our app: 

&nbsp;    loading indicates whether the query has completed and results have been returned. 

&nbsp;   error is an object that contains any errors that the operation has thrown.

&nbsp;  data contains the results of the query after it has completed. To set 

&nbsp; variables in our query, we declare them in the 

&nbsp; second parameter of the useQuery hook, inside an options object.







Lift-off IV: Mutations



1\. Which of these are good names for mutations based on the recommended conventions above?



&nbsp;	createMission

&nbsp;	deleteMission

&nbsp;	addMission



Correct. That's right! Mutation names should start with a verb, followed by whatever data you're modifying!



2 . Which of these are reasons to create a separate object for a mutation's return type?

&nbsp;	

&nbsp;	When the mutation modifies multiple objects, it can return all of those objects

&nbsp;	The return type can contain partial errors and partial successful data.



3.Read 

Queries and mutations are both types of GraphQL operations

. Queries are read operations that always retrieve

&nbsp;data. Mutations are write operations that always modify

data. Similar to Query fields, fields of the Mutation type are also entry points into a GraphQL API.



4.In the mutation response type (IncrementTrackViewsResponse), why is the modified object's return type nullable (track: Track)



Answer : The mutation might encounters errors that prevent a track from being modified





5\. Add a mutation to this existing schema. The mutation should assign a spaceship to a specific mission. It should be called assignSpaceship. It takes in two required arguments: a spaceshipId of type ID!, and a missionId of type ID!. It should return a non-nullable type AssignSpaceshipResponse. This return type should return the three informational properties discussed above, as well as nullable spaceship and mission fields with corresponding return types.



&nbsp;   type Mutation {

&nbsp;     assignSpaceship(spaceshipId:ID!,missionId:ID!) : AssignSpaceshipResponse

&nbsp;   }



&nbsp; # write your mutation return type here



&nbsp;   type AssignSpaceshipResponse{

&nbsp;     code:Int!

&nbsp;     success:Boolean!

&nbsp;     message:String!

&nbsp;     spaceship:Spaceship

&nbsp;     mission:Mission

&nbsp;   }



6.Why do we use a separate RESTDataSource class to handle data retrieval?



&nbsp;	It automatically handles resource caching and request deduplication for our REST API Calls

&nbsp;	To keep data fetching implementation in a dedicated class and keep resolvers simple and clean



Correct. That's right! Using the RESTDataSource class is so handy!



7.Which of these are true about the structure of the resolvers object?



Answer :

&nbsp;	Query and Mutation types in the schema should have corresponding keys in the resolvers object

&nbsp;	Resolver function name must match the field name in the schema



8.Why can't this resolver immediately return the results of the TrackAPI call in this case?



Answer :

&nbsp;	It builds part of the response from the REST operation status

&nbsp;	The schema is also expecting code , success and message .



9.Add a resolver for the new assignSpaceship mutation provided in the schema, using arrow function syntax. Use the dataSources.spaceAPI class and its method assignSpaceshipToMission, which takes the spaceshipId and the missionId as arguments (in that order). This method returns an object with the newly updated spaceship and mission. Follow the schema requirements to return an object for the successful result. code should be 200, success should be true and message should say Successfully assigned spaceship ${spaceshipId} to mission ${missionId} 



const resolvers = {

&nbsp;	Mutation : {

&nbsp;   	assignSpaceship : async (\_,{spaceshipId,missionId},{dataSources})=>{

&nbsp;     	const {spaceship,mission} = await dataSources.spaceAPI.assignSpaceshipToMission(spaceshipId,missionId);

&nbsp;     		return {

&nbsp;       	code: 200,

&nbsp;       	success: true,

&nbsp;       	message: `Successfully assigned spaceship ${spaceshipId} to mission ${missionId}`,

&nbsp;       	spaceship,

&nbsp;       	mission

&nbsp;     	}

&nbsp;   }

&nbsp; }

}



10 . Which of the following statements are true about the err.extensions fields returned by the server?



Answer : 

&nbsp;	It helps us make our response object fields more dynamic

&nbsp;	The error message is accessible from err.extensions.response.body

&nbsp;	The HTTP status code is accessible from err.extensions.response.status



11\. The resolver handles both a successful response and possible errors. Which of these are true about the return objects for those two cases?



Answer : 

&nbsp;	Only the successful response contains track

&nbsp;	Both objects fulfill the requirements of the schema



12\. We start with the same assignSpaceship Mutation resolver example as in the previous code challenge. This time, we assume the dataSources.spaceAPI.assignSpaceshipToMission() call will return an error. Modify the following piece of code to properly handle the error returned by the API and return a properly formatted object that conforms to the schema definition of the AssignSpaceshipResponse type. Remember to use a try/catch block and to return dynamic error data when possible.



Answer :

// Here's the solution (your code is commented out after):

const resolvers = {

&nbsp; Mutation: {

&nbsp;   assignSpaceship: async (\_, { spaceshipId, missionId }, { dataSources }) => {

&nbsp;     try {

&nbsp;       const { spaceship, mission } = await dataSources.spaceAPI.assignSpaceshipToMission(spaceshipId, missionId);

&nbsp;       return {

&nbsp;         code: 200,

&nbsp;         success: true,

&nbsp;         message: `Successfully assigned spaceship ${spaceshipId} to ${missionId}`,

&nbsp;         spaceship: spaceship,

&nbsp;         mission: mission

&nbsp;       };

&nbsp;     } catch (error) {

&nbsp;       return {

&nbsp;         code: error.extensions.response.status,

&nbsp;         success: false,

&nbsp;         message: error.extensions.response.body,

&nbsp;         spaceship: null,

&nbsp;         mission: null

&nbsp;       };

&nbsp;     }

&nbsp;   }

&nbsp; }

}



13\. Read it 



We use hooks to send requests to our GraphQL API from a React client. To send a mutation, we use the useMutation hook. This returns an array, where the first element is the mutate function used to trigger the mutation. The second element is an object with more information about the mutation, such as loading, error and data. This hook takes in a GraphQL operation as the first parameter. It also takes in an options object as the second parameter, where properties like variables are set



14\. Which of these are differences between the useQuery and useMutation hooks?



Answer :



&nbsp;useQuery hook runs automatically on component render, whereas the useMutation  hook returns a mutate function needed to trigger the mutation

&nbsp;useQuery hook is used to send queries, whereas the useMutation hook is used to send mutations

&nbsp;useQuery hook is used to send queries, whereas the  useMutation hook is used to send mutations



15.Use the useMutation hook to send the ASSIGN\_SPACESHIP\_MUTATION mutation to the server. It takes two variables: spaceshipId and missionId. Destructure the mutate function (call it assignSpaceship), as well as the loading, error and data properties from the return array of the hook.





// build the useMutation hook here

const \[assignSpaceship,{loading,error,data}] = useMutation(ASSIGN\_SPACESHIP\_MUTATION,{

&nbsp; variables:{

&nbsp;   spaceshipId:spaceshipId,

&nbsp;   missionId:missionId

&nbsp; }

});

&nbsp; 

16\. How do we see the number of views for a track update while we're on the page?



Answer :The value is first loaded from cache, then updates when the mutation response comes back successfully







Topics:

N + 1 problem

RestDataSource

Resolver Chain









🧩 Apollo Federation



&nbsp;   What it is: A way to break a large GraphQL schema into smaller, independently managed services called subgraphs.



&nbsp;   Why use it:



&nbsp;       Modular and scalable architecture



&nbsp;       Teams can work independently



&nbsp;       Easier maintenance and faster development



&nbsp;       One combined schema served to the client via a central gateway



🚪 Apollo Gateway



&nbsp;   What it is: A GraphQL server that acts as the central entry point. It fetches and composes schemas from multiple subgraph services.



&nbsp;   Why use it:



&nbsp;       Provides a single GraphQL endpoint for clients



&nbsp;       Automatically combines schemas from all subgraphs



&nbsp;       Hides the complexity of multiple backend services from the frontend



⚙️ How to Use It (Example Overview)



&nbsp;   Create subgraph services:



&nbsp;       users-service: handles user data



&nbsp;       products-service: handles product data



&nbsp;   Each subgraph defines its own schema and uses Apollo’s @key directive to participate in the unified schema.



&nbsp;   Create Apollo Gateway:



&nbsp;       Connect it to all subgraphs using their URLs



&nbsp;       It fetches and merges the schemas automatically



&nbsp;   Client sends a single GraphQL query to the gateway, which routes requests to the appropriate subgraphs and returns a combined response.



🧠 Final Takeaway:



&nbsp;   Apollo Federation + Gateway = A modern, scalable way to build large GraphQL APIs using a modular microservices architecture, with a seamless experience for frontend clients.



Let me know if you want code samples, a visual diagram, or help setting it up in a real project!

You said:

Example

ChatGPT said:



Sure! Here's a complete example showing how Apollo Federation with Apollo Gateway works — including code snippets for two subgraph services (users and products) and one gateway.

⚙️ Tech Stack



&nbsp;   Node.js



&nbsp;   Apollo Server



&nbsp;   Apollo Federation libraries



📁 Project Structure



federation-example/

│

├── gateway/

│   └── index.js

│

├── users/

│   └── index.js

│

└── products/

&nbsp;   └── index.js



🧑‍💻 Step-by-Step Code

1️⃣ Users Subgraph



📁 users/index.js



const { ApolloServer } = require('@apollo/server');

const { startStandaloneServer } = require('@apollo/server/standalone');

const { buildSubgraphSchema } = require('@apollo/subgraph');

const gql = require('graphql-tag');



const typeDefs = gql`

&nbsp; type User @key(fields: "id") {

&nbsp;   id: ID!

&nbsp;   name: String

&nbsp; }



&nbsp; type Query {

&nbsp;   me: User

&nbsp; }

`;



const resolvers = {

&nbsp; Query: {

&nbsp;   me: () => ({ id: '1', name: 'Alice' }),

&nbsp; },

};



const server = new ApolloServer({

&nbsp; schema: buildSubgraphSchema(\[{ typeDefs, resolvers }]),

});



startStandaloneServer(server, {

&nbsp; listen: { port: 4001 },

}).then(() => {

&nbsp; console.log(`✅ Users service ready at http://localhost:4001`);

});



2️⃣ Products Subgraph



📁 products/index.js



const { ApolloServer } = require('@apollo/server');

const { startStandaloneServer } = require('@apollo/server/standalone');

const { buildSubgraphSchema } = require('@apollo/subgraph');

const gql = require('graphql-tag');



const typeDefs = gql`

&nbsp; type Product @key(fields: "id") {

&nbsp;   id: ID!

&nbsp;   name: String

&nbsp; }



&nbsp; type Query {

&nbsp;   topProducts: \[Product]

&nbsp; }

`;



const resolvers = {

&nbsp; Query: {

&nbsp;   topProducts: () => \[

&nbsp;     { id: '101', name: 'iPhone' },

&nbsp;     { id: '102', name: 'MacBook' },

&nbsp;   ],

&nbsp; },

};



const server = new ApolloServer({

&nbsp; schema: buildSubgraphSchema(\[{ typeDefs, resolvers }]),

});



startStandaloneServer(server, {

&nbsp; listen: { port: 4002 },

}).then(() => {

&nbsp; console.log(`✅ Products service ready at http://localhost:4002`);

});



3️⃣ Apollo Gateway



📁 gateway/index.js



const { ApolloServer } = require('@apollo/server');

const { startStandaloneServer } = require('@apollo/server/standalone');

const { ApolloGateway } = require('@apollo/gateway');



const gateway = new ApolloGateway({

&nbsp; serviceList: \[

&nbsp;   { name: 'users', url: 'http://localhost:4001' },

&nbsp;   { name: 'products', url: 'http://localhost:4002' },

&nbsp; ],

});



const server = new ApolloServer({

&nbsp; gateway,

&nbsp; subscriptions: false,

});



startStandaloneServer(server, {

&nbsp; listen: { port: 4000 },

}).then(() => {

&nbsp; console.log(`🚀 Gateway running at http://localhost:4000`);

});



📬 Client Query Example



Hit this GraphQL query at: http://localhost:4000



query {

&nbsp; me {

&nbsp;   id

&nbsp;   name

&nbsp; }

&nbsp; topProducts {

&nbsp;   id

&nbsp;   name

&nbsp; }

}



🔁 Response:



{

&nbsp; "data": {

&nbsp;   "me": {

&nbsp;     "id": "1",

&nbsp;     "name": "Alice"

&nbsp;   },

&nbsp;   "topProducts": \[

&nbsp;     { "id": "101", "name": "iPhone" },

&nbsp;     { "id": "102", "name": "MacBook" }

&nbsp;   ]

&nbsp; }

}



✅ Summary



&nbsp;   You created two subgraph services (users, products)



&nbsp;   Apollo Gateway connects them and exposes one endpoint



&nbsp;   The client can query both services through a single unified API





🔧 Expressions in Connectors: Mapping and Transforms



When you're using Apollo Connectors (now known as Apollo Data Sources, like RESTDataSource), you often need to:



&nbsp;   Call a REST API



&nbsp;   Extract and restructure the response



&nbsp;   Map it into your GraphQL schema



📌 Scenario



Imagine your REST API returns complex or deeply nested data:

Example REST response:



{

&nbsp; "data": {

&nbsp;   "movie": {

&nbsp;     "id": "123",

&nbsp;     "attributes": {

&nbsp;       "title": "Inception",

&nbsp;       "year": 2010,

&nbsp;       "details": {

&nbsp;         "director": "Christopher Nolan",

&nbsp;         "rating": "PG-13"

&nbsp;       }

&nbsp;     }

&nbsp;   }

&nbsp; }

}



But your GraphQL schema expects:



type Movie {

&nbsp; id: ID!

&nbsp; title: String

&nbsp; year: Int

&nbsp; director: String

&nbsp; rating: String

}



🧩 Step-by-Step: Mapping with Apollo Data Source

1\. Create a RESTDataSource



const { RESTDataSource } = require('apollo-datasource-rest');



class MoviesAPI extends RESTDataSource {

&nbsp; constructor() {

&nbsp;   super();

&nbsp;   this.baseURL = 'https://api.example.com/';

&nbsp; }



&nbsp; async getMovieById(id) {

&nbsp;   const response = await this.get(`movies/${id}`);

&nbsp;   

&nbsp;   // transform the response

&nbsp;   return this.movieReducer(response.data.movie);

&nbsp; }



&nbsp; // transformation logic here

&nbsp; movieReducer(movie) {

&nbsp;   return {

&nbsp;     id: movie.id,

&nbsp;     title: movie.attributes.title,

&nbsp;     year: movie.attributes.year,

&nbsp;     director: movie.attributes.details.director,

&nbsp;     rating: movie.attributes.details.rating,

&nbsp;   };

&nbsp; }

}



2\. Hook into Apollo Server



const server = new ApolloServer({

&nbsp; typeDefs,

&nbsp; resolvers,

&nbsp; dataSources: () => ({

&nbsp;   moviesAPI: new MoviesAPI()

&nbsp; })

});



3\. Use in Resolvers



const resolvers = {

&nbsp; Query: {

&nbsp;   movie: (\_, { id }, { dataSources }) => {

&nbsp;     return dataSources.moviesAPI.getMovieById(id);

&nbsp;   }

&nbsp; }

};



🔄 Why Use This Pattern?



✅ Decouple your schema from messy or inconsistent REST responses

✅ Normalize or enrich data on the server

✅ Reuse transform logic across multiple queries

✅ Maintain separation of concerns — resolvers stay clean

🛠️ Advanced Techniques

🔹 Use lodash or deep access tools



For very nested data, use libraries like lodash.get():



const get = require('lodash.get');



movieReducer(movie) {

&nbsp; return {

&nbsp;   id: get(movie, 'id'),

&nbsp;   title: get(movie, 'attributes.title'),

&nbsp;   director: get(movie, 'attributes.details.director')

&nbsp; };

}



🔹 Add computed fields



movieReducer(movie) {

&nbsp; return {

&nbsp;   id: movie.id,

&nbsp;   title: movie.attributes.title,

&nbsp;   isClassic: movie.attributes.year < 2000

&nbsp; };

}



✅ Summary



Mapping and transforming REST responses with Apollo Connectors lets you:



&nbsp;   Extract deep/nested data



&nbsp;   Flatten or reshape it for your GraphQL types



&nbsp;   Add logic, filters, or computed fields



&nbsp;   Keep your GraphQL API clean, even with messy REST sources



Would you like a working GitHub example or boilerplate to try this out locally?

You said:

Summarize with example

ChatGPT said:



Sure! Here's a concise summary with an example for:

🚀 Expressions in Apollo Connectors: Mapping and Transforms

🧠 Summary



Apollo Connectors (data sources like RESTDataSource) allow you to connect to REST APIs and transform complex responses into the shape your GraphQL schema expects.



You do this by writing mapping functions (like movieReducer) that restructure data, extract fields, and even compute new values.

✅ Use Case



REST API response (raw \& nested):



{

&nbsp; "data": {

&nbsp;   "movie": {

&nbsp;     "id": "123",

&nbsp;     "attributes": {

&nbsp;       "title": "Inception",

&nbsp;       "year": 2010,

&nbsp;       "details": {

&nbsp;         "director": "Christopher Nolan",

&nbsp;         "rating": "PG-13"

&nbsp;       }

&nbsp;     }

&nbsp;  



You said:

Summarize whole chat so as to revise each topic and what why and how

ChatGPT said:



Absolutely! Here’s a summary of our chat to help you revise the key topics we covered, including what, why, and how for each:

Summary: GraphQL meets REST with Apollo Connectors — Mapping and Transforms

1\. GraphQL meets REST with Apollo Connectors

What?



&nbsp;   Using GraphQL as a flexible API layer that interfaces with existing REST APIs.



&nbsp;   Apollo Connectors (now called Apollo Data Sources) help connect your GraphQL server to REST endpoints.



Why?



&nbsp;   To modernize APIs without rewriting all backend REST services.



&nbsp;   To leverage GraphQL’s flexible querying and strong typing.



&nbsp;   To provide a unified API gateway for clients.



How?



&nbsp;   Use Apollo’s RESTDataSource class to build connectors that fetch data from REST.



&nbsp;   Call those connectors from GraphQL resolvers.



&nbsp;   Example snippet:



&nbsp;   class MoviesAPI extends RESTDataSource {

&nbsp;     baseURL = 'https://api.example.com/';

&nbsp;     async getMovieById(id) {

&nbsp;       return this.get(`movies/${id}`);

&nbsp;     }

&nbsp;   }



2\. Expressions in Connectors: Mapping and Transforms

What?



&nbsp;   REST APIs often return complex, deeply nested data.



&nbsp;   You need to map and transform that data to match your GraphQL schema.



Why?



&nbsp;   To decouple your schema from messy REST responses.



&nbsp;   To normalize, flatten, or enrich data before returning to clients.



&nbsp;   To keep resolvers clean and reuse transformation logic.



How?



&nbsp;   Define mapping functions inside your connector classes (e.g., movieReducer).



&nbsp;   Extract nested fields and reshape objects.



&nbsp;   Optionally add computed fields or logic.



&nbsp;   Example:



&nbsp;   movieReducer(movie) {

&nbsp;     return {

&nbsp;       id: movie.id,

&nbsp;       title: movie.attributes.title,

&nbsp;       director: movie.attributes.details.director,

&nbsp;       rating: movie.attributes.details.rating,

&nbsp;     };

&nbsp;   }



&nbsp;   Call this in your data source method before returning the result.



3\. Example: Putting it all together

REST API response (raw):



{

&nbsp; "data": {

&nbsp;   "movie": {

&nbsp;     "id": "123",

&nbsp;     "attributes": {

&nbsp;       "title": "Inception",

&nbsp;       "year": 2010,

&nbsp;       "details": {

&nbsp;         "director": "Christopher Nolan",

&nbsp;         "rating": "PG-13"

&nbsp;       }

&nbsp;     }

&nbsp;   }

&nbsp; }

}



GraphQL schema expects:



type Movie {

&nbsp; id: ID!

&nbsp; title: String

&nbsp; year: Int

&nbsp; director: String

&nbsp; rating: String

}



Connector with mapping:



class MoviesAPI extends RESTDataSource {

&nbsp; baseURL = 'https://api.example.com/';



&nbsp; async getMovieById(id) {

&nbsp;   const response = await this.get(`movies/${id}`);

&nbsp;   return this.movieReducer(response.data.movie);

&nbsp; }



&nbsp; movieReducer(movie) {

&nbsp;   return {

&nbsp;     id: movie.id,

&nbsp;     title: movie.attributes.title,

&nbsp;     year: movie.attributes.year,

&nbsp;     director: movie.attributes.details.director,

&nbsp;     rating: movie.attributes.details.rating,

&nbsp;   };

&nbsp; }

}



Usage in resolver:



const resolvers = {

&nbsp; Query: {

&nbsp;   movie: (\_, { id }, { dataSources }) => dataSources.moviesAPI.getMovieById(id),

&nbsp; },

};



Why is this important?



&nbsp;   Enables gradual migration from REST to GraphQL.



&nbsp;   Provides a clean, flexible, and efficient API to frontend clients.



&nbsp;   Allows adding custom logic, caching, and error handling on top of REST.



&nbsp;   Keeps backend REST APIs intact while modernizing the API interface.





