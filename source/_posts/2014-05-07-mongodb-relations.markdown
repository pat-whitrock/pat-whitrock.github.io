---
layout: post
title: "MongoDB Relations"
subtitle: "Embedding and referencing documents"
date: 2014-05-07 01:10:36 -0400
comments: true
categories: 
---

Mongo’s relations are the NoSQL equivalent of associations found in relational databases. While serving a similar purpose and appearing similar in implementation, they do behave differently.

### NoSQL relations vs RDBMS associations

Relations associate data from one model with data from another model. On a high level, this is precisely what RDBMS associations are for. However, the way in which data is associated in NoSQL is quite different.

In traditional relational databases, associations are used to relate data from one table to another. MongoDB and similar NoSQL databases are document-oriented (there are no tables), so relationships are drawn between document-objects instead.
<!--more-->

#### ActiveRecord

```ruby
class Post < ActiveRecord::Base
  belongs_to :author
  has_many :comments
end
```

#### Mongoid (referencing)

```ruby
class Post
  include Mongoid::Document
  belongs_to :author
  has_many :comments
end
```

#### Mongoid (embedding)

```ruby
class Post
  include Mongoid::Document
  embedded_in :author
  embeds_many :comments
end
```

All of the classes above implement basic one-to-many relationships in which an author has many posts and a post has many comments. One could say that `embedded_in` and `embeds_many` are the equivalent of `belongs_to` and `has_many`, but this would be an oversimplification.

### ActiveRecord

Relational databases handle associations through the use of foreign keys referencing a unique row in another table. For example, a `post` would have an `author_id` referencing the ID of the record in the authors table to which it belongs. If an `author` has multiple `posts`, multiple post records would have the same `author_id`. Authors do not have `post_ids`.

![Relational database schema](https://d262ilb51hltx0.cloudfront.net/max/800/1*N--OFHhEC155ulYfHhPQGQ.png)

### Mongoid References

One way NoSQL databases handle one to many relationships is through references. Referencing is more similar to associations in relational databases than embedding (the pattern we will discuss next). References also utilize foreign keys, however they point from one document to another, rather than a record in one table to a record in another table.

```javascript
// An author document.
{
  "_id" : ObjectId("4d3ed089fb60ab534684b7e9"),
  "name" : "Pat Whitrock"
}

// A post document.
{
  "_id" : ObjectId("4d3ed089fb60ab534684b7e0"),
  "author_id" : ObjectId("4d3ed089fb60ab534684b7e9"),
  "name" : "Mongo Stuff"
}

// A comment document.
{
  "_id" : ObjectId("4d3ed089fb60ab534684b7e8"),
  "post_id" : ObjectId("4d3ed089fb60ab534684b7e0"),
  "content" : "Lots of stuff about Mongo."
}
```

### Mongoid Embeds

Another way NoSQL databases handle this sort of one to many relationship is through embedding documents. One document is embedded within another, resulting in what is basically a giant hash. For example, the author document would embed many post documents and each post document would embed many comment documents. Each author document is a hash containing an array of post documents, each containing an array of comment documents.

```javascript
// An author document.
{
  "_id" : ObjectId("4d3ed089fb60ab534684b7e9"),
  "name" : "Pat Whitrock",
  "posts" : [
    // An embedded post document.
    {
      "_id" : ObjectId("4d3ed089fb60ab534684b7e0"),
      "name" : "Mongo Stuff",
      "comments" : [
        // An embedded comment document.
        {
          "_id" : ObjectId("4d3ed089fb60ab534684b7e8"),
          "content" : "Lots of stuff about Mongo."
        }
      ]
    }
  ]
}
```

### Embedding vs Referencing

Why is it necessary for Mongo to have multiple ways of defining the same one to many relationship when ORMs like ActiveRecord require only one? Both embedding and referencing are valid options, but each caters more to specific use cases. There are a few things to consider before deciding.

Will your data be referred to from multiple locations? If you need to access your data from multiple locations, you’re probably better off using references. If your data is only useful in relation to its parent document, embedding is the way to go.

Also important to consider are data consistency and document size. MongoDB documents can are limited to a maximum size of 4MB, however it’s unlikely that this is a problem you would encounter early on.