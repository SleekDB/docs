<!--METADATA
{
    "title": "Join Stores",
    "url": "join-stores",
    "icon": "git-merge"
}
!METADATA-->

# Join Stores

With SleekDB it is easy to join multiple stores. You can add more than one `join` as well as nested `join` methods are also supported!

Example:

```php
$userStore
  ->getQueryBuilder()
  ->join(function($user) use ($commentsStore) {
    return $commentsStore
      ->getQueryBuilder()
      ->where("user", "=", $user["_id"]);
  }, "comments")
  ->getQuery()
  ->fetch();
```

The above command would query into the "users" store to fetch all the data.
Each user also has an additional property called "comments", that contains all comments of the user.

## Apply Filters and Conditions

### join()

To join stores we use the join() method of the QueryBuilder object.

The join() method takes two arguments, those are:

```php
join(callable $joinFunction, string $dataPropertyName): QueryBuilder
```

1. # $joinFunction

   This function has to return the result of an executed sub query or prepares a sub query for the join and returns it as a QueryBuilder object.

2. # $dataPropertyName

   Name of the new property added to each document.

### Example of using join() to join stores

To get the users with their comments we would join like this:

```php
$userStore = new \SleekDB\Store("users", $dataDir);
$commentsStore = new \SleekDB\Store("comments", $dataDir);

$users = $userStore
  ->getQueryBuilder()
  ->join(function($user) use ($commentsStore){
    return $commentsStore
      ->getQueryBuilder()
      ->where("userId", "=", $user["_id"]);
  }, "comments")
  ->getQuery()
  ->fetch();
```

You can use multiple `join()`.

Example:

```php
$userStore = new \SleekDB\Store("users", $dataDir);
$commentsStore = new \SleekDB\Store("comments", $dataDir);
$articlesStore = new \SleekDB\Store("articles", $dataDir);

$users = $userStore
  ->getQueryBuilder()
  ->join(function($user) use ($commentsStore) {
    return $commentsStore
      ->getQueryBuilder()
      ->where("userId", "=", $user["_id"]);
  }, "comments")
  ->join(function($user) use ($articleStore) {
    return $articleStore
      ->getQueryBuilder()
      ->where("author", "=", $user["_id"]);
  }, "articles")
  ->getQuery()
  ->fetch();
```

You can use `join()` within a join sub query.

Example:

```php
$userStore = new \SleekDB\Store("users", $dataDir);
$commentsStore = new \SleekDB\Store("comments", $dataDir);
$articlesStore = new \SleekDB\Store("articles", $dataDir);

$users = $userStore
  ->getQueryBuilder()
  ->join(function($user) use ($articleStore){
    return $articleStore
      ->getQueryBuilder()
      ->where("author", "=", $user["_id"])
      ->join(function($article) use ($commentsStore){
        return $commentsStore
          ->getQueryBuilder()
          ->where("articleId", "=", $article["_id"]);
      }, "comments");
  }, "articles")
  ->getQuery()
  ->fetch();
```
