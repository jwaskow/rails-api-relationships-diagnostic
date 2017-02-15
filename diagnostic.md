# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
A join table is useful when you have a many to many relationship.  The join table references id's in the other two tables.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
A profile has a list of favorites.  A profile has many movies through the list of favorites.  The same movie can be on many profile's favorite lists, so movies have many profiles through favorites.
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
  has_many :movies, through :favorites
  has_many :favorites
  validates :given_name, presence: true
  validates :surname, presence: true
  validates :email, presence: true
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
  has_many :profiles, through :favorites
  has_many :favorites
  validates :title, presence: true
  validates :release_date, presence: true
  validates :length, presence: true
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
  belongs_to :profile
  belongs_to :movie
  validates :profile, presence: true
  validates :movie, presence: true
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
A serializer takes the data from the request and converts it to JSON as a response.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
  attributes :given_name, :surname, :email, :movies

  def movies
    object.movies.pluck(1)
  end
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
bin/rails destroy scaffold Favorites
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
dependent: destroy is attached to an object that has many objects.  When the parent object is destroyed, all the related objects are destroyed as well.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    # < Your Response Here >
  ```
