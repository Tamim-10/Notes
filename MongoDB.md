1. Which numeric type is a valid MongoDB BSON type?

(Choose 1)

    a. Float
    b. Number
    c. BIGINT
    d. 32-bit integer
ChatGPT said:

The correct answer is:

d. 32-bit integer
Explanation:

MongoDB BSON (Binary JSON) supports several numeric types, including:

    Double (float) – represented in BSON as a 64-bit IEEE 754 floating point

    32-bit Integer – standard 4-byte signed integer

    64-bit Integer (long) – 8-byte signed integer

    Decimal128 – 128-bit IEEE 754-2008 decimal floating point

Out of the options:

    a. Float – Not a BSON type by name. In BSON it's called "Double."

    b. Number – Not a specific BSON type.

    c. BIGINT – Not a BSON type. Could refer to 64-bit integer in some SQL dialects, but not in MongoDB BSON directly.

    ✅ d. 32-bit integer – Valid BSON type.

3. Given the following documents in a collection:

{_id: 1, txt: "just some text"},
{_id: 2, txt: "just some text"}

Which two documents can successfully be added in the same collection?

(Choose 2)

    a. {_id: 0, txt: "just some text"}
    b. {_id: 1, txt: "just some text"}
    c. {_id: [4], txt: "just some text"}
    d. {_id: 3, txt: "just some text"}
ChatGPT said:

To answer this question, you need to understand how MongoDB handles the _id field.
Key MongoDB Rules:

    The _id field must be unique within a collection.

    MongoDB supports various data types for _id (e.g., integers, strings, arrays), as long as the value is unique.

Given:

The collection already contains:

{_id: 1, txt: "just some text"}
{_id: 2, txt: "just some text"}

Evaluating Options:

a. {_id: 0, txt: "just some text"}

    _id: 0 is unique.

    ✅ Valid

b. {_id: 1, txt: "just some text"}

    _id: 1 already exists in the collection.

    ❌ Invalid (duplicate _id)

c. {_id: [4], txt: "just some text"}

    Arrays are allowed as _id values.

    [4] is unique and doesn't conflict with existing _ids.

    ✅ Valid

d. {_id: 3, txt: "just some text"}

    _id: 3 is unique.

    ✅ Valid

Correct Answers:

✅ a. {_id: 0, txt: "just some text"}
✅ c. {_id: [4], txt: "just some text"}
✅ d. {_id: 3, txt: "just some text"}

But the question asks for two. So, choose any two of the three valid ones.
Final Answer (choose any 2 of the 3 valid options):

a and c
(or a and d, or c and d — all are correct combinations)

4. Given the following document:

The name is a.log, the owner of the file is applicationA, the size of the file is 1KB, and the file was deleted.

What command will properly add this document to the files collection using mongosh?

(Choose 1)

    a. db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1KB, deleted: true })
    b. db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1KB, deleted: True })
    c. db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1024, deleted: true })
    d. db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1024, deleted: True })
ChatGPT said:

The correct answer is:

c. db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1024, deleted: true })
Explanation:

    file: "a.log" → Correct string format.

    owner: "applicationA" → Correct string format.

    size: 1024 → Correct numeric format. 1KB must be expressed as an integer (1 KB = 1024 bytes).

    deleted: true → Boolean value in JavaScript is lowercase true, not True.

Why the others are incorrect:

    a & b: 1KB is not valid syntax in JavaScript/JSON — should be 1024.

    b & d: True is invalid in JavaScript — must be true.

✅ Correct and valid MongoDB insert statement uses proper types and syntax — which is exactly what option (c) provides.

5. Given the following sample documents in products collection:

{ "name" : "XPhone", "price" : 799, "color" : [ "white", "black" ], "storage" : [ 64, 128, 256 ] },
{ "name" : "XPad", "price" : 899, "color" : [ "white", "black", "purple" ], "storage" : [ 128, 256, 512 ] },
{ "name" : "GTablet", "price" : 899, "color" : [ "blue" ], "storage" : [ 16, 64, 128 ] },
{ "name" : "GPad", "price" : 699, "color" : [ "white", "orange", "gold", "gray" ], "storage" : [ 128, 256, 1024 ] },
{ "name" : "GPhone", "price" : 599, "color" : [ "white", "orange", "gold", "gray" ], "storage" : [ 128, 256, 512 ] }

Given the following query:

db.products.find({$and : [{"price" : {$lte : 800}},
                                       {$or : [{"color" : "purple"}, {"storage" : 1024}]}]})

6. What is the correct output of the query?

(Choose 1)

    a. { "name" : "XPhone", "price" : 799, "color" : [ "white", "black" ], "storage" : [ 64, 128, 256 ] }
    b. { "name" : "XPad", "price" : 899, "color" : [ "white", "black", "purple" ], "storage" : [ 128, 256, 512 ] }
    c. { "name" : "GPhone", "price" : 599, "color" : [ "white", "orange", "gold", "gray" ], "storage" : [ 128, 256, 512 ] }
    d. { "name" : "GPad", "price" : 699, "color" : [ "white", "orange", "gold", "gray" ], "storage" : [ 128, 256, 1024 ] }

explain answer in short and why
ChatGPT said:

The correct answer is:
d. { "name" : "GPad", "price" : 699, "color" : [ "white", "orange", "gold", "gray" ], "storage" : [ 128, 256, 1024 ] }
Explanation:

The query is:

db.products.find({
  $and: [
    { "price": { $lte: 800 } },
    {
      $or: [
        { "color": "purple" },
        { "storage": 1024 }
      ]
    }
  ]
})

So the document must:

    Have price ≤ 800

    AND either:

        Include "purple" in the color array OR

        Include 1024 in the storage array

Let’s check each option:

    a. XPhone

        ✅ Price = 799

        ❌ No "purple" and no 1024 in storage
        ➤ Not matched

    b. XPad

        ❌ Price = 899 (not ≤ 800)
        ➤ Not matched

    c. GPhone

        ✅ Price = 599

        ❌ No "purple" and no 1024 in storage
        ➤ Not matched

    d. GPad

        ✅ Price = 699

        ✅ Has 1024 in storage
        ➤ Matched

✅ Only GPad satisfies both conditions.

6. An inventory collection consists of 200 documents.

What method should be used to get all documents from a cursor using mongosh?

(Choose 1)

    a. db.inventory.findOne()
    b. db.inventory.find().toArray();
    c. db.inventory.find();
    d. db.inventory.findMany().toArray()

explain answer in simple and short
ChatGPT said:

Correct answer: b. db.inventory.find().toArray();
Explanation:

    db.inventory.find() returns a cursor (not actual documents).

    To get all documents from the cursor, use .toArray() to convert the cursor into an array of documents.

    So, db.inventory.find().toArray(); gets all 200 documents.

Why others are wrong:

    a. findOne() returns only one document.

    c. find() returns a cursor, not documents directly.

    d. findMany() is not a valid MongoDB method.

✅ Use .toArray() with .find() to get all documents.

7. A collection has documents like the following:

 { _id: 1, name: 'Oatmeal Fruit Cake with Gummy Bears ', price: 11)},
 { _id: 2, name: 'Cheesecake Trifle with Chocolate Sprinkles ', price: 14)},
 { _id: 3, name: 'Pistachio Brownie with Walnuts ', price: 5},
 { _id: 4, name: 'Strawberry Ice Cream Cake with Butterscotch Syrup ', price: 3)}

How should the 'autocomplete' index be defined to look for matches at the beginning of a word on the name field?

(Choose 1)

    a. {  "mappings": {    "dynamic": false,    "fields": {         "name": [   {  "type": "autocomplete",                              "tokenization": "regexCaptureGroup"} ]     } }}
    b. {  "mappings": {    "dynamic": false,    "fields": {         "name": [   {  "type": "autocomplete",                              "tokenization": "edgeGram"} ]    } }}
    c. {  "mappings": {    "dynamic": false,    "fields": {         "name": [   {  "type": "autocomplete",                              "tokenization": "nGram"} ]    } }}
    d. {  "mappings": {    "dynamic": false,    "fields": {         "name": [   {  "type": "autocomplete",                              "tokenization": "matchNGram"}} ]     } }}

explain it answer in simple and short wh
ChatGPT said:

Correct answer: b.

{
  "mappings": {
    "dynamic": false,
    "fields": {
      "name": [
        {
          "type": "autocomplete",
          "tokenization": "edgeGram"
        }
      ]
    }
  }
}

✅ Explanation (simple & short):

    edgeGram tokenization matches from the beginning of words, which is exactly what autocomplete needs.

    It breaks words into starting chunks (e.g., "Cake" → "C", "Ca", "Cak", "Cake").

❌ Why others are wrong:

    a. regexCaptureGroup is for custom patterns, not ideal for word-start matching.

    c. nGram matches any part of the word, not just the beginning.

    d. matchNGram is not a valid tokenization option.

✅ Use edgeGram for autocomplete that matches the start of words.

8. Given the following sample documents:

{_id:1, name: "Quesedillas Inc.", active: true },
{_id:2, name: "Pasta Inc.", active: true },
{_id:3, name: "Tacos Inc.", active: false },
{_id:4, name: "Cubanos Inc.", active: false },
{_id:5, name: "Chicken Parm Inc.", active: false }

9. A company wants to create a mobile app for users to find restaurants by name. The developer wants to show the user restaurants that match their search. An Atlas Search index has already been created to support this query.

What query satisfies these requirements?

(Choose 1)

    a. db.restaurants.aggregate([{    "$search": {      "text": { "path": "name", "synonym": "cuban"}    } }])
    b. db.restaurants.aggregate([{    "$search": {      "text": { "path": "name", "query": "cuban"}    } }])
    c. db.restaurants.aggregate([{    "$search": {      "text": { "field": "name", "query": "cuban"}    } }])
    d. db.restaurants.aggregate([{    "$search": {      "text": { "field": "name", "synonym": "cuban"}    } }])

explain answer in simple and short
ChatGPT said:

Correct answer: b

db.restaurants.aggregate([
  { "$search": { 
      "text": { 
        "path": "name", 
        "query": "cuban" 
      } 
  }}
])

✅ Why this is correct:

    $search is used in Atlas Search.

    text is the operator for full-text search.

    path correctly points to the field to search in (here, "name").

    query specifies the search term ("cuban").

    This matches documents where the name contains terms like "Cubanos Inc.".

❌ Why others are wrong:

    a: "synonym" is not a valid parameter unless used in a custom analyzer.

    c: "field" is incorrect, should be "path".

    d: Uses both "field" and "synonym", both incorrectly.

So, option b is the only valid and correctly structured search query.

9. Given the data set and query:

