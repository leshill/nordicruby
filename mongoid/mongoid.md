!SLIDE

# Mongoid

!SLIDE bullets

# Why Mongoid?

* Rails 3 Compatible
* Atomic Operations by Default
* Handles Large Data Sets Easily
* Badass Criteria API

!SLIDE bullets

# Rails 3 Compatibility

* Gemfile
* Generate Configuration
* Modify application.rb
* Generate Models

!SLIDE

# Modify Your Gemfile

    @@@ ruby
    gem "mongoid", "2.0.0.beta6"
    gem "bson_ext", "1.0.1"

!SLIDE

# Generate Your Configuration

    @@@ ruby
    rails generate mongoid:config

!SLIDE

# Modify Your application.rb

    @@@ ruby
    config.generators do |g|
      g.orm :mongoid
    end

!SLIDE

# Generate Models

    @@@ ruby
    rails generate model person dob:Date name:String --timestamps

!SLIDE bullets

# Atomic Operations by Default

* $set
* $push
* $unset
* $pull

!SLIDE

# Save new embeds_one on existing parent

    @@@ruby
    person = Person.where(:first_name => "Greedo").first
    email = Email.new(:address => "greedo@tt.net")
    person.email = email
    email.save

    MongoDB Query:
    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { "$set" : { "email" : { "address" : "greedo@tt.net" } } },
      false,
      true
    );

!SLIDE

# Save new embeds_many on existing parent

    @@@ruby
    person = Person.where(:first_name => "Ponda").first
    address = Address.new(:street => "Outer Kerner Way")
    person.addresses << address
    address.save

    MongoDB Query:
    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { "$push" : { "addresses" : { "street" : "Outer Kerner Way" } } },
      false,
      true
    );

!SLIDE

# Updating existing embeds_one

    @@@ruby
    person = Person.where(:first_name => "Jabba").first
    email = person.email
    email.address = "jabba@tt.org"
    email.save

    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { "$set" : { "email.address" : "jabba@tt.org" } },
      false,
      true
    );

!SLIDE

# Updating an existing embeds_many

    @@@ruby
    person = Person.where(:first_name => "Luke").first
    address = person.addresses.first
    address.street = "Galactic Way"
    address.save

    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { "$set" : { "addresses.0.street" : "Galactic Way" } },
      false,
      true
    );

!SLIDE

# Deleting an existing embeds_one

    @@@ruby
    person = Person.where(:first_name => "Boba").first
    email = person.email
    email.delete # or destroy

    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { "$unset" : { "email" : true } },
      false,
      true
    );


!SLIDE

# Deleting an existing embeds_many

    @@@ruby
    person = Person.where(:first_name => "Boba").first
    address = person.addresses.first
    address.delete # or destroy

    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { "$pull" : { "addresses" : { "_id" : "4baa56f1230048567300485d" } } },
      false,
      true
    );

!SLIDE bullets

# Large Data Sets and Performance

!SLIDE bullets

# Mongoid

* Saving 10k New Documents                    21.35
* Querying & Iterating 10k Documents           2.32
* Updating The Root Document 10k Times         3.20
* Updating An Embedded Document 10k Times      3.34
* Appending A New Embedded Document 10k Times  7.51

!SLIDE bullets

# ActiveRecord

* Saving 10k New Objects                                  84.35
* Querying & Iterating 10k Documents (With Eager Loading)  7.70
* Updating The Parent Object 10k Times                    15.93
* Updating An Association 10k Times                       14.34
* Appending A New Association 10k Times                   61.71

!SLIDE bullets

# Criteria API

* Like ARel
* Named scopes
* Class methods
* Chainable

!SLIDE

# Criteria Basics

    @@@ruby
    Person.where(:first_name => "Jabba")
    Person.where(:first_name.in => [ "Jabba", "Boba" ])
    Person.any_in(:first_name => [ "Han", "Lea" ])

!SLIDE

# Named Scopes

    @@@ruby
    class Person
      include Mongoid::Document
      field :name
      scope :jabba, where(:name => "Jabba")
    end

    Person.jabba

!SLIDE

# Class Methods

    @@@ruby
    class Person
      include Mongoid::Document
      field :name
      def self.jabba
        where(:name => "Jabba")
      end
    end

    Person.jabba

!SLIDE

# Chaining

    @@@ruby
    Person.only(:first_name, :last_name)
          .where(:first_name.in => [ "Jabba", "Boba" ])
          .order_by([[ :first_name, Mongo::ASCENDING ]])
