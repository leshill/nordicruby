!SLIDE bullets incremental

# Mongoid

* Object Document Mapper
* Store Ruby objects in MongoDB
* And query them easily

!SLIDE bullets

# Mongoid

* Durran Jordan
* hashrocket.com
* @modetojoy
* github.com/durran/mongoid

!SLIDE

# Inserting a new document

    @@@ ruby
    jake = Person.new(:last_name => "Sully")
    jake.save

    # MongoDB query:
    insert([{"created_at"=>Fri Apr 30 15:05:40 UTC 2010,
      "updated_at"=>Fri Apr 30 15:05:40 UTC 2010,
      "_id"=>"4bdaf1c42557ab099f00000b",
      "_type"=>"Person",
      "clearance_level"=>0,
      "last_name"=>"Sully"}])
