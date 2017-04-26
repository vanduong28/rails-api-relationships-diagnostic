# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
    Join table allows you to model a many to many relationship so that a FK
    will only have one value stored in it rather than an array of values.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    Profile
      id SERIAL PRIMARY KEY,
      given_name TEXT,
      surname TEXT,
      email TEXT
    Movie
      id SERIAL PRIMARY KEY,
      title TEXT
      release_date DATE,
      length INTEGER
    Favorite
      id SERIAL PRIMARY KEY
      profile_id INTEGER REFERENCES profiles(id),
      movie_id INTEGER REFERENCES movies(id)

  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    has_many :movies, through: :favorites
    has_many :favorites
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    has_many :profiles, through: :favorites
    has_many :favorites
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to :profile, inverse_of: :favorites
    belongs_to :movie, inverse_of: :favorites
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
  A serializer can act as a filter and allows us to define what info we want to
  see.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, :movies
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
  bin/rails generate scaffold favorites movie:references profile:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    dependent: destroy is a method that will delete any associations if a
    resource is deleted. For example if a profile is deleted then its
    association with movies and favorites will be deleted. You would use this
    in the class definition of tables that are not join tables - in this example
    it woudl be in the Movies and Profiles.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
  In a medical clinic a patient could have many different doctors and a doctor can
  have many patients thereby making a many to many relationship. However,
  a patient should only have one primary care physician while the primary care
  physician will have many patients, thereby making a one to many relationship.
  ```