{ "_id" : ObjectId("512bc95fe835e68f199c8686"), "player" : "p1", "score" : 89 }
{ "_id" : ObjectId("512bc962e835e68f199c8687"), "player" : "p2", "score" : 85 }
{ "_id" : ObjectId("55f5a192d4bede9ac365b257"), "player" : "p2", "score" : 65 }
{ "_id" : ObjectId("55f5a192d4bede9ac365b258"), "player" : "p3", "score" : 65 }
{ "_id" : ObjectId("55f5a1d3d4bede9ac365b259"), "player" : "p3", "score" : 75 }
{ "_id" : ObjectId("55f5a1d3d4bede9ac365b25a"), "player" : "p5", "score" : 70 }
{ "_id" : ObjectId("55f5a1d3d4bede9ac365b25b"), "player" : "p6", "score" : 100 }


db.scores.aggregate(
[{ $group: {
      _id: '$player',
      score: {
           $avg: '$score'
      }
 },
 { $match: {
     score: { 
            $gt: 70
      }
 }
])

What is the output?​

(Choose 1)

    a. { "player" : "p1", "score" : 89 } { "player" : "p2", "score" : 85 } { "player" : "p3", "score" : 75 } { "player" : "p6", "score" : 100 }
    b. { "player" : "p1", "score" : 89 } { "player" : "p2", "score" : 75 } { "player" : "p6", "score" : 100 }
    c. { "player" : "p1", "score" : 89 } { "player" : "p2", "score" : 75 } { "player" : "p3", "score" : 70 } { "player" : "p5", "score" : 70 } { "player" : "p6", "score" : 100 }
    d. { "player" : "p1", "score" : 89 } { "player" : "p2", "score" : 75 } { "player" : "p3", "score" : 70 } { "player" : "p6", "score" : 100 }

10. A collection coll in database mdb has the following documents :

{_id: 1, type: "A", value: 60}
{_id: 2, type: "B", value: 80}
{_id: 3, type: "C", value: 10}

After executing the following aggregation pipeline:

db.getSiblingDB("mdb").coll.aggregate([
    { $out: {db:'test', collection:'results'}} ])

What are two expected results?

(Choose 2)

    a. Collection results is created in database test.
    b. There is a syntax error command. Collection results is not created.
    c. No documents in collection coll are written to collection results.
    d. All documents in collection coll are written to collection results.

explain answer in simple and short
ChatGPT said:

Correct Answers:

✅ a. Collection results is created in database test.
✅ d. All documents in collection coll are written to collection results.
Explanation (Simple & Short):

    The $out stage writes all output documents from the pipeline into the specified collection.

    { db: 'test', collection: 'results' } tells MongoDB to create the results collection in the test database.

    Since no filter or transformation is applied before $out, all documents from coll are written to results.

No syntax error occurs, so b is wrong.
Documents are written, so c is wrong.

11. Given the following documents:

{_id:1, a: "one", b: "four"}
{_id:2, a: "two", b: "four"}
{_id:3, a: "three", b: "four", c: "three"}

If the following command is executed:

db.coll.replaceOne({}, {a: "ten", b: "five"})

What is the result?

(Choose 1)

    a. {_id:1, a: "ten", b: "five"} {_id:2, a: "ten", b: "five"} {_id:3, a: "ten", b: "five"}
    b. {_id:1, a: "ten", b: "five"} {_id:2, a: "two", b: "four"} {_id:3, a: "three", b: "four", c: "three"}
    c. {_id:1, a: "ten", b: "five"} {_id:2, a: "ten", b: "five"} {_id:3, a: "ten", b: "five", c: "three"}
    d. {_id:1, a: "one", b: "four"} {_id:2, a: "two", b: "four"} {_id:3, a: "three", b: "four", c: "three"}
ChatGPT said:

The correct answer is:
b. {_id:1, a: "ten", b: "five"} {_id:2, a: "two", b: "four"} {_id:3, a: "three", b: "four", c: "three"}
Explanation:

The command:

db.coll.replaceOne({}, {a: "ten", b: "five"})

    replaceOne(filter, replacement) replaces a single document that matches the filter.

    The filter {} is an empty query, which matches any document. MongoDB will match the first document it finds according to its internal ordering (often by _id).

    In this case, it will replace the document with _id:1.

Since it's a replacement, it replaces the entire document except for the _id field.

So the document with _id:1 becomes:

{_id:1, a: "ten", b: "five"}

The other documents are unchanged.
Final State of Collection:

{_id:1, a: "ten", b: "five"}
{_id:2, a: "two", b: "four"}
{_id:3, a: "three", b: "four", c: "three"}

12. Given the collection called coll, with only the following documents,

{
 _id:1,
 a:1,
 b:1
},
{
 _id:2,
 a:2
}


The update operation db.coll.updateMany({},{$set:{b:2}}) successfully completes.

What is the output of db.coll.find()?

(Choose 1)

    a. [{_id:1,  b:2}, {_id:2,  b:2}]
    b. [{_id:1,  a:1,  b:2}, {_id:2,  a:2}]
    c. [{_id:1,  a:1,  b:1}, {_id:2,  a:2,  b:2}]
    d. [{_id:1,  a:1,  b:2}, {_id:2,  a:2,  b:2}]
ChatGPT said:

The operation is:

db.coll.updateMany({}, { $set: { b: 2 } })

This means: for all documents in the coll collection, set the value of field b to 2. The $set operator will:

    Update the value of b to 2 if it exists.

    Add the field b with value 2 if it doesn't exist.

Given the original documents:

{ _id: 1, a: 1, b: 1 }
{ _id: 2, a: 2 }

After the update:

    The first document already has b, so its value changes from 1 to 2.

    The second document does not have b, so the field is added with the value 2.

So the final documents will be:

{ _id: 1, a: 1, b: 2 }
{ _id: 2, a: 2, b: 2 }

✅ Correct answer:

d. [{_id:1, a:1, b:2}, {_id:2, a:2, b:2}]

13. Given the following documents:

{_id:1, a: "one", b: "four"}
{_id:2, a: "two", b: "four"}
{_id:3, a: "three", b: "four", c: "three"}

If the following command is executed:

db.coll.replaceOne({}, {a: "ten", b: "five"})

What is the result?

(Choose 1)

    a. {_id:1, a: "ten", b: "five"} {_id:2, a: "ten", b: "five"} {_id:3, a: "ten", b: "five"}
    b. {_id:1, a: "ten", b: "five"} {_id:2, a: "two", b: "four"} {_id:3, a: "three", b: "four", c: "three"}
    c. {_id:1, a: "ten", b: "five"} {_id:2, a: "ten", b: "five"} {_id:3, a: "ten", b: "five", c: "three"}
    d. {_id:1, a: "one", b: "four"} {_id:2, a: "two", b: "four"} {_id:3, a: "three", b: "four", c: "three"}
ChatGPT said:

The MongoDB command:

db.coll.replaceOne({}, {a: "ten", b: "five"})

does the following:

    replaceOne(filter, replacement) replaces a single document that matches the filter.

    The filter {} matches any document, and MongoDB will match the first document it finds (order is not guaranteed unless specified).

    The replacement {a: "ten", b: "five"} replaces the entire document, except for the _id, which is preserved automatically.

Given the documents:

{_id:1, a: "one", b: "four"}
{_id:2, a: "two", b: "four"}
{_id:3, a: "three", b: "four", c: "three"}

MongoDB will replace the first matching document (likely _id:1) with:

{_id:1, a: "ten", b: "five"}

The other documents remain unchanged.
✅ Correct Answer:

b. {_id:1, a: "ten", b: "five"} {_id:2, a: "two", b: "four"} {_id:3, a: "three", b: "four", c: "three"}

14. Given the collection called coll, with only the following documents,

{
 _id:1,
 a:1,
 b:1
},
{
 _id:2,
 a:2
}

The update operation db.coll.updateMany({},{$set:{b:2}}) successfully completes.

What is the output of db.coll.find()?

(Choose 1)

    a. [{_id:1,  b:2}, {_id:2,  b:2}]
    b. [{_id:1,  a:1,  b:2}, {_id:2,  a:2}]
    c. [{_id:1,  a:1,  b:1}, {_id:2,  a:2,  b:2}]
    d. [{_id:1,  a:1,  b:2}, {_id:2,  a:2,  b:2}]
ChatGPT said:

The operation in question is:

db.coll.updateMany({}, { $set: { b: 2 } })

What does it do?

    updateMany({}): Matches all documents in the collection.

    $set: { b: 2 }: Sets the field b to 2, whether or not it already exists.

Initial documents:

{ _id: 1, a: 1, b: 1 }
{ _id: 2, a: 2 }

After the update:

    Document 1 already has b: 1, which gets updated to b: 2.

    Document 2 does not have field b, so $set adds b: 2.

Final result:

[
  { _id: 1, a: 1, b: 2 },
  { _id: 2, a: 2, b: 2 }
]

✅ Correct answer:

d. {_id:1, a:1, b:2}, {_id:2, a:2, b:2}

15. Given the following document from the cakeFlavors collection. All documents in this collection have the same schema.

{
"_id" : 1,
"flavor" : "chocolate",
"number" : 15
}

What operation on the cakeFlavors collection will update the value of the number field to 100 for a document with a "strawberry" flavor value and insert a new document if it does not exist?

(Choose 1)

    a. db.cakeFlavors.updateOne({ flavor: "strawberry"} , { $set: { number: 100 } }, { $upsert: true })
    b. db.cakeFlavors.insertOne({ flavor: "strawberry"} , { $set: { number: 100 } }, { $upsert: true })
    c. db.cakeFlavors.insertOne({ flavor: "strawberry"} , { $set: { number: 100 } }, { upsert: true })
    d. db.cakeFlavors.updateOne({ flavor: "strawberry"} , { $set: { number: 100 } }, { upsert: true })
ChatGPT said:

The correct answer is:

a. db.cakeFlavors.updateOne({ flavor: "strawberry"} , { $set: { number: 100 } }, { $upsert: true })
Explanation:

You're being asked to update a document with a specific flavor value, and insert it if it doesn’t exist — this is exactly what the upsert option does in an updateOne() operation.

    updateOne(filter, update, options) is the correct method.

    { $set: { number: 100 } } is the correct update operator to change the number field.

    { $upsert: true } ensures that the document is inserted if it doesn’t exist.

Why the other options are wrong:

    b & c: insertOne() does not accept update operators like $set, nor does it take an upsert option. It’s used to insert full documents only.

    d: Almost identical to a, but upsert is not correctly capitalized. MongoDB expects { **upsert**: true }, but best practice is to use $upsert (MongoDB actually allows both, but $upsert is technically incorrect — this is a trick question, and a uses the correct form).

So again, Option a is the correct and most precise answer. ✅

16. Given the following example document from the movie collection:

{
   _id: ObjectId("62872ccd590c3c06d78af00d"),
   genres: [ "Drama", "Romance", "War" ],
   title: "A.B.",
   year: 1921,
   tomatoes: { rating: 3.9, votes: 507, id: "76" },
   countries: [ "USA" ],
   classic : false
}

All documents in this collection have the same schema.

What command updates the value of the classic field to true for all documents with a year value less than 2000?

(Choose 1)

    a. db.movie.updateOne({ year: { $lt: 2000 } }, { $set: { classic: true } }, { multi: true })
    b. db.movie.updateMany({ year: { $lt: 2000 } }, { $set: { classic: true } })
    c. db.movie.updateMulti({ year: { $lt: 2000 } }, { $set: { classic: true } })
    d. db.movie.updateBulk({ year: { $lt: 2000 } }, { $set: { classic: true } })

Explain answer in simple and short
ChatGPT said:

Correct answer: b.
db.movie.updateMany({ year: { $lt: 2000 } }, { $set: { classic: true } })
Explanation (Simple & Short):

    updateMany updates all matching documents, not just one.

    { year: { $lt: 2000 } } finds movies before the year 2000.

    { $set: { classic: true } } changes the classic field to true.

Other options are incorrect because:

    a uses updateOne (only updates one document).

    c and d use non-existent commands (updateMulti, updateBulk).

17. Given the following sample documents in the loans collection:

{ _id: 122, book: "ABC", name: "L.A.", date: ISODate("2022-05-20") },
{ _id: 343, book: "EFF", name: "T.B.", date: ISODate("2022-05-22") },
{ _id: 454, book: "CFH", name: "M.C.", date: ISODate("2022-05-12") }

What command deletes the document where the value of book is "EFF" and user is "T.B.", and returns the deleted document?

(Choose 1)

    a. db.loans.deleteOne( { book: "EFF", name: "T.B." } )
    b. db.loans.findOneAndDelete( { book: "EFF", name: "T.B." } )
    c. db.loans.remove( { book: "EFF", name: "T.B." } , {returnDocument: true} )
    d. db.loans.deleteOne( { book: "EFF", name: "T.B." }, {returnDocument: true} )
ChatGPT said:

The correct answer is:

b. db.loans.findOneAndDelete( { book: "EFF", name: "T.B." } )
Explanation:

    **deleteOne** and **remove** will delete the document, but they do not return the deleted document.

    Only **findOneAndDelete** both deletes and returns the deleted document.

    remove is deprecated, and returnDocument is not a valid option for deleteOne.

✅ Correct syntax:

db.loans.findOneAndDelete({ book: "EFF", name: "T.B." })

This command will:

    Search for the document with book: "EFF" and name: "T.B."

    Delete it

    Return the deleted document as a result

Let me know if you want a quick test query or MongoDB shell example.

18. Given the following sample documents in the inventory collection:

{ _id: 6305, name : "A. S.", "assignment" : 5, "status" : "A" },   
{ _id: 6308, name : "B. M.", "assignment" : 3, "status" : "B" },   
{ _id: 6312, name : "E. M.", "assignment" : 5, "status" : "C" },   
{ _id: 6319, name : "R. S.", "assignment" : 2, "status" : "D" },   
{ _id: 6322, name : "A. S.", "assignment" : 2, "status" : "A" },   
{ _id: 6234, name : "R. S.", "assignment" : 1, "status" : "B" },
{ _id: 6235, name : "A. S.", "assignment" : 1, "status" : "C" },
{ _id: 6315, name : "E. M.", "assignment" : 3, "status" : "A" }

What expression will remove all documents with "status" : "C" in the inventory collection?

(Choose 1)

    a. db.inventory.delete ({"status" : "C"})
    b. db.inventory.deleteOne ({"status" : "C"})
    c. db.inventory.deleteMany ({"status" : "C"})
    d. db.inventory.findOneAndDelete ({"status" : "C"})
ChatGPT said:

The correct answer is:

c. db.inventory.deleteMany({"status" : "C"})
Explanation:

    deleteMany() removes all documents that match the specified condition.

    In this case, we want to remove all documents where "status" : "C".

Why the others are incorrect:

    a. delete() – Not a valid MongoDB method.

    b. deleteOne() – Only deletes one document that matches the condition.

    d. findOneAndDelete() – Finds and deletes one matching document, not all.

✅ Correct command to remove all documents with "status" : "C":

db.inventory.deleteMany({"status" : "C"})

19. Given the following documents in the ratings collection:

{ _id: 0, hotel: "AAA", rating: 4.5 },
{ _id: 1, hotel: "BBB", rating: 3.0 },
{ _id: 2, hotel: "CCC", rating: 4.2 }

What mongosh command will return a document for hotel "CCC"?

(Choose 1)

    a. db.ratings.return_one( {hotel: "CCC"} )
    b. db.ratings.find_one( {hotel: "CCC"} )
    c. db.ratings.returnOne( {hotel: "CCC"} )
    d. db.ratings.findOne( {hotel: "CCC"} )

explain answer in simple and short
ChatGPT said:

Correct answer: d. db.ratings.findOne( {hotel: "CCC"} )
Explanation (simple and short):

In mongosh, the correct method to get one document that matches a condition is **findOne**.

    db.ratings = collection

    .findOne({hotel: "CCC"}) = finds one document where hotel is "CCC"

Other options like return_one, returnOne, and find_one are not valid MongoDB methods.

✅ findOne is correct and valid.

0. The following query generates a collection scan:

db.people.find({employer : "ABC" }).sort ({last_name:1 , job:1})

Which two indexes will most improve the performance of the query?

(Choose 2)

    a. db.people.createIndex({employer:1, last_name : 1  , job : 1 } )
    b. db.people.createIndex({employer:1, last_name : -1  , job : -1 } )
    c. db.people.createIndex({employer:1,last_name : -1  , job : 1 } )
    d. db.people.createIndex({employer:1,last_name : 1  , job : -1 } )
ChatGPT said:

The query is:

db.people.find({ employer: "ABC" }).sort({ last_name: 1, job: 1 })

This query:

    Filters on employer: "ABC"

    Sorts by last_name: 1 and job: 1

To optimize this query and avoid a collection scan, you want a compound index that:

    Starts with the filter field: employer

    Follows the sort order: last_name: 1, job: 1

✅ Best Indexes:

Indexes that match both filter and sort in the correct order will significantly improve performance.

So the best choices are:

    a. db.people.createIndex({employer:1, last_name:1, job:1})

    c. db.people.createIndex({employer:1, last_name:-1, job:1})

Both will work because:

    MongoDB can use indexes in reverse sort order as long as the direction is consistent after the filter key.

In option c, since all fields have consistent direction after the employer field, it can still be used efficiently for sorting.
❌ Less Optimal:

    b. and d. use different sort directions after the filter key, inconsistent with the query sort {last_name: 1, job: 1}. This makes them less optimal or non-usable for this query.

✅ Correct Answers:

a and c ✅
You said:

21. Given a collection called collection, in which all documents have the following shape:

{
 _id:1,
 objs:[
   {a:1,b:2},{a:2,b:1}
 ] 
}

And the query on this collection:

db.collection.find({"objs.a":1})

What index will support this query?

(Choose 1)

    a. {"objs.a":1}
    b. {objs:1}
    c. {objs:1,"objs.a"1}
    d. {objs:1,"a"1}
explain answer in simple and short
ChatGPT said:

The correct answer is:

a. {"objs.a":1}

Explanation:
The query db.collection.find({"objs.a":1}) looks for documents where the array field objs contains an element with a equal to 1.
To optimize this, the index must be on the specific field inside the array elements, which is "objs.a".

    Option a creates an index on the field inside the array elements (objs.a), so the query can efficiently search for objs.a = 1.

    Options b, c, d either index the whole array or incorrect fields and won't efficiently support this query.

In short: Index on "objs.a" is the right choice to speed up queries searching inside array objects by field a.

22. Given the following query:

db.coll.find({}).sort({"product": 1, "price": 1})

Which two indexes will improve the performance of this query?

(Choose 2)

    a. {"product": 1, "price": 1}
    b. {"product": 1, "price": -1}
    c. {"product": -1, "price": 1}
    d. {"product": -1, "price": -1}

explain answer in simple and why is it
ChatGPT said:

Sure! Let's break it down simply.
Query:

db.coll.find({}).sort({"product": 1, "price": 1})

    The query finds all documents ({} means no filter).

    Then it sorts by "product" ascending (1) and "price" ascending (1).

Which indexes help?

Indexes help queries by allowing the database to quickly locate and sort data.

For sorting, the index's sort order should match the query's sort order.
Now, let's look at the options:

    a. {"product": 1, "price": 1}
    Matches exactly: both product and price ascending.
    This will improve performance.

    b. {"product": 1, "price": -1}
    Product ascending, price descending — does NOT match the query's sort order on price.
    This won't help for sorting in this case.

    c. {"product": -1, "price": 1}
    Product descending, price ascending — product order does not match query's ascending order.
    Won't help.

    d. {"product": -1, "price": -1}
    Both descending — the query is ascending, so it doesn't match.
    Won't help.

So the best index is:

    a. {"product": 1, "price": 1}

What about the second index?

Actually, MongoDB can also use the reverse of the index to satisfy the query if the sort order is reversed. So, if the query were descending, index with descending order would help.

But in this question, since query sort is ascending on both fields, the only index that perfectly matches is (a).

But the question says choose two indexes.
MongoDB can use index in ascending order or descending order on the fields. But the index order on fields matters:

    For a compound index { product: 1, price: 1 }, it supports sorting by product ascending, then price ascending.

    For { product: 1, price: -1 }, sorting order conflicts, so doesn't help.

    For { product: -1, price: 1 } or { product: -1, price: -1 }, since product is descending, won't help for ascending sort.

However, MongoDB can scan an index in reverse order.

So if you had an index with both fields descending, and wanted to sort ascending, MongoDB could scan the index backward.

Therefore:

    The index { product: -1, price: -1 } (option d) can be scanned backwards to satisfy the ascending sort.

    The index { product: 1, price: 1 } (option a) can be scanned forwards to satisfy the ascending sort.

Final two indexes that improve the query:

    a. {"product": 1, "price": 1}

    d. {"product": -1, "price": -1}

Summary:
Index	Matches Query Sort?	Notes
a. {"product":1, "price":1}	Yes (exact match)	Best performance
b. {"product":1, "price":-1}	No	Price sort order mismatch
c. {"product":-1, "price":1}	No	Product sort order mismatch
d. {"product":-1, "price":-1}	Yes (if scanned backward)	Can support ascending by backward scan

23. Given the following query:

db.coll.find({}).sort({"product": 1, "price": 1})

Which two indexes improve the performance of this query the most?

(Choose 2)

    a. { v: 2, key: { price: 1, product: 1 }, name: 'price_1_product_1' }
    b. { v: 2, key: { product: 1, price: 1 }, name: 'product_1_price_1' }
    c. { v: 2, key: { price: -1, product: -1 }, name: 'price_-1_product_-1' }
    d. { v: 2, key: { product: -1, price: -1 }, name: 'product_-1_price_-1' }

    Conclusion:

    The two best indexes to improve this query are:

b. { product: 1, price: 1 } — matches the query sort exactly
a. { price: 1, product: 1 } — might help in some cases, but less ideal than b

Simple explanation:

    Your query sorts by product ascending, then price ascending.

    MongoDB works fastest if the index sorts the data in the same order.

    Index b has exactly that order, so it's best.

    Index a has the right ascending order but reversed fields; it's still somewhat helpful.

    Descending indexes (c and d) don't help because your sort is ascending.

24. What mongosh command shows how many indexes are associated with an inventory collection?

(Choose 1)

    a. db.inventory.getIndexes()
    b. db.inventory.showIndexes()
    c. db.inventory.displayIndexes()
    d. db.inventory.indexes()

explain in simple and short to understant
ChatGPT said:

The correct answer is:

a. db.inventory.getIndexes()

Explanation:
db.inventory.getIndexes() shows a list of all indexes on the inventory collection. By counting the items in this list, you know how many indexes there are.

The other options are not valid commands in mongosh.

25. A typical products collection is in an e-commerce database.

What schema is the most effective?

(Choose 1)

    a. Orders for products should be embedded as an array in each product document.
    b. Reviews for products should be embedded as an array in each product document.
    c. Current and historical prices for product should be embedded in each product document.
    d. Current inventory/availability for product should be embedded in each product document.

explain answer in simle and short to understant
ChatGPT said:

The best answer is:

b. Reviews for products should be embedded as an array in each product document.

Why?
Reviews are closely related to a product and usually accessed together. Embedding reviews inside the product makes it easy to get all reviews with the product info in one query, which is efficient.

Why not the others?

    a. Orders: Orders involve many products and customers; embedding orders inside products would cause data duplication and be hard to manage.

    c. Prices: Historical prices can grow large and are better stored separately or in a time-series way.

    d. Inventory: Inventory changes often and quickly; embedding it in product could cause update conflicts—better to keep it separate.


26. A Cooking dataset is in Atlas. There is a Recipes database with a Desserts collection.

How can one document be found that provides a recipe for cookies without chocolate using Atlas Data Explorer?

(Choose 1)

    a. 1. Select the collection on the left-hand side. 2. Select the "Aggregation" view. 3. Specify the first stage as $match query: {dessert_type: "Cookie"} 

4. Specify the second stage as `$match` query:

 {ingredients: {$all: ["chocolate"]}} 5. Set $limit to 1.
    b. 1. Select the collection on the left-hand side. 2. Select the "Aggregation" view. 3. Specify the first stage as $match query: 

{dessert_type: "Cookie", ingredients: {$nin: ["chocolate"]}}

 4. Set $limit to 1.
    c. 1. Select the collection on the left-hand side. 2. Specify the "Find" view. 3. Specify the filter query: 

{dessert_type: "Cookie", ingredients: {$nin: ["chocolate"]}

 4. Specify the project query: 

{dessert_type: 1}
    d. 1. Select the collection on the left-hand side. 2. Specify the "Find" view. 3. Specify the filter query with limit:

 {dessert_type: "Cookie", ingredients: {$nin: ["chocolate"], $limit: 1}

explain in simple and short to understant
ChatGPT said:

The goal is to find one cookie recipe without chocolate in the Desserts collection using Atlas Data Explorer.

Let's break down the options simply:

a.

    Uses two $match stages: first finds cookies, then looks for recipes with chocolate ($all: ["chocolate"]).

    This is wrong because you want without chocolate, not with.

b.

    Uses $match with {dessert_type: "Cookie", ingredients: {$nin: ["chocolate"]}} — means cookie recipes not containing chocolate.

    Then limits results to 1.

    This is correct.

c.

    Uses "Find" with filter {dessert_type: "Cookie", ingredients: {$nin: ["chocolate"]} but no limit mentioned.

    Projection shows only dessert_type.

    Could work but doesn't limit to one result.

d.

    Filter query tries to combine $nin and $limit inside the filter object, which is invalid.

    $limit is a separate parameter, not inside the filter.

Answer:
b is correct — it matches cookies without chocolate and limits to one result.

27. Given the following documents:

{_id:1, restaurant: "Quesadillas Inc.", rating: 4.5 },
{_id:2, restaurant: "Pasta Inc.", rating: 3.9},
{_id:3, restaurant: "Tacos Inc.", rating: 2.5}

A developer wants to find the highest rated restaurant in a list. An index has been created on the appropriate field.

What query satisfies the requirements?

(Choose 1)

    a. const pipeline = [        { $sort: { rating : -1, limit: 1 } } ]; const aggCursor = coll.runAggregation(pipeline);
    b. const pipeline = [        { $sort: { rating : -1 } },    { $limit: 1 } ]; const aggCursor = coll.runAggregation(pipeline);
    c. const pipeline = [    { $sort: { rating : -1 , limit: 1} } ]; const aggCursor = coll.aggregate(pipeline);
    d. const pipeline = [    { $sort: { rating : -1 } },    { $limit: 1 } ]; const aggCursor = coll.aggregate(pipeline);

ChatGPT said:

Let's analyze the options carefully.

You want to find the highest rated restaurant, so:

    You need to sort by rating descending (-1).

    You need to limit the results to 1 to get the top rated.

In MongoDB aggregation pipeline:

    $sort stage accepts only sorting criteria.

    $limit stage limits the number of documents.

So you cannot specify limit inside $sort—it must be a separate stage.

Also, the aggregation method should be aggregate, not runAggregation.

Given that:

    Option a and c incorrectly put limit inside $sort.

    Option a uses runAggregation which is invalid.

    Option c uses aggregate but still incorrect $sort syntax.

    Option b uses runAggregation which is invalid.

    Option d uses proper $sort, $limit stages and calls aggregate.

Answer:

d.

const pipeline = [    
  { $sort: { rating : -1 } },    
  { $limit: 1 } 
]; 
const aggCursor = coll.aggregate(pipeline);

This is the correct query to get the highest rated restaurant.

28. What are two valid method names for MongoClient class?

(Choose 2)

    a. db()
    b. open()
    c. close()
    d. destroy()
ChatGPT said:

The two valid method names for the MongoClient class are:

a. db()
c. close()

Explanation:

    db() is used to access a specific database.

    close() is used to close the connection to the MongoDB server.

Methods like open() and destroy() are not standard MongoClient methods.

So, the correct answers are a and c.

29. What are two advantages of using Connection Pooling within the Javascript (NodeJS) Driver?

(Choose 2)

    a. Reduce application latency
    b. Limit the number of connections to the server
    c. Remove the need for the application to authenticate
    d. Remove the need to open and close connections in the application
ChatGPT said:

The two advantages of using Connection Pooling within the JavaScript (NodeJS) driver are:

a. Reduce application latency
Connection pooling allows reuse of existing connections, which reduces the overhead of establishing new connections, thereby reducing latency.

b. Limit the number of connections to the server
Connection pooling manages a fixed number of connections, preventing the application from opening too many simultaneous connections to the server.

So, the correct answers are:

a. Reduce application latency
b. Limit the number of connections to the server

30. Given the following collection named movies, containing the following six documents:

{ _id: 1, "title": ''A.", "genre": "sci-fi'', "year": "1983" }
{ _id: 2, "title": ''B.", "genre": "comedy'', "year": "1990" }
{ _id: 3, "title": ''C.", "genre": "documentary', "year": "1995" }
{ _id: 4, "title": ''D.", "genre": "sci-fi'', "year": "1985" }
{ _id: 5, "title": ''E.", "genre": "drama'', "year": "2000" }
{ _id: 6, "title": ''F.", "genre": "comedy'', "year": "1977" }

What syntax will update multiple documents in the collection?

(Choose 1)

    a. const filter = { genre: "sci-fi" }; const updateDoc = {    $set: {        genre: "science fiction"        } }; const result = await movies.updateMany(filter, updateDoc);
    b. const filter = { genre = "sci-fi" }; const updateDoc = {        $set = {        genre = "science fiction",        review = 'While very fictional, I found the science lacking'    } }; const result = await movies.updateMany(filter, updateDoc);
    c. const filter = { genre = "sci-fi" }; const updateDoc = {    $set: {                genre = "science fiction",        review = 'I found it neither scientific nor fictional'    } }; const result = await movies.update(filter, updateDoc);
    d. const filter = { genre: "sci-fi" }; const updateDoc = {    $set: {        genre: "science fiction",        review: 'I found it neither scientific nor fictional'    } }; const result = await movies.update(filter, updateDoc);
ChatGPT said:

Let's break down the options based on correct MongoDB update syntax and the method to update multiple documents:

    The correct MongoDB method to update multiple documents is updateMany().

    Filter and update document syntax uses colons : for key-value pairs, not equals =.

    Update operators like $set should be an object with colon syntax, e.g., $set: { genre: "science fiction" }.

Analyzing each option:

a.

const filter = { genre: "sci-fi" };
const updateDoc = { $set: { genre: "science fiction" } };
const result = await movies.updateMany(filter, updateDoc);

    Uses updateMany().

    Proper syntax with : in filter and $set.

    Updates only the genre field.

    This is correct syntax and will update multiple documents.

b.

const filter = { genre = "sci-fi" };
const updateDoc = { $set = { genre = "science fiction", review = 'While very fictional, I found the science lacking' } };
const result = await movies.updateMany(filter, updateDoc);

    Uses = instead of : — syntax error.

    Incorrect assignment inside object literal.

    Incorrect.

c.

const filter = { genre = "sci-fi" };
const updateDoc = { $set: { genre = "science fiction", review = 'I found it neither scientific nor fictional' } };
const result = await movies.update(filter, updateDoc);

    Uses = in filter and inside $set — syntax error.

    Uses update(), which only updates one document by default.

    Incorrect.

d.

const filter = { genre: "sci-fi" };
const updateDoc = { $set: { genre: "science fiction", review: 'I found it neither scientific nor fictional' } };
const result = await movies.update(filter, updateDoc);

    Uses proper syntax with : and $set.

    But uses update() (deprecated) instead of updateMany().

    update() updates one document only by default.

    So not the best choice for updating multiple documents.

Final answer:

a. is the correct choice.