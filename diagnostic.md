# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

```sh
  Join tables allow us to reuse existing data columns to make comparisons between an arbitrary number of columns.  Without them, if we wanted a comparison, we would have to make an entirely new table for each comparison you wanted to do.  This would be cumbersome and duplicative.
```

1.  Provide a database table structure and explain the Entity Relationship that
describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
(Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
`Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
join table with references to `Movies` and `Profiles`.

```sh
  Profile:
  - given_name | text
  - surname    | text
  - email      | text

  Movie:
  - title        | text
  - release_date | date
  - length       | integer

  Favorites:
  - Movie_id   | integer
  - Profile_id | integer

  A Profile has many Movies through Favorites.
  A Movie has many Profiles through Favorites.
  
  
```

1.  For the above example, what needs to be added to the Model files?

```rb
class Profile < ActiveRecord::Base
  has_many :favorites
  has_many :movies, through: :favorites
end
```

```rb
class Movie < ActiveRecord::Base
  has_many :favorites
  has_many :profiles, through: :favorites
end
```

```rb
class Favorite < ActiveRecord::Base
  belongs_to :profile
  belongs_to :movie
end
```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

```sh
A serializer takes our model and uses it to render JSON to be sent to the client
```

```rb
class ProfileSerializer < ActiveModel::Serializer
  attributes :favorites
end
```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

```sh
  rails g migration create_favorites profile_id:integer movie_id:integer
```
^Reference(http://stackoverflow.com/questions/26865865/scaffolding-table-obtained-by-many-to-many-relationship)

1.  What is `Dependent: Destroy` and where/why would we use it?

```sh
  We would use Dependent: Destroy in our models to tell the database to make an association between two columns such that if one is destroyed, its dependents should be destroyed as well.
  
```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

```sh
  A Setlist belongs to Tunes
  A Setlist belongs to Musicians
  A Musician has many Setlists
  A Tune has many Setlists
  
  A Musician has many Tunes, through Setlists
  A Tune has many Musicians, through Setlists
    
  (individual tunes, not organized with a setlist)
  A Tune has many Musicians
  A Musician belongs to a Tune
  
```
