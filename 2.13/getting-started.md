<!--METADATA
{
    "title": "Getting Started",
    "url": "getting-started",
    "icon": "rocket"
}
!METADATA-->

# Getting Started

Getting started with SleekDB is super easy. We keep data in a "store", which is similar to MySQL "table" or MongoDB "collection". Each store contains multiple documents, which are equivalent to rows in SQL based relational database.

> As an example for a blog it can be something like this: **posts** > **documents**

Here "posts" is a store and "documents" are a collection of JSON files that contains all the blog posts.

## 📌 Important Note

> With the release of version 2.0 we have updated the internal implementation.<br/>We introduce a new API and the old SleekDB object has to retire!<br/>Although, we are providing downwards compatibility in this version, but will remove the old SleekDB object with version 3.0.<br />This documentation focus on the new API.

## Query Life Cycle

Each query will follow the below steps or execution life cycle.

1. # Store

   The `Store` class is the first, and in most cases also the only part you need to come in contact with.

2. # Query Builder

   The `QueryBuilder` class is used to prepare a query. To execute it `getQuery` method can be used.

3. # Query

   At this step the `Query` class contains all information needed to execute the query.

4. # Cache

   The `Cache` class handles everything regarding caching. It will decide when to cache or not to cache.

## First Example - Insert and Fetch Data

1. To begin with, we need a valid "path" where we want to store our data.<br/>Both absolute and relative paths are supported.

   ```php
   $databaseDirectory = __DIR__ . "/myDatabase";
   ```

2. Once we have the data directory, we can initialize the store.<br/>If the store doesn't exist it will be created automatically.

   ```php
   $newsStore = new \SleekDB\Store("news", $databaseDirectory);
   ```

   Optionally you can **pass a configuration array as a third parameter**.<br/>
   Read more about <a class="gotoblock" href="/#/configurations">configurations</a>.

3. Inserting your first news article using the `insert` method. The article has to be an array.

   ```php
   $article = [
     "title" => "Google Pixel XL",
     "about" => "Google announced a new Pixel!",
     "author" => [
       "avatar" => "profile-12.jpg",
       "name" => "Foo Bar"
     ]
   ];
   $results = $newsStore->insert($article);
   ```

   The results variable will contain all the inserted data and with their `_id` property which will be unique and added automatically.

4. To find all news articles we will use the `findAll` method.

   ```php
   $allNews = $newsStore->findAll();

   print_r($allNews);
   ```

> 🎉 Congrats! You just inserted your first data into a SleekDB store and also learned how to fetch them.
