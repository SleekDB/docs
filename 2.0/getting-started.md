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

> With this release of version 2.0 we have updated the internal implementation. We introduced new API and the old SleekDB object has to retire! Although, we are providing downwards compatibility in this version but it will be removed in version 3.0<br />This documentation contains examples using the new API.

## Query Life Cycle

Each query will follow the below steps or execution life cycle,

1. # Store

   While initializing the SleekDB object, at first it will initialize the store, using the `Store` class. It will handle new or existing store, validate the configurations and as well as the store permission.

2. # Query Builder

   The `QueryBuilder` class handles the creation of a query. The query will be prepared using the `getQuery` method.

3. # Query

   At this step the `Query` class will be instantiated and would get the query from the `QueryBuilder` in order to process the query. Then it will be passed to the caching layer if it's a fetch or select query.

4. # Cache

   The `Cache` class handles everything regarding caching. It will decide when to cache or not to cache against a query, deleting cache etc and then return the data.

## Insert and Fetch Data

1. To began with, it requires a valid "path" where it can write data or find existing data. Absolute or relative path both are supported.

   ```php
   $dataDir = __DIR__ . "/mydb";
   ```

2. Once we have the data directory, now we can initialize the store. If the store doesn't exist then it will be created automatically.

   ```php
   $newsStore = new \SleekDB\Store("news", $dataDir);
   ```

   Optionally you can pass a configuration array in the third parameter. Read more about <a class="gotoblock" href="/#/configurations">configurations</a>.

3. Inserting your first news article using the `insert` method. The insertable item should be an array that will be written in a JSON file.

   ```php
   $newsInsertable = [
     "title" => "Google Pixel XL",
     "about" => "Google announced a new Pixel!",
     "author" => [
       "avatar": "profile-12.jpg",
       "name" : "Foo Bar"
     ]
   ];
   $results = $newsStore->insert($newsInsertable);
   ```

   The results variable would contain all the inserted data and with the `_id` property which will be unique and added automatically.

4. To find all news we will use the `fetch` method,

   ```php
   $allNews = $newsStore->createQueryBuilder()->getQuery()->fetch();

   print_r($allNews);
   ```

> 🎉 Congrats! You just inserted your first data into a SleekDB store and also learned how to fetch them.
