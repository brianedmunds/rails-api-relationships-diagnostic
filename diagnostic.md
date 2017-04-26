# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
Join table are valuable when you have two sets of data that you want to be able to connect to each other.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
There would be three tables, with the following attributes.  Favorites would be a join table.

1. Profiles
     - id
     - given_name
     - surname
     - email
2. Movies
     - id
     - title
     - release_date
     - length
3. Favorites
     - profile_id
     - movie_id
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base

  has_many :movies, through :favorites
  has_many :favorites

  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
  has_many :profiles, through :favorites
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
    The serializer acts like a filter and allows you to create custom JSON responses.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, given_name,
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
  bin/rails generate scaffold favorites profile:references movie:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
   This is used when you have an association and you want to ensure that when one record gets deleted, all other dependent records get deleted as well.  For example, if you have an author with many books, and you delete that author, you may want all books written by that author to also be deleted.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    Let's say you have four tables: Author, Book, Loan, Borrower

    You could have a one-to-many relationshipn between Author and Book.  You could then have a many-to-many relationship between Book and Borrower through the Loan join table.
  ```
