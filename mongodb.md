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
db.collection.<br>find(query, projection)</br>
</pre>

2.SOME DATATYPES IN MONGODB
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

------------------------------------------------
DATABASE COMMANDS
------------------------------------------------
db.runCommand( { <db_command> } )

db.runCommand( {currentOp: 1} ) // if we want to retrieve the current operations being executed in MongoDB
db.getCollectionNames()

//CRAETING VIEW
db.createView(
"short_movie_info",
"movies",
[ { $project: { "year": 1, "title":1, "plot":1}}]
)


db.short_movie_info.findOne()

-------------------------------------------------
3QUERYING DATA
-------------------------------------------------
db.users.find({"name":"David"})

db.comments.find() // find all documents
db.comments.findOne()

var comments = db.comments.find()
comments.next()
comments.hasNext()

// All of the queries have the same behavior
db.comments.find()
db.comments.find({})
db.comments.find({"a_non_existent_field" : null})

Typing "it" every time will return the next set of 20 documents until the
collection contains more elements.


//PROJECTION
db.comments.find(
{"name" : "Lauren Carr"},
{"name" : 1, "date": 1}
)

db.movies.distinct("rated")

db.movies.distinct("rated", {"year" : 1994}) //all the unique ratings the films that were released in 1994

db.movies.count()
db.movies.count({"num_mflix_comments" : 6})
db.movies.countDocuments({"year": 1999}) //query argument is mandatory
db.movies.countDocuments({}) // to count all documents
db.movies.estimatedDocumentCount()// Based on the collections metadata

db.movies.find({"num_mflix_comments" : 5})
db.movies.find({ "num_mflix_comments" : {$eq : 5 }}) //returns movies that have exactly 5 comments

db.movies.find({year : {$gt : 2015}}).count()
db.movies.find({"released" :{$gte: new Date('2000-01-01')}}).count()

$lt : less than
$lte: less than or equal to

db.movies.find({"rated" :{$in : ["G", "PG", "PG-13"]}})
db.movies.find({"rated" :{$nin : ["G", "PG", "PG-13"]}})
db.movies.countDocuments({"nef" :{$nin : ["a value", "another value", null ]}}) // in mongodb non existent fields always has a value of null


//LOGICAL OPERATORS
db.movies.countDocuments (
{$and : [{"rated" : "UNRATED"}, {"year" : 2008}]}
)

db.movies.find(
{$or:[
	{"rated" : "G"},
	{"year" : 2005},
	{"num_mflix_comments" : {$gte : 5}}
]}

$nor: The $nor operator is syntactically like $or but behaves in the opposite way.

db.movies.find(
 {"num_mflix_comments" :
      {$not : {$gte : 5} }
 }
)

db.movies.find(
  {"title" : {$regex :"Opera"}}
)

db.movies.find(
	{"title" : {$regex :"^Opera"}}  //that starts with Opera
)

db.movies.find(
	{"title" : {$regex :"Opera$"}} //that ends with Opera
)

db.movies.find(
      {
         "title" :{"$regex" : "the", $options: "i"} // case insensitive option
      }
)

// if array field contains at least one element that satisfies the query
db.movies.find(
    {  $and :[
               {"cast" : "Charles Chaplin"},
               {"cast": "Edna Purviance"}
             ]
    }
)

// Querying with an array value: Array values must match exactly (order etc.)
db.movies.find(
{"languages" : ["German", "English"]}
)

//$all operator
//The $all operator finds all those documents where the value of the field contains all
//the elements, irrespective of their order or size:
db.movies.find(
{
  "languages":{
               "$all" :[ "English", "French", "Cantonese"]
              }
})

//ARRAY PROJECTION 
db.movies.find
{"languages" : "Syriac"}, 
{"languages.$" :1}   //if more than one element is matched, the $ operator projects only the first matching element
)

db.movies.find(
{
   "title" : "Youth Without Youth"
},
{"languages" : {$slice : 3}} //first three elements of the array (you can query different field and still use this operator)
).pretty()

{"languages" : {$slice : -2}} // last two elements

{"languages" : {$slice : [2, 4]}} //skip first two elements and get four elements


//QUERYING NESTED OBJECTS: This means that all the field-value pairs, along with the order of the fields must match exactly.
db.movies.find(
               {"awards":
                        {"wins": 1, "nominations": 0, "text": "1 win."}
               }
)

