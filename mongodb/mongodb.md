!SLIDE bullets incremental

# What is MongoDB?

* richer than a key-value store
* horizontally scalable
* flexible and dynamic

!SLIDE

# MongoDB combines...

> '...the best features of key/value stores, document databases and relational
> databases in one.'
> - @jnunemaker

!SLIDE bullets incremental

# MongoDB is...

* documents
* collections
* dynamic queries
* indexes
* atomic operations
* map-reduce

!SLIDE bullets incremental

# ...Rich Documents

* JSON
* not relational
* not OODB

!SLIDE bullets incremental

# ...that are Schemaless

* no constraints
* flexible evolution
* polymorphic fun!

!SLIDE bullets incremental

# ...Organized into Collections

* just a container
* queryable
* indexed

!SLIDE bullets

# ...that can be Queried

* rich set of query operations
* reach into documents
* schemaless!

!SLIDE bullets

# ...using Indexes

* primary index is the `ObjectID`
* secondary indexes
* compound indexes

!SLIDE bullets

# ...and updated with Atomic Operations

* no transactions
* partial updates
* 'update if current'

!SLIDE bullets

# ...and supporting Map/Reduce

* JavaScript on the server
* grouping
* permanent or temporary collections

!SLIDE bullets incremental

# Understanding MongoDB
# (sub-optimally!)

* A collection is like a table
* A document is like a row
* You can query like SQL `select where`

!SLIDE bullets incremental

# Avoid the relational crutch!

* A collection is not a table
* A document is not a row
* A query is not a SQL statement

!SLIDE bullets incremental

# Understanding MongoDB (optimally!)

* Documents
* Indexing
* Atomic Updates

!SLIDE bullets

# Structuring your Documents

* embedding
* deep queries
* referencing another document
* optimize for queries and atomic updates

!SLIDE

# Tags

    @@@ javascript
    db.persons.findOne()

    { name: "Joe", address: { city: "San Francisco", state: "CA" } ,
      likes: [ 'scuba', 'math', 'literature' ] }

    db.persons.find({ likes: 'math' })

!SLIDE

# Catalog

    @@@ javascript
    db.products.find({'_id': ObjectID("4bd87bd8277d094c458d2fa5")});
    {_id: ObjectID("4bd87bd8277d094c458d2a43"),
     title: "A Love Supreme [Original Recording Reissued]"
     author: "John Coltrane",
     author_id: ObjectID("4bd87bd8277d094c458d2fa5"),
     details: {
       number_of_discs: 1,
       label: "Impulse Records",
       issue_date: "December 9, 1964",
       average_customer_review: 4.95,
       asin: "B0000A118M"
     },
     pricing: {
      list: 1198,
      retail: 1099,
      savings: 99,
      pct_savings: 8
     },
     categories: [
       ObjectID("4bd87bd8277d094c458d2a43"),
       ObjectID("4bd87bd8277d094c458d2b44"),
       ObjectID("4bd87bd8277d094c458d29a1")
     ]
    }

!SLIDE bullets incremental

# Indexing

* only one index is used per query
* compound indexes
* indexing options

!SLIDE

# Explain

    @@@ javascript
    db.people.ensureIndex({last_name: 1, first_name: 1})
    db.people.find({last_name : 'Skywalker'}).explain()
    {
      "cursor" : "BtreeCursor last_name_1_first_name_1",
      ...

# Profile

    @@@javascript
    db.setProfilingLevel(1); // Log ops over 100 ms
    db.system.profile.find().sort({ $natural : -1 }); // Most recent info first.

!SLIDE bullets incremental

#
