# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
    Join tables are valuable because they allow you to relate two tables together such as a customer table related to a tee time table at a golf course. Join tables also allow you to conveniently reference one table from another using ids.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    client -> server -> router -> controller -> model -> database
                                  controller -> views
    |Profiles|-|--<|Favorites|>--|-|Movies|
    profiles have many movies through favorites
    movies belong to many profiles through favorites
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    has_many :favorites
    has_many :movies, through :favorites
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    has_many :favorites
    has_many :profiles, through :favorites
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

  ```md
    serializers are responsible for formatting the JSON response that gets returned to us when we make an HTTP request
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :given_name, :surname, :email
    def movies
      object.movies.all
    end
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    bin/rails generate scaffold favorites movie_id:references profile_id:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    Dependent: Destroy is a command that will delete a particular object and all objects associated with it. This can make adding rows and deleting associated data much easier down the road.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    a one to many relationship would be having a dossier of children in an elementary school. one teacher has many children.
    a many to many relationship would be 
  ```
