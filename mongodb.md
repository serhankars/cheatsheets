1.INTRODUCTION
============

How to create a database in MongoDB?
-------------------
<pre>
  <b>use</b> dataBaseName
</pre>

How to create a collection in MongoDB?
---------------------
<pre>
db.<b>createCollection</b>( '<collectionName>',
{
capped: <boolean>,
autoIndexId: <boolean>,
size: <number>,
max: <number>,
storageEngine: <document>,
validator: <document>,
validationLevel: <string>,
validationAction: <string>,
indexOptionDefaults: <document>,
viewOn: <string>,
pipeline: <pipeline>,
collation: <document>,
writeConcern: <document>
})
</pre>

> **Insert creates a collection if it doesn't exists:**

<pre>
use yourDatabaseName;
db.myCollectionName.<b>insert</b>(
                                  {"name" : "Yahya A", "company" : "Sony"}
);
</pre>

How to list collections in MongoDB?
-----------------------------------
<pre>
 <b>show collections;</b>
</pre>

How to insert a single document in MongoDB?
-----------------------------------
<pre>
db.blogs.<b>insertOne</b>(
                  { 
                    username: "Zakariya", 
                    noOfBlogs: 100, 
                    tags: ["science","fiction"]
                  }
);
</pre>

How to insert many documents in MongoDB?
----------------------------------------------
<pre>
db.blogs.<b>insertMany</b>(
                    [
                     { username: "Thaha", noOfBlogs: 200, tags: ["science","robotics"]},
                     { username: "Thayebbah", noOfBlogs: 500, tags: ["cooking","general knowledge"]},
                     { username: "Thaherah", noOfBlogs: 50, tags: ["beauty","arts"]}
                    ]
);
</pre>

How to fetch documents in MongoDB?
------------------------------------------------
<pre>
db.collection.<b>find(query, projection)</b>
</pre>
------------------------------------------------
2.SOME DATATYPES IN MONGODB & COMMAND SYNTAX
============
<pre>
var explicitInt = <b>NumberInt</b>("1299")
var explicitLong_double = <b>NumberLong</b>(444333222111242)

var uniqueID = new <b>ObjectId()</b>;
uniqueId.<b>getTimestamp()</b>;  // print document insertion time

var date = <b>Date()</b>; // will be in local timezeone
var date = <b>new  Date()</b>; //If you add the new keyword to the constructor, you get the BSON date that is wrapped in ISODate()
var date = <b>ISODate()</b>;

// The first argument to BinData is a binary subtype to indicate the type of information stored.
// Zero value stands for plain binary data and can be used with text or media files.
var binData = <b>BinData(0,"VghASDI10ZGUUu")</b>
</pre>

Database Commands Syntax in MongoDB
------------------------------------------------
<pre>
db.<b>runCommand</b>( { <db_command> } )
db.<b>runCommand</b>( {currentOp: 1} ) // if we want to retrieve the current operations being executed in MongoDB
db.<b>getCollectionNames()</b> // some commands have their own functions
</pre>
How to create views in MongoDB?
------------------------------------------------
<pre>
db.<b>createView</b>("short_movie_info",
              "movies",
              [ { $project: { "year": 1, "title":1, "plot":1}}]
)
</pre>
------------------------------------------------
3.QUERYING IN MONGODB
==============
<pre>
db.users.<b>find({"name":"David"})</b>
db.comments.<b>find()</b> // find all documents
db.comments.<b>findOne()</b>

var comments = db.comments.find()
comments.<b>next()</b>
comments.<b>hasNext()</b>

// All of the queries have the same behavior
db.comments.<b>find()</b>
db.comments.<b>find({})</b>
db.comments.<b>find({"a_non_existent_field" : null})</b>
</pre>

> Documents are retured in a cursor.
> Typing "it" every time will return the next set of 20 documents until the collection contains more elements.

