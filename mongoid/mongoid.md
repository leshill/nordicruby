!SLIDE smbullets incremental

# Mongoid

* rails 2 (1.9.x) and rails 3 (2.0.x) compatible
* atomic operations by default
* handles large data sets with ease
* badass criteria api
* lots of nice extras
* name is not offensive
* similar api to datamapper

!SLIDE

# Documents

    @@@ruby
    class User
      include Mongoid::Document
      field :dob, :type => Date
      embeds_one :name
      embeds_many :addresses

      referenced_in :organization
      references_many :posts
    end

    class Name
      include Mongoid::Document
      field :given
      field :family
      embedded_in :user,
        :inverse_of => :name
    end

    class Organization
      include Mongoid::Document
      field :name
      references_many :users
    end

!SLIDE

# Atomic Updates

## save new embeds_one

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

# Atomic Updates

## save new embeds_many

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

# Atomic Updates

## update existing embeds_one

    @@@ruby
    person = Person.where(:first_name => "Jabba").first
    email = person.email
    email.address = "jabba@tt.org"
    email.save

    MongoDB Query:
    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { "$set" : { "email.address" : "jabba@tt.org" } },
      false,
      true
    );

!SLIDE

# Atomic Updates

## updating existing embeds_many

    @@@ruby
    person = Person.where(:first_name => "Luke").first
    address = person.addresses.first
    address.street = "Galactic Way"
    address.save

    MongoDB Query:
    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { "$set" : { "addresses.0.street" : "Galactic Way" } },
      false,
      true
    );

!SLIDE

# Atomic Updates

## delete existing embeds_one

    @@@ruby
    person = Person.where(:first_name => "Boba").first
    email = person.email
    email.delete # or destroy

    MongoDB Query:
    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { "$unset" : { "email" : true } },
      false,
      true
    );


!SLIDE

# Atomic Updates

## delete existing embeds_many

    @@@ruby
    person = Person.where(:first_name => "Boba").first
    address = person.addresses.first
    address.delete # or destroy

    MongoDB Query:
    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { "$pull" : { "addresses" : { "_id" : "4baa56f1230048567300485d" } } },
      false,
      true
    );

!SLIDE

# Criteria

    @@@ruby
    Person.where(:first_name => "Jabba")
    Person.where(:first_name.in => [ "Jabba", "Boba" ])
    Person.any_in(:first_name => [ "Han", "Lea" ])

    class User
      include Mongoid::Document
      field :name
      field :bounties, :type => Integer
      scope :jabba, where(:name => “Jabba”)
      scope :kills_over, lambda { |num| where(:bounties.gt => num) }
    end

    class User
      include Mongoid::Document
      field :name
      class << self
        def jabba
          where(:name => “Jabba”)
        end
      end
    end

!SLIDE

# Chaining Criteria

    @@@ruby
    User.kills_over(100000)
      .only(:first_name, :last_name)
      .where(:first_name.in => [ "Jabba", "Boba" ])
      .order_by([[ :first_name, Mongo::ASCENDING ]])
      .skip(10)
      .limit(25)

## criteria are lazily evaluated

!SLIDE

# Nested Criteria

    @@@ruby
    person = Person.where(:name => “Luke”)
    person.addresses
          .tattoine
          .skip(10)
          .limit(20)

!SLIDE smbullets incremental

# Extras

* timestamping
* versioning
* master/slave connection support
* validations, dirty attributes, callbacks a la activemodel and activesupport
* rails 3 config and model generator
* indexing

