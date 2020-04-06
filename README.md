# MongoDB

```js
show dbs
use my_db

db

db.createCollection("sample_collection1")
db.createCollection("sample_collection2")

show collections

db.sample_collection2.drop()

show collections

db.createCollection("sample_collection2")

db.dropDatabase()
```

## Create moviedb and create a collection name movies

```js
use moviedb
db.createCollection("movies")
```

## Insert One

```js
db.movies.insertOne({"title" : "Stand by Me"}) 
db.movies.insertOne({"title" : "Kingsman: The Secret Service"}) 
db.movies.insertOne({"title" : "Tinker, Tailor, Soldier, Spy"}) 
db.movies.insertOne({"title" : "Argo"})

db.movies.find()
```

## Insert Many
```js
db.movies.insertMany([ 
    {"title" : "Ghostbusters"}, 
    {"title" : "E.T."}, 
    {"title" : "Blade Runner"}, 
    {"title" : "007"}, 
    {"title" : "War"}, 
    {"title" : "IP Man"} 
])
```

## Find
```js
db.movies.find()
db.movies.find().pretty()
```

## Find One

```js
db.movies.findOne({title:"War"})
```

## Insert One Array
```js
db.movies.insertOne({ 
    "title": "Ip Man 2", 
    "rating": 7.6, 
    "director": ["Wilson Yip"], 
    "languages": [ "English", "Standard Mandarin", "Cantonese"],
    "actors": [ 
    "Donnie Yen", 
    "Sammo Hung", 
    "Lynn Hung", 
    "Louis Fan", 
    "Simon Yam" 
    ]
})
```

```js
db.movies.insertOne({ 
    "title": "Ip Man 3", 
    "rating": 7.1, 
    "director": ["Wilson Yip"], 
    "languages": [ "English", "Standard Mandarin", "Cantonese"], 
    "actors": [ 
    "Donnie Yen", 
    "Zhang Jin", 
    "Mike Tyson", 
    "Danny Chan Kwok-kwan", 
]})
```

## Insert Many Array
```js
db.movies.insertMany([
{
  "title": "Black Panther",
  "rating": 7.4,
  "director": ["Ryan Coogler"],
  "languages": [
        "English",
   ],
"actors": [
        "Chadwick Boseman",
        "Michael B. Jordan",
        "Lupita Nyong'o",
  ]
},
{
  "title": "Mission: Impossible – Fallout",
  "rating": 7.8,
  "director": ["Christopher McQuarrie"],
  "languages": [
        "English",
   ],
  "actors": [
        "Tom Cruise",
        "Henry Cavill",
        "Rebecca Ferguson",
  ]
}])
```

## Create a new collection users
```js
db.createCollection("users")
db.users.insertMany(
   [
      {
         "name":"Max",
         "programming_languages":["Python", "Go", "C", "C++"],
         "phone":"131782734"
      },
      {
         "name":"Manuel",
         "programming_languages":["Java", "C", "C++", "ASM"],
         "phone":"012177972",
         "age":30
      },
      {
         "name":"Manuel",
         "programming_languages":["Java", "C", "C++", "ASM"],
         "phone":"312177972",
         "age":39
      },
      {
         "name":"Suresh",
         "programming_languages":["Python", "C", "C++", "Java", "ASM"],
         "phone":"80811987291",
         "age":null
      },
      {
         "name":"Anna",
         "programming_languages":["Python", "C", "C++", "Java", "ASM"],
         "phone":"80811987291",
         "age":null
      },
      {
         "name":"Chris",
	   "programming_languages":[]	
      },
   ]
)

db.users.find()
```

## Update One $set
```js
db.users.updateOne({name:"Chris"}, {$set:{name:"Chris Sigera"}})
db.users.updateOne({name:"Anna"}, {$set:{name:"Chris Sigera"}})
```

## Update one using object_id
```js
db.users.updateOne({
    _id:ObjectId("5e88d735e76e345e3d9a76c9")
}, 
{
    $set:{
        "name":"Anna"
    }
})
```

## Adding elements to programming languages user Chris Sigera
```js
db.users.update(
    {
        "name": "Chris Sigera"
    },
    { $addToSet:
        {"programming_languages":
            {
                $each:["Java","ReactJS", "D3"]
            }
        }
    }
)         
```

## Remove last element
```js
db.users.update(
    {
        "name": "Chris Sigera"
    }, 
    {
        $pop: {
            "programming_languages" : 1 
        }
    }
)
```

## Remove first element
```js
db.users.update(
    {
        "name": "Chris Sigera"
    }, 
    {
        $pop: {
            "programming_languages" : -1 
        }
    }
)
```

## Add element
```js
db.users.update(
    {
        "name": "Chris Sigera" 
    },
    {
        $push:{
            "programming_languages": "JAVA" 
        }
    }
)
```

```js
db.users.update(
    {
        "name": "Chris Sigera" 
    },
    {
        $push:{
            "programming_languages": "Python" 
        }
    }
)
```

## Update single value in the array
```js

db.users.updateOne(
   { _id: ObjectId("5e89e5ade76e345e3d9a76dd"), programming_languages: "Python" },
   { $set: { "programming_languages.$" : "Python3" } }
)
```




## Update one $upset - remove fields from all or specific collection records
```js
db.users.updateOne({name:"Manuel"}, {$unset:{phone:""}})

db.users.find().pretty()

db.users.updateOne({name:"Manuel"}, {$set:{phone:"341871672"}})

db.users.updateOne({name:"Manuel"}, {$set:{has_hobbies:false}})
```

## Update many
```js
db.users.update({}, {$set: {"has_hobbies": true}}, false, true)
```

- criteria: query which selects the record to update;
- objNew: updated object or $ operators (e.g., $inc) which manipulate the object
- upsert: if this should be an “upsert” operation; that is, if the record(s) do not exist, insert one. Upsert only inserts a single document.
- multi: indicates if all documents matching criteria should be updated rather than just one. Can be useful with the $ operators below.

## Delete
```js
db.users.remove({ "_id": ObjectId("5e88de81e76e345e3d9a76d1")})
db.users.remove( { "age": null }, { $unset: { age: "" } } )
```

# MongoDB comparison operators
```js
use ga
db.createCollection("courses")
db.courses.insertMany([
    {
        "class": "Python",
        "title": "Intro to Python",
        "author": "Suresh",
        "duration": "12 weeks",
        "type": "paid",
        "fee": 8000,
        "number_of_students": 25
    },
    {
        "class": "ReactJS",
        "title": "Intro to ReactJS",
        "author": "Joe",
        "duration": "12 weeks",
        "type": "paid",
        "fee": 8000,
        "number_of_students": 35    
    },
    {
        "class": "D3",
        "title": "Intro to D3",
        "author": "Joe",
        "duration": "12 weeks",
        "type": "paid",
        "fee": 8000,
        "number_of_students": 15    
    },
    {
        "class": "Java",
        "title": "Intro to Java",
        "author": "Unknown",
        "duration": "15 weeks",
        "type": "paid",
        "fee": 9000,
        "number_of_students": 45    
    },
    {
        "class": ".NET",
        "title": "Intro to .NET",
        "author": "Guest",
        "duration": "18 weeks",
        "type": "paid",
        "fee": 9000,
        "number_of_students": 15    
    },
    {
        "class": "Data Science",
        "title": "Intro to Data Science",
        "author": "Suresh",
        "duration": "12 weeks",
        "type": "paid",
        "fee": 8000,
        "number_of_students": 25
    }
])

db.courses.find().pretty()

db.courses.find({"class":{$eq:"Java"}}).pretty();
db.courses.find({"author":{$eq:"Joe"}}).pretty();

db.courses.find({"author":{$ne:"Joe"}}).pretty();
db.courses.find({"class":{$ne:"Data Science"}}).pretty();

db.courses.find({"duration":{$in:["12 weeks","15 weeks"]}}).pretty();
db.courses.find({"number_of_students":{$in:[25,35]}}).pretty();
```
These MongoDB operators are used to perform logical operations on the given expressions.  
They help to make a decision based on multiple conditions. Each operand is considered a condition that can be evaluated 
to a true or false value. Then the value of the conditions is used to determine the overall value of all operators used 
as a group. Below are various logical operators in MongoDB –

```js
db.courses.find({
  $and:[{
    "author":"Joe"},
    {"fee":8000}
  ]
}).pretty();

db.courses.find({
  $or:[{
    "class":"Python"},
    {"class":"Java"}
  ]
}).pretty();

db.courses.find({
  $nor:[
    {"class":"Python"},
    {"author":"Suresh"}
  ]
}).pretty();

db.courses.find({
  "class":{
    $not:{
      $eq:"Python"
    }
  }
}).pretty();

db.courses.find({
  $nor:[
    {"class":"Python"},
    {"author":"Suresh"}
  ]
}).pretty();

db.courses.find({
  "fee":{
    $gt:8000
  }
}).pretty()

db.courses.find({
  "fee":{
    $gte:8000
  }
}).pretty()


db.courses.find({
  "fee":{
    $lt:8000
  }
}).pretty()

db.courses.find({
  "number_of_students":{
    $lt:20
  }
}).pretty()

db.courses.find({
  "fee":{
    $lte:8000
  }
}).pretty()

db.courses.find({
  $and:[
    {"author":"Joe"},
    {
      "number_of_students":{
        $eq: 35
      }
    }
  ]
}).pretty();
```