<pre>
db.comments.<b>find</b>(
                 {"name" : "Lauren Carr"},
                 <b>{"name" : 1, "date": 1}</b> //Projection: Selecting the returned fields
)

db.movies.<b>distinct("rated")</b>
db.movies.<b>distinct</b>("rated", <b>{"year" : 1994}</b>) //all the unique ratings the films that were released in 1994

db.movies.<b>count()</b> // without a query it is based on collection's metadata
db.movies.<b>count({"num_mflix_comments" : 6})</b>
db.movies.<b>countDocuments({"year": 1999})</b> // query argument is mandatory
db.movies.<b>countDocuments({})</b> // to count all documents
db.movies.<b>estimatedDocumentCount()</b> // based on the collection's metadata

db.movies.<b>find({"num_mflix_comments" : 5})</b>
db.movies.<b>find</b>({ "num_mflix_comments" : <b>{$eq : 5 }</b>}) //returns movies that have exactly 5 comments

db.movies.<b>find</b>({year : <b>{$gt : 2015}</b>}).count()
db.movies.<b>find</b>({"released" :<b>{$gte: new Date('2000-01-01')}</b>}).count()

<b>$lt:</b>  less than
<b>$lte:</b> less than or equal to

db.movies.<b>find</b>({"rated" :<b>{$in : ["G", "PG", "PG-13"]}</b>})
db.movies.find({"rated" : <b>{$nin : ["G", "PG", "PG-13"]}</b>})
db.movies.<b>countDocuments</b>(<b>{"non_existing_field" :{$nin : ["a value", "another value", null ]}</b>}) // in mongodb non existent fields always has a value of null
</pre>

Logical Operators
------------------
<pre>
db.movies.<b>countDocuments</b>(
                                 {<b>$and: [{"rated" : "UNRATED"}, {"year" : 2008}]</b>}
)

