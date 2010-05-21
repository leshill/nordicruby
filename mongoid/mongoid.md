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