//QUERYING NESTED OBJECT FIELDS
db.movies.find(
                {"awards.wins" : 4}
)

//To project only specified fields.
db.movies.find(
               {},
               {
                "awards.wins" :1,
                "awards.nominations" : 1,
                "_id":0
               }
)

//LIMITING
db.movies.find(
               {"cast" : "Charles Chaplin"},
               {"title": 1, "_id" :0}
).limit(3)

limit(0) //returns all

Different MongoDB drivers can have different batch sizes. 
However, for a single query, the batch size can be set.

db.movies.find(
               {"cast" : "Charles Chaplin"},
               {"title": 1, "_id" :0}
).batchSize(5)


db.movies.find(
			{"cast" : "Charles Chaplin"},
			{"title": 1, "_id" :0}
).skip(2)  
// The skip() operation does not make use of any indexes, so it performs nicely on a smaller collection
// but may lag noticeably on larger collections.

db.movies.find(
			{"cast" : "Charles Chaplin"},
			{"title" : 1, "_id" :0}
).sort({"title" : 1})  //1:ascending -1:descending

--------------------------------------------
	INSERT UPDATE DELETE
--------------------------------------------

db.collection.insert( <Document To Be Inserted>)

db.new_movies.insertMany([
                          {"_id" : 2, "title": "Baby Driver"},
                          {"_id" : 3, "title": "Logan"},
                          {"_id" : 4, "title": "John Wick: Chapter 2"},
                          {"_id" : 5, "title": "A Ghost Story"}
])


db.new_movies.deleteOne({"_id": 2})
db.new_movies.deleteMany({"title" : {"$regex": "^movie"}})

//Difference from deleteOne: you can sort before delete,
// returns deleted item
// you can project deleted item

db.new_movies.findOneAndDelete( 
{"title" : {"$regex" : "^movie"}},
{sort : {"_id" : -1}}
)


db.new_movies.findOneAndDelete(
{"title" : {"$regex" : "^movie"}},
           {
            sort : {"_id" : -1}, 
            projection : {"_id" : 0, "title" : 1}
           }
)


db.users.replaceOne(
                    {"_id" : 5},
                    {"name": "Margaery Baratheon", "email": "Margaery.Baratheon@got.es"} //no _id field in the updated document
)

//UPSERT OPTION
db.users.replaceOne(
                    {"name" : "Margaery Baratheon"},
                    {"name": "Margaery Tyrell", "email": "Margaery.Tyrell@got.es"},
                    { upsert: true }
)

db.movies.findOneAndReplace(
                             {"title" : "Macbeth"}, 
                             {"title" : "Macbeth", "latest" : true},
                             {
                               sort : {"_id" : -1},
                               projection : {"_id" : 0}
					    returnNewDocument : true
                             }
)

var deletedDocument = db.movies.findOneAndDelete(
{"title" : "Macbeth"},
{sort : {"_id" : -1}}
)
db.movies.insert(
{"_id" : deletedDocument._id, "title" : "Macbeth", "latest" : true}
)


db.movies.updateOne(
                    {"title" : "Macbeth"},
                    {$set : {"year" : 2015}}
)

//can be used to update multiple fields in the same command,
//and if a field is new, it will be added to the document
db.movies.updateOne(
                    {"title" : "Macbeth"},
                    {$set : {"type" : "movie", "num_mflix_comments" : 1}}
)

//If not found it is inserted and and it will even have title field.
db.movies.updateOne(
                    {"title" : "Sicario"},
                    {$set : {"year" : 2015}},
                    {"upsert" : true}
)

//By default it returns the old document
db.movies.findOneAndUpdate(
                          {"title" : "Macbeth"},
                          {$set : {"num_mflix_comments" : 10}}
)


db.movies.findOneAndUpdate(
                           {"title" : "Macbeth"},
                           {$set : {"num_mflix_comments" : 15}},
                           {
                             "projection" : {"_id" : 0, "num_mflix_comments" : 1},
                             "returnNewDocument" : true,
					  "sort" : {"_id" : -1}
                           }
)

db.movies.updateMany(
                     {"year" : 2015},
                     {$set : {"languages" : ["English"]}}
)

//UPDATE OPERATORS
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