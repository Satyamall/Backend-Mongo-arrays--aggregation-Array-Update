
# Backend Mongo- Arrays update queries, Aggregation Pipelines -> Array-Update


# use the dbs array for collection posts
- use array

# Create a collection of posts

 - db.posts.insertMany([
    {
        "title": "Masai 101",
        "author_id": "1",
        "author": "Satya",
        "comments": [
            {
                "body": "Hello",
                "author": "Sid",
                "author_id": "2"
            }
        ]
    },
    {
        "title": "Masai 102",
        "author_id": "3",
        "author": "Jaswant",
        "comments": [
            {
                "body": "Hello",
                "author": "Satya",
                "author_id": "1"
            }
        ]
    }
])

 - db.posts.find()
[
  {
    _id: ObjectId("62091a592c0260bb8ee2e076"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [ { body: 'Hello', author: 'Sid', author_id: '2' } ]
  },
  {
    _id: ObjectId("62091a592c0260bb8ee2e077"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ]
  }
]

**Below Picture for above querry**
![Screenshot (254)](https://user-images.githubusercontent.com/80479635/153762071-18994ca1-7c9d-4b29-ac7a-ea3ece85b6a7.png)


# 1. push a new tag into a post.

 - db.posts.updateOne({},{$push: {tags: ["Programming"]}})

 - db.posts.updateOne({_id: ObjectId("62091a592c0260bb8ee2e077")},{$push: {tags: {$each:["Programming"]}}})

 - db.posts.find()

**Output->**
[
  {
    _id: ObjectId("62091a592c0260bb8ee2e076"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [ { body: 'Hello', author: 'Sid', author_id: '2' } ],
    tags: [ [ 'Programming' ] ]
  },
  {
    _id: ObjectId("62091a592c0260bb8ee2e077"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ 'Programming' ]
  }
]

**Below picture for above**
![Screenshot (255)](https://user-images.githubusercontent.com/80479635/153762081-cad4dd2c-707e-4587-af37-efaa10e3c969.png)



# 2. push multiple tags into a post.

 - db.posts.updateMany({},{$push: {tags: {$each:["Coding","FSWD"]}}})

**Output->**
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 2,
  modifiedCount: 2,
  upsertedCount: 0
}

 - db.posts.find()

**Output->**
[
  {
    _id: ObjectId("62091a592c0260bb8ee2e076"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [ { body: 'Hello', author: 'Sid', author_id: '2' } ],
    tags: [ [ 'Programming' ], 'Coding', 'FSWD' ]
  },
  {
    _id: ObjectId("62091a592c0260bb8ee2e077"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ 'Programming', 'Coding', 'FSWD' ]
  }
]

**Below Picture show above querry**
![Screenshot (256)](https://user-images.githubusercontent.com/80479635/153762088-a9915413-a3cb-47da-aa4e-49a954a51f84.png)


# 3. pull and remove a particular tag.

- db.posts.updateMany({title: "Masai 101"},{$pull: {tags: ["Programming"]}})

- db.posts.find()

**Output->**
[
  {
    _id: ObjectId("62091a592c0260bb8ee2e076"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [ { body: 'Hello', author: 'Sid', author_id: '2' } ],
    tags: [ 'Coding', 'FSWD' ]
  },
  {
    _id: ObjectId("62091a592c0260bb8ee2e077"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ 'Programming', 'Coding', 'FSWD' ]
  }
]

**Below picture for above**
![Screenshot (257)](https://user-images.githubusercontent.com/80479635/153762098-422eace8-96fe-4f99-8861-77b5e2c8b61f.png)


# 4. pull and remove an array of values use $in.

- db.posts.updateMany({},{$pull: {tags: {$in: ["Programming"]}}})

- db.posts.find()

**Output->**
[
  {
    _id: ObjectId("62091a592c0260bb8ee2e076"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [ { body: 'Hello', author: 'Sid', author_id: '2' } ],
    tags: [ 'Coding', 'FSWD' ]
  },
  {
    _id: ObjectId("62091a592c0260bb8ee2e077"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ 'Coding', 'FSWD' ]
  }
]

**Below picture for above querry**
![Screenshot (258)](https://user-images.githubusercontent.com/80479635/153762110-af16e469-5ded-4882-abdd-41d14b2c4e37.png)



# 5. use $pop to remove first and last tag.

**$pop from last**
-  db.posts.updateMany({},{$pop: {tags: 1}})

- db.posts.find()

**Output->**
[
  {
    _id: ObjectId("62091a592c0260bb8ee2e076"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [ { body: 'Hello', author: 'Sid', author_id: '2' } ],
    tags: [ 'Coding' ]
  },
  {
    _id: ObjectId("62091a592c0260bb8ee2e077"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ 'Coding' ]
  }
]


**$pop from front**
- db.posts.updateMany({},{$pop: {tags: -1}})

- db.posts.find()

**Output->**
[
  {
    _id: ObjectId("62091a592c0260bb8ee2e076"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [ { body: 'Hello', author: 'Sid', author_id: '2' } ],
    tags: [ 'Programming' ]
  },
  {
    _id: ObjectId("62091a592c0260bb8ee2e077"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ 'Programming' ]
  }
]

**Below picture for above**
![Screenshot (259)](https://user-images.githubusercontent.com/80479635/153762123-87066cbd-7119-4c5b-af9b-be8f11b0195a.png)
![Screenshot (260)](https://user-images.githubusercontent.com/80479635/153762138-f3e2423c-dc74-48e3-835c-028bb096fd61.png)


# 6. use addToSet to add.5 tags.

- db.posts.updateMany({},{$addToSet: {tags: {$each: ["DSA","Coding","React","JavaScript","Programming"]}}})

- db.posts.find()

**Output->**
[
  {
    _id: ObjectId("62091a592c0260bb8ee2e076"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [ { body: 'Hello', author: 'Sid', author_id: '2' } ],
    tags: [ 'Programming', 'DSA', 'Coding', 'React', 'JavaScript' ]
  },
  {
    _id: ObjectId("62091a592c0260bb8ee2e077"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ 'Programming', 'DSA', 'Coding', 'React', 'JavaScript' ]
  }
]

**Below picture show above**
![Screenshot (261)](https://user-images.githubusercontent.com/80479635/153762145-bf3ebb75-a6dd-459c-93d4-4c4384f0cb22.png)



# Part - II

# 7. create a collection of users with high scores.

- db.users.insertMany([ { name: "Satya", scores: [96,94,70] }])



# 8. each user can have 3 high scores.

- db.users.find()

**Output->**
[
  {
    _id: ObjectId("6209284c2c0260bb8ee2e078"),
    name: 'Satya',
    scores: [ 96, 94, 70 ]
  }
]


# 9. it should be always in descending order.

- db.users.find({},{scores: 1})

**Output->**
[
  { _id: ObjectId("6209284c2c0260bb8ee2e078"), scores: [ 96, 94, 70 ] }
]


# 10. add 5 new high scores, and maintain the order, and keep the limit of 3 scores.

- db.users.updateMany({}, { $push: { scores: {$each: [67,64,35, 100, 45], $sort: -1 , $slice: 3 }} })

- db.users.find()

**Output->**
[
  {
    _id: ObjectId("6209284c2c0260bb8ee2e078"),
    name: 'Satya',
    scores: [ 100, 100, 96 ]
  }
]


# 11. remove multiple scores which are greater than x value.

- db.users.updateMany({}, { $pull: { scores: {$gt: 96}}})

- db.users.find()

**Output->**
[
  {
    _id: ObjectId("6209284c2c0260bb8ee2e078"),
    name: 'Satya',
    scores: [ 96 ]
  }
]


**Below picture for above querrie from 7-11**
![Screenshot (263)](https://user-images.githubusercontent.com/80479635/153762166-28d2e12f-a8a0-43ba-a0c4-b9159b2de94e.png)
![Screenshot (264)](https://user-images.githubusercontent.com/80479635/153762169-dfc9339a-65f2-4ff9-adfb-25f36b50ee14.png)