db.movies.<b>find</b>(
                {
                  <b>$or:[
	                     {"rated" : "G"},
	                     {"year" : 2005},
	                     {"num_mflix_comments" : {$gte : 5}}
]</b>}

<b>$nor:</b> The $nor operator is syntactically like $or but behaves in the opposite way.

db.movies.<b>find</b>(
                      {
                        "num_mflix_comments" :<b>{$not : {$gte : 5} }</b>
                      }
)

db.movies.<b>find</b>(
               {"title" : <b>{$regex :"Opera"}</b>}
)

db.movies.<b>find</b>(
	             {"title" : <b>{$regex :"^Opera"}</b>}  // that <b>starts</b> with Opera
)

db.movies.<b>find</b>(
	             {"title" : <b>{$regex :"Opera$"}</b>} //that <b>ends</b> with Opera
)

db.movies.<b>find</b>(
      {
         "title" :{<b>"$regex"</b> : "the", <b>$options: "i"</b>} // case insensitive option
      }
)

// if <b>array field</b> contains at least one element that satisfies the query
db.movies.<b>find</b>(
    {  $and :[
               {"cast" : "Charles Chaplin"},
               {"cast": "Edna Purviance"}
             ]
    }
)

// <b>querying with an array value:</b> Array values must match <b>exactly</b> (order etc.)
db.movies.<b>find</b>(
                <b>{"languages" : ["German", "English"]}</b>
)

// the <b>$all</b> operator finds all those documents where the value of the field contains all
// the elements, irrespective of their order or size:
db.movies.<b>find</b>(
                      {
                       "languages":{<b>"$all" :[ "English", "French", "Cantonese"]</b>}
                      }
)
</pre>

How to project array fields in MongoDB?
--------------- 
<pre>
db.movies.find(
                {"languages" : "Syriac"}, 
                {"languages<b>.$</b>" :1}   // if more than one element is matched, the <b>$ operator projects only the first matching element</b>
)

db.movies.<b>find</b>({"title" : "Youth Without Youth"},
               {"languages" : <b>{$slice : 3}</b>} //first three elements of the array (you can query different field and still use this operator)
).<b>pretty()</b>

{"languages" : <b>{$slice : -2}</b>} // last two elements
{"languages" : <b>{$slice : [2, 4]</b>}} // skip first two elements and get four elements
</pre>

How to query nested objects in MongoDB?
---------------------- 
> This means that <b>all the field-value pairs, along with the order of the fields must match exactly.</b>
<pre>
db.movies.<b>find</b>(
               {"awards":
                        <b>{"wins": 1, "nominations": 0, "text": "1 win."}</b>
               }
)
</pre>

How to query by nested objects' fields in MongoDB?
-----------------------------
<pre>
db.movies.find(
                <b>{"awards.wins" : 4}</b>
)
</pre>

How to project nested objects' fields in MongoDB?
---------------------------------
<pre>
db.movies.find(
               {},
               {
                <b>"awards.wins" :1,
                "awards.nominations" : 1,
                "_id":0
                </b>
               }
)
</pre>

How to limit query result in MongoDB?
---------------------
<pre>
db.movies.find(
               {"cast" : "Charles Chaplin"},
               {"title": 1, "_id" :0}
).<b>limit(3)</b>

limit(0) //returns all
</pre>

> Different MongoDB drivers can have different batch sizes. However, for a single query, the batch size can be set.

<pre>
db.movies.find(
               {"cast" : "Charles Chaplin"},
               {"title": 1, "_id" :0}
).<b>batchSize(5)</b>
</pre>

> The skip() operation does not make use of any indexes, so it performs nicely on a smaller collection but may lag noticeably on larger collections.
<pre>
db.movies.find(
			         {"cast" : "Charles Chaplin"},
			         {"title": 1, "_id" :0}
).<b>skip(2)</b>  

db.movies.find(
			{"cast" : "Charles Chaplin"},
			{"title" : 1, "_id" :0}
).<b>sort({"title" : 1}) //1:ascending -1:descending</b>
</pre>
------------------------------------

4.INSERT, UPDATE, DELETE OPERATIONS
==============

How to insert documents in MongoDB?
--------------------
<pre>
db.collection.<b>insert(<Document To Be Inserted>)</b>
db.new_movies.<b>insertMany([
                          {"_id" : 2, "title": "Baby Driver"},
                          {"_id" : 3, "title": "Logan"},
                          {"_id" : 4, "title": "John Wick: Chapter 2"},
                          {"_id" : 5, "title": "A Ghost Story"}
])</b>
</pre>

How to delete documents in MongoDB?
--------------------
<pre>
db.new_movies.<b>deleteOne({"_id": 2})</b>
db.new_movies.<b>deleteMany({"title" : {"$regex": "^movie"}})</b>

// Difference <b>findOneAndDelete</b> of from deleteOne: 
// [1] you can sort before delete,
// [2] returns deleted item
// [3] you can project deleted item

db.new_movies.<b>findOneAndDelete</b>( 
                                  {"title" : {"$regex" : "^movie"}},
                                  {<b>sort</b> : {"_id" : -1}}
)

db.new_movies.<b>findOneAndDelete</b>(
                                  {"title" : {"$regex" : "^movie"}},
                                  {
                                   <b>sort</b> : {"_id" : -1}, 
                                   <b>projection</b> : {"_id" : 0, "title" : 1}
                                  }
)
</pre>

How to replace documents in MongoDB?
--------------------
<pre>
db.users.<b>replaceOne(
                    {"_id" : 5},
                    {"name": "Margaery Baratheon", "email": "Margaery.Baratheon@got.es"} // beware: no _id field in the updated document
)
</b>

// <b>upsert</b> option: insert if not found
db.users.<b>replaceOne</b>(
                    {"name" : "Margaery Baratheon"},
                    {"name": "Margaery Tyrell", "email": "Margaery.Tyrell@got.es"},
                    { <b>upsert:</b> true }
)
</pre>

<pre>
db.movies.<b>findOneAndReplace</b>(
                                   {"title" : "Macbeth"}, 
                                   {"title" : "Macbeth", "latest" : true},
                                   {
                                    <b>sort:</b> {"_id" : -1},
                                    <b>projection:</b> {"_id" : 0}
					                          <b>returnNewDocument:</b> true
                                   }
)
</pre>

**Its effect is equal to:**

<pre>
<b>var deletedDocument</b> = db.movies.<b>findOneAndDelete</b>(
                                                               {"title" : "Macbeth"},
                                                               {sort : {"_id" : -1}}
                                                              );
db.movies.<b>insert</b>(
                 {"_id" : deletedDocument._id, "title" : "Macbeth", "latest" : true}
)
</pre>

How to update documents in MongoDB?
--------------------
<pre>
db.movies.<b>updateOne</b>(
                    {"title" : "Macbeth"},
                    {<b>$set :</b> {"year" : 2015}}
)

// can be used to update multiple fields in the same command,
// and if a field is new, it will be added to the document
db.movies.<b>updateOne</b>(
                    {"title" : "Macbeth"},
                    { <b>$set :</b> {"type" : "movie", "num_mflix_comments" : 1}}
)

//If not found it is inserted and and it will even have title field.
db.movies.<b>updateOne</b>(
                    {"title" : "Sicario"},
                    {$set : {"year" : 2015}},
                    {"upsert" : true}
)

// <b>by default it returns the old document</b>
db.movies.<b>findOneAndUpdate</b>(
                          {"title" : "Macbeth"},
                          { <b>$set :</b> {"num_mflix_comments" : 10}}
)

db.movies.<b>findOneAndUpdate</b>(
                           {"title" : "Macbeth"},
                           {<b>$set :</b> {"num_mflix_comments" : 15}},
                           {
                             <b>"projection" :</b> {"_id" : 0, "num_mflix_comments" : 1},
                             <b>"returnNewDocument" :</b> true,
					                   <b>"sort" :</b> {"_id" : -1}
                           }
)

db.movies.<b>updateMany</b>(
                     {"year" : 2015},
                     {<b>$set :</b> {"languages" : ["English"]}}
)
</pre>

Some update operators in MongoDB
--------------------------------
Set ($set)

Increment ($inc):
db.movies.findOneAndUpdate(
                           {"title" : "Macbeth"},
                           {$inc : {"num_mflix_comments" : 3, "rating" : 1.5}},
                           {returnNewDocument : true}
)


Multiply ($mul):
// When using a non-existent field with $mul, we should always remember that no matter what
// multiplier we provide, the field will be created and always set to zero
db.movies.findOneAndUpdate(
                           {"title" : "Macbeth"},
                           {$mul : {"rating" : 2}},
                           {returnNewDocument : true}
)

Rename ($rename):
//The provided field and its new name must be different. 
//If they're the same, the operation fails with an error

db.movies.findOneAndUpdate(
                           {"title" : "Macbeth"},
                           {$rename : {"num_mflix_comments" : "comments",
                                       "imdb_rating" : "rating"}},
                           {returnNewDocument : true}
)


db.movies.findOneAndUpdate(
                            {"title" : "Macbeth"},
                            {$rename : {"rating" : "imdb.rating"}}, // a field can be moved to nested document.
                            {returnNewDocument : true}
)

Current Date ($currentDate):
db.movies.findOneAndUpdate(
                           {"title" : "Macbeth"},
                           {$currentDate : {
                                             "created_date" : true, // defaults type to Date type
                                             "last_updated.date" : {$type : "date"},
                                             "last_updated.timestamp" : {$type : "timestamp"},
                                           }},
                           {returnNewDocument : true}
)


Removing Fields ($unset):
//values have no effect
db.movies.findOneAndUpdate(
                           {"title" : "Macbeth"},
                           {
                             $unset : {
                                       "created_date" : "",
                                       "last_updated" : "dummy_value",
                                       "box_office_collection": 142.2,
                                       "imdb" : null,
                                       "flag" : ""
                                      }
                          },
                          {returnNewDocument : true}
)

Setting When Inserted ($setOnInsert):
db.movies.findOneAndUpdate(
                            {"title":"Macbeth"},
                            {
                              $rename:{"comments":"num_mflix_comments"},
                              $setOnInsert:{"created_time":new Date()}
                            },
                            {
                              upsert : true,
                              returnNewDocument:true
                            }
)


*******************************************************
CHAPTER 6 UPDATING WITH AGGREGATION PIPELINE AND ARRAYS
*******************************************************
db.users.updateMany( {},
                     [
                      {$set : {"name_array" : {$split : ["$full_name", " "]}},},
                      {
                        $set: {
                               "first_name" : {"$arrayElemAt" : ["$name_array", 0]},
                               "last_name" : {"$arrayElemAt" : ["$name_array", 1]}
                               }
                      },
                      {
                         $project : {
                                     "first_name" : 1,
                                     "last_name" : 1,
                                     "full_name" : {
                                                     $concat : [{$toUpper : "$first_name"}, " ", "$last_name"]
                                                   }
                                    }
                      }
                     ]
)

db.movies.findOneAndUpdate(
                           {_id : 111},
                           {$push : {"genre" : "unknown"}},
                           {"returnNewDocument" : true}
)


db.movies.findOneAndUpdate(
                           {_id : 111},
                           {$push : {
                                     "genre" : {
                                                $each : ["History", "Action"]
                                               }
                                    }
                           },
                           {"returnNewDocument" : true}
)


db.movies.findOneAndUpdate(
                           {_id : 111},
                           {$push : {
                                     "genre" : {
                                                $each : [],
                                                $sort : 1
                                                }
                                    }
                           },
                           {"returnNewDocument" : true}
)

//sorts using properties of array items
db.items.findOneAndUpdate(
                          {_id : 11},
                          {$push : {
                                    "items" : {
                                                $each : [],
                                                $sort : {"price" : -1}
                                              } 
                                   }
                          },
                          {"returnNewDocument" : true}
)

//$addToSet adds only unique elements
db.movies.findOneAndUpdate(
                           {_id : 111},
                           {$addToSet : {"genre" : "Action"}},
                           {"returnNewDocument" : true }
)

//Removing the First or Last Element ($pop)
db.movies.findOneAndUpdate(
					{_id : 111},
					{$pop : {"genre" : 1}},      //1:removes last element, -1: removes first element
					{"returnNewDocument" : true }
)

//Removing All Elements with a value
db.movies.findOneAndUpdate(
                           {_id : 111},
                           {$pullAll : {"genre" : ["Action", "Crime"]}},
                           {"returnNewDocument" : true }
)

//Removing by querying
db.items.findOneAndUpdate(
                          {_id : 11},
                          { $pull : {
                                     "items" : {
                                                 "quantity" : 3,
                                                 "name" : {$regex: "ck$"}
                                               }
                                    }
                          },
                          {"returnNewDocument" : true }
)

// updates all
db.movies.findOneAndUpdate(
                          {_id : 111},
                          {$set : {"genre.$[]" : "Action"}},
                          {"returnNewDocument" : true}
)

//Updating a specific array item using filters
// arrayFilters keyword definethe query condition myElements
db.items.findOneAndUpdate(
                          {_id : 11},
                          {$set : {
                                   "items.$[myElements]" : {
                                                            "quantity" : 7,
                                                            "price" : 4.5,
                                                            "name" : "marker"
                                                           }
                                  }
                          },
                          {
                            "returnNewDocument" : true,
                            "arrayFilters" : [{"myElements.quantity" : null}]
                          }
)


*******************************************************
CHAPTER 7 DATA AGGREGATION
*******************************************************
use sample_mflix;
var pipeline = [] // The pipeline is an array of stages.
var options = {} 
var cursor = db.movies.aggregate(pipeline, options);


var pipeline = [
                { $match: { "location.address.state": "MN"} },
                { $project: { "location.address.city": 1 } },
                { $sort: { "location.address.city": 1 } },
                { $limit: 3 }
];


db.theaters.aggregate(pipeline)

db.theaters.aggregate(pipeline).forEach(printjson);



var pipeline = [
{ $match: {genres: "Romance", released: {$lte: new ISODate("2001-01-01T00:00:00Z") }}},
{ $sort: {"imdb.rating": -1}}, 
{ $limit: 3 },
];
db.movies.aggregate(pipeline).forEach(printjson);

//$group
// In aggregation terms, an expression can be a literal, 
// an expression object, an operator,or a field path.

// The $group stage will interpret _id: "$rated" as equivalent to _id: "$$CURRENT.rated".
var pipeline = [
                {$group: {
                          _id: "$rated"
                         }
                }
               ];

//ACCUMULATOR EXPRESSIONS
var pipeline = [
                {$group: {
                           _id: "$rated",
                           "numTitles": { $sum: 1},
                         }
                }
];
db.movies.aggregate(pipeline).forEach(printjson);

//$avg accumulator
{$group: {
           _id: "$rated",
           "avgRuntime": { $avg: "$runtime"},
}}

// $trunc stage to give integer value
{$project: {
"roundedAvgRuntime": { $trunc: "$avgRuntime"}
}}



var pipeline = [
                { $match: {released: {$lte: new ISODate("2001-01-01T00:00:00Z") }
                          }
                },
                { $group: {
                      _id: {"$arrayElemAt": ["$genres", 0]},
                      "popularity": { $avg: "$imdb.rating"},
                      "top_movie": { $max: "$imdb.rating"},
                      "longest_runtime": { $max: "$runtime"}
                     }
                },
                { $sort: { popularity: -1}},
                { $project: {
                         _id: 1,
                         popularity: 1,
                         top_movie: 1,
                         adjusted_runtime: { $add: [ "$longest_runtime",12 ] //12 value is the length of trailers
                       } 
                 } 
                }
];


// $first accumulator is used to choose first element of the group
{ $group: {
           _id: {"$arrayElemAt": ["$genres", 0]},
           "recommended_title": {$first: "$title"},
           "recommended_rating": {$first: "$imdb.rating"},
           "recommended_raw_runtime": {$first: "$runtime"},
           "popularity": { $avg: "$imdb.rating"},
           "top_movie": { $max: "$imdb.rating"},
           "longest_runtime": { $max: "$runtime"}
          }
},

//$sample
// The primary reason is that $limit always respects the order of the documents 
// and thus returns the same documents every time
{ $sample: {size: 100}}, // This will reduce the scope to
100 random docs.

//$lookup: JOINING TWO COLLECTIONS
var pipeline = [
                { $match: { $or: [{"name": "Catelyn Stark"},{"name": "Ned Stark"}]}},
                { $lookup: {
                             from: "comments",
                             localField: "name",
                             foreignField: "name",
                             as: "comments"
                            }
                },
                { $limit: 2},
];
db.users.aggregate(pipeline).forEach(printjson);

//$unwind
Converts this: {a: 1, b: 2, c: [1, 2, 3, 4]}
To this: 
{"a" : 1, "b" : 2, "c" : 1 }
{"a" : 1, "b" : 2, "c" : 2 }
{"a" : 1, "b" : 2, "c" : 3 }
{"a" : 1, "b" : 2, "c" : 4 }

{ $unwind: "$comments"}


// SAVING OUPUT OF A QUERY TO ANOTHER COLLECTION
$out; the only parameter to specify is the desired output collection.
{ $out: "myOutputCollection"}
It will either create a new collection or completely replace an existing collection.
$out must output to the same database as the aggregation target.

//From Mongo 4.2 $merge is available
{ 
	$merge: {
			// This can also accept {db: <db>, coll: <coll>} to
			//merge into a different dbinto: "myOutputCollection",
              }
}

//AN EXAMPLE
var pipeline = [
                { $sample: {size: 5000}},
                { $group: {
                           _id: "$movie_id",
                           "sumComments": { $sum: 1}
                          }
                },
               { $sort: { "sumComments": -1}},
               { $limit: 5},
               { $lookup: {
                            from: "movies",
                            localField: "_id",
                            foreignField: "_id",
                            as: "movie"
                          }
               },
               { $unwind: "$movie" },
               { $project: {
                             "movie.title": 1,
                             "movie.imdb.rating": 1,
                             "sumComments": 1,
                           }
               },
               { $out: "most_commented_movies" }
];

//AGGREGATE OPTIONS
maxTimeMS: The amount of time an operation may be processed before MongoDB kills it.
allowDiskUse: true means using disk is allowed to handle massive datasets
bypassDocumentValidation: No validation of output when using $out or $merge
comment: This options is for debugging allows a string to be specified

var options = {
               maxTimeMS: 30000,
               allowDiskUse: true
}
db.movies.aggregate(pipeline, options);


***************************************************
	           INDEXES IN MONGODB
***************************************************
db.movies.explain().find(
                          {
                           "year" : 2015
                          },
                          {
                           "title" : 1,
                           "awards.wins" : 1
                          }
                         ).sort(
                                {"awards.wins" : -1}
                               )

explain() function can take an optional parameter:verbosityMode
Possible Values:
queryPlanner: default option, print query planner detail, rejected plans, winning plan, execution stagesof winning plan 
executionStats: Useful for finding performance related problems
allPlansExecution: 


//CREATING INDEX
db.movies.createIndex(
                      {year: 1} //1 is ascending
)


db.movies.getIndexes()

db.theaters.createIndex( 
                        {theaterId : -1},
                        {name : "myTheaterIdIndex"}
);

db.movies.createIndex(
                      {year : 1, rated : 1} //compound index
)

db.theaters.createIndex(
                        { "location.address.zipcode" : 1}
)

db.theaters.createIndex(
                        { "location" : 1}
)

//Wildcard indexes for dynamic schema
db.products.createIndex(
                        { "specifications.$**" : 1}
)

//For all fields
db.products.createIndex(
                        { "$**" : 1 }
)

//You can also omit wildcard indexes
db.products.createIndex(
                         { "$**" : 1 },
                         {
                           "wildcardProjection" : { "name" : 0 }
                         }
)

db.collection.createIndex(
                          { field: type},
                          { unique: true }
)

// Time to live TTL indexes
// Document (not the index) will be deleted after a time
// The index can be defined on a field of date type
db.collection.createIndex({ field: type}, { expireAfterSeconds: seconds })

//A sparse index will not have entries from the collection where the indexed field does not exist
db.collection.createIndex({ field1 : type, field2: type2, ...}, { sparse:
true })

//PARTIAL INDEX(Index for documents that satisfy some condition)
db.movies.createIndex(
                      {title: 1, type:1},
                      {
                        partialFilterExpression: {year : { $gt: 1950}}
                      }
)

//case insensitive index
db.movies.createIndex(
                       {title: 1},
                       {
                         collation: {
                                      locale: 'en', strength: 2
                                    }
                        }
)

db.collection.dropIndex(indexNameOrSpecification);

db.collection.dropIndexes() //drops all indexes except default _id index

db.movies.explain("executionStats").find()

db.collection.hideIndex(indexNameOrSpecification); //to analyze performance impact without removing
db.collection.unhideIndex(indexNameOrSpecification);

db.users.createIndex(
                     { name : "text"} //This is text index. Since it is not sorted it is faster. Can be defined on text fields and arrays with texts.
)

// We can give hints to MongoDB query planner to use which index
db.users.find().hint(
                     { index }
)