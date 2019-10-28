# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version
Installation:

First, we need to install rvm (ruby version manager)
```
gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
\curl -sSL https://get.rvm.io | bash -s stable
```

Next, we install the version of ruby that we want:
```
rvm install ruby-2.6.3
```

Next, set the default ruby version to that ruby installation:
```
rvm --default use 2.6.3
```

Next, install rails:
```
gem install rails -v 5.2.3
```

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...

## Data Model Notes To Change
* Add salt to user table (for password)
* Add email to user table (will use as username)
* Add link for profile picture to user table (I think this is the best way to store it, dump the image in a bucket somewhere, save the link to it in the database. We will need to do this for recipes since you need to have preview images for a recipe so may as well do it here too)
* New Table: RecipeImages
  * id (PK)
  * recipeId (FK)
  * fullResolutionUrl (string)
  * minifiedUrl (string)
* Add column to recipes for bannerImage (optional, FK to recipeImages)
* Add column to recipes for previewImage (do we make this mandatory or optional? FK to RecipeImages)