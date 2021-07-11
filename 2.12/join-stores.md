<!--METADATA
{
    "title": "Join Stores",
    "url": "join-stores",
    "icon": "git-merge"
}
!METADATA-->

# Join Stores

With SleekDB it is easy to join multiple stores. You can add more than one `join` as well as nested `join` methods are also supported!

### Quick Example

Query into the "users" store to fetch all users.
Each user also will get an additional property called "comments", that contains all comments of the user.

```php
$usersWithComments = $userStore
  ->createQueryBuilder()
  ->join(function($user) use ($commentStore) {
    return $commentStore->findBy(["user", "=", $user["_id"]]);
  }, "comments")
  ->getQuery()
  ->fetch();
```
#### Result
```
[
  [
    "_id" => 1, 
    "name" => "John", 
    "comments" => [
      [
        "_id" => 1,
        "articleId" => 3
        "content" => "I love SleekDB"
      ],
      ...
    ]
  ],
  ...
]
```

## join()

To join stores we use the join() method of the QueryBuilder object.

The join() method takes two arguments, those are:

```php
function join(Closure $joinFunction, string $propertyName): QueryBuilder
```

### Parameters

1. # $joinFunction: Closure

   This anonymous function has to return the `result of an executed sub query` or prepare a sub query for the join and return it as a `QueryBuilder object`.

2. # $propertyName: string

   Name of the new property added to each document.

### Example 1

To get the users with their comments we would join like this:

```php
use SleekDB\Store;

$userStore = new Store("users", $dataDir);
$commentStore = new Store("comments", $dataDir);

$users = $usersStore
  ->createQueryBuilder()
  ->join(function($user) use ($commentStore){
    // returns result
    return $commentStore->findBy([ "userId", "=", $user["_id"] ]);
  }, "comments")
  ->getQuery()
  ->fetch();

// or
$users = $userStore
  ->createQueryBuilder()
  ->join(function($user) use ($commentStore){
    // returns Querybuilder
    return $commentStore
      ->createQueryBuilder()
      ->where([ "userId", "=", $user["_id"] ]);
  }, "comments")
  ->getQuery()
  ->fetch();
```

### Example 2

Use multiple `join()`.<br/>
Retrieve all users with their comments and their articles.

```php
use SleekDB\Store;

$userStore = new Store("users", $dataDir);
$commentStore = new Store("comments", $dataDir);
$articleStore = new Store("articles", $dataDir);

$users = $userStore
  ->createQueryBuilder()
  ->join(function($user) use ($commentStore) {
    // returns result
    return $commentStore->findBy([ "userId", "=", $user["_id"] ]);
  }, "comments")
  ->join(function($user) use ($articleStore) {
    // returns result
    return $articleStore->findBy([ "author", "=", $user["_id"] ]);
  }, "articles")
  ->getQuery()
  ->fetch();
```

### Example 3

Use `join()` within a join sub query.<br/>
Retrieve all users with their created articles containing the comments.

```php
use SleekDB\Store;

$userStore = new Store("users", $dataDir);
$commentStore = new Store("comments", $dataDir);
$articleStore = new Store("articles", $dataDir);

$users = $userStore
  ->createQueryBuilder()
  ->join(function($user) use ($articleStore, $commentStore){
    // returns QueryBuilder
    return $articleStore
      ->createQueryBuilder()
      ->where([ "author", "=", $user["_id"] ])
      ->join(function($article) use ($commentStore){
        // returns result
        return $commentStore->findBy("articleId", "=", $article["_id"]);
      }, "comments");

  }, "articles")
  ->getQuery()
  ->fetch();
```
