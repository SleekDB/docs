<!--METADATA
{
    "title": "Features",
    "url": "features",
    "icon": "filing"
}
!METADATA-->

# Features

## Installing SleekDB

- # Light-weight, faster

  Stores data in plain-text utilizing JSON format, no binary conversion needed to store or fetch the data.

- # Schema free data storage

  SleekDB does not require any schema meaning you can insert any types of data you want.

- # Query on nested properties

  Supports filter and conditions on nested properties of the JSON documents!
  If you write this where clause:

  ```php
  where( 'post.author.role' '=', 'admin' )
  ```

  SleekDB will look for data at:

  ```json
  {
    "post": {
      "author": {
        "role": "admin"
      }
    }
  }
  ```

- # Dependency free, only need PHP to run

  Supports PHP 5.5+, PHP 7+. Requires no third-party plugins or softwares.

- # Default caching layer

  SleekDB will serve data from cache by default and regenerate cache each time it creates, updates and deletes an object!

- # Filters: where, in, notIn, orWhere, sort, skip, limit and search

  Use multiple conditional comparisons, text search, sorting on multiple properties and nested properties.

- # Runs every where

  Runs perfectly on shared-servers or VPS too.

- # Easy to learn and implement

  SleekDB provides a very simple elegant API to handle all of your data.

- # Actively maintained

  SleekDB is actively maintained by <a href="https://twitter.com/rakibtg" target="_blank">@rakibtg</a>. For support and other requirements <a class="gotoblock" href="#contact">contact</a>.

- Easily import/export or backup data.
