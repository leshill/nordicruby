!SLIDE bullets incremental

# What is MongoDB?

* '...the best features of key/value stores, document databases and relational databases in one.' &#8212; @jnunemaker
* 'MongoDB is MySQL on meth' &#8212; @modetojoy

!SLIDE bullets incremental

# MongoDB is...

* documents
* dynamic queries
* indexes
* atomic operations
* map/reduce

!SLIDE bullets incremental

# Rich Documents

* json
* not relational
* not oodb

!SLIDE bullets incremental

# Schema-Free

* no constraints
* polymorphic fun!

!SLIDE bullets incremental

# Collections

* simple container

!SLIDE bullets incremental

# Querying

* rich set of operations
* reach into documents

!SLIDE bullets incremental

# Indexes

* primary index is the `ObjectID`
* secondary indexes

!SLIDE bullets incremental

# Atomic Operations

* partial updates
* no transactions

!SLIDE bullets

# Map/Reduce

* javascript on the server
* aggregation

<!-- !SLIDE bullets incremental -->

<!-- # Understanding MongoDB -->
<!-- # (sub-optimally!) -->

<!-- * A collection is like a table -->
<!-- * A document is like a row -->
<!-- * You can query like SQL `select where` -->

<!-- !SLIDE bullets incremental -->

<!-- # Avoid the relational crutch! -->

<!-- * A collection is not a table -->
<!-- * A document is not a row -->
<!-- * A query is not a SQL statement -->

!SLIDE bullets incremental

# MongoDB &#8212; What you need to know

* documents
* atomic updates
* indexing

!SLIDE bullets incremental

# Structuring your Documents

* embedding
* referencing another document
* optimize for queries and atomic updates

!SLIDE

# Embedding

    @@@ javascript
    > db.persons.findOne()

      { name: "Joe", address: { city: "San Francisco", state: "CA" } ,
        likes: [ 'scuba', 'math', 'literature' ] }

    > db.persons.find({ likes: 'math' })

      { name: "Joe", address: { city: "San Francisco", state: "CA" } ,
        likes: [ 'scuba', 'math', 'literature' ] }

!SLIDE smaller

# Complex Doc

    @@@ javascript
    > db.products.find({'_id': ObjectID("4bd87bd8277d094c458d2fa5")})

      {_id: ObjectID("4bd87bd8277d094c458d2a43"),
       title: "A Love Supreme [Original Recording Reissued]"
       author: "John Coltrane",
       author_id: ObjectID("4bd87bd8277d094c458d2fa5"),
       details: {
         number_of_discs: 1,
         label: "Impulse Records",
         issue_date: "December 9, 1964",
         average_customer_review: 4.95,
         asin: "B0000A118M" },
       pricing: {
        list: 1198,
        retail: 1099,
        savings: 99,
        pct_savings: 8 },
       categories: [
         ObjectID("4bd87bd8277d094c458d2a43"),
         ObjectID("4bd87bd8277d094c458d2b44"),
         ObjectID("4bd87bd8277d094c458d29a1")
       ] }

### from http://kylebanker.com/blog/2010/04/30/mongodb-and-ecommerce

!SLIDE bullets incremental

# Updating Atomically

* partial updates
* no transactions! oh noes!
* optimistic locking

!SLIDE

# `$addToSet`

    @@@ javascript
    > db.links.insert({url: 'http://blog.leshill.org',
      tags: ['ruby', 'rails']})
    > db.links.findOne()

      { "_id" : ObjectId("4bf6990341014702415c70d8"),
        "url" : "http://blog.leshill.org",
        "tags" : [
          "ruby",
          "rails" ]}

    > db.links.update({url : 'http://blog.leshill.org'}, {'$addToSet' :
      { tags : {'$each' : ['ruby', 'mongodb'] }}})
    > db.links.findOne()

      { "_id" : ObjectId("4bf6990341014702415c70d8"),
        "url" : "http://blog.leshill.org",
        "tags" : [
          "ruby",
          "rails",
          "mongodb" ]}

!SLIDE

# `find_and_modify`

    @@@ javascript
    > db.inventory.findOne()

      { "_id" : ObjectId("4bf6a04841014702415c70d9"),
        "sku" : "T-6732",
        "qty" : 1 }

    > db.inventory.findAndModify({ query : { sku : 'T-6732', qty : { '$gt' : 0 }},
      update : { '$inc' : { qty : -1 }}, new : 1})

      { "_id" : ObjectId("4bf6a04841014702415c70d9"),
        "sku" : "T-6732",
        "qty" : 0 }

    > db.inventory.findAndModify({ query : { sku : 'T-6732', qty : { '$gt' : 0 }},
      update : { '$inc' : { qty : -1 }}, new : 1})

      {}

!SLIDE bullets incremental

# Indexing

* compound indexes
* indexing options
* only one index is used per query

!SLIDE

# Indexing

    @@@ javascript
    > db.people.ensureIndex({ last_name: 1, first_name: 1 })
    > db.people.find({ last_name : 'Skywalker' }).explain()

      {
        "cursor" : "BtreeCursor last_name_1_first_name_1",
        ...

    > db.people.getIndexNames()

      [{ "name" : "_id_",
      "ns" : "mongoid_test.people",
      "key" : { "_id" : 1 }
      },
      { "_id" : ObjectId("4bf687b80a01c05d77662fc9"),
      "ns" : "mongoid_test.people",
      "key" : {
        "last_name" : 1,
        "first_name" : 1 },
      "name" : "last_name_1_first_name_1" }]

    > db.people.find({ last_name : 'Skywalker' }).hint("_id_")

!SLIDE

# Profiling

    @@@javascript
    > db.setProfilingLevel(2)
    > db.system.profile.find().sort({ $natural : -1 })

      { "ts" : "Fri May 21 2010 17:34:59 GMT+0200 (CEST)",
        "info" : "query mongoid_test.links
                  ntoreturn:1
                  reslen:140
                  nscanned:1
                  query: {}
                  nreturned:1
                  bytes:124",
        "millis" : 0 }

!SLIDE bullets incremental

# When not to use MongoDB?

* never? always use it!

!SLIDE bullets incremental

# When to use MongoDB

* non-relational data
* scaling and performance
