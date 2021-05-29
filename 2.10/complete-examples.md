<!--METADATA
{
    "title": "Complete Examples",
    "url": "complete-examples",
    "icon": "color-wand"
}
!METADATA-->

# Some complete code examples

We assume you read the documentation so you can understand the following small code examples.

## Summary

- <a class="gotoblock" href="#/complete-examples#insert-one">Insert one document</a>
- <a class="gotoblock mb" href="#/complete-examples#insert-multiple">Insert multiple documents</a>

- <a class="gotoblock" href="#/complete-examples#retrieve-store">Retrieve document/s using just the Store class</a>
- <a class="gotoblock mb" href="#/complete-examples#retrieve-query_builder">Retrieve document/s using the QueryBuilder class</a>

- <a class="gotoblock mb" href="#/complete-examples#edit-query_builder">Editing/ Updating documents using the QueryBuilder class</a>

- <a class="gotoblock mb" href="#/complete-examples#group">Grouping documents</a>

- <a class="gotoblock" href="#/complete-examples#search-store">Searching documents using just the Store object</a>
- <a class="gotoblock" href="#/complete-examples#search-query_builder">Searching documents using the QueryBuilder object</a>

## Insert one document {#complete-examples-insert-one}

```php
require_once "./vendor/autoload.php";

use SleekDB\Store;
use SleekDB\Query;

$databaseDirectory = __DIR__."/database";

// applying the store configuration is optional
$storeConfiguration = [
  "auto_cache" => true,
  "cache_lifetime" => null,
  "timeout" => 120,
  "primary_key" => "_id",
  "search" => [
    "min_length" => 2,
    "mode" => "or",
    "score_key" => "scoreKey",
    "algorithm" => Query::SEARCH_ALGORITHM["hits"]
  ]
];

// creating a new store object
$userStore = new Store("users", $databaseDirectory, $storeConfiguration);

$newUser = [
  "username" => "Bob",
  "age" => 20,
  "address" => [
    "street" => "down street",
    "streetNumber" => 12,
    "postalCode" => "8174",
  ],
];

$newUser = $userStore->insert($newUser);

// Output user with its unique id.
header("Content-Type: application/json");

echo json_encode($newUser);
```

## Insert multiple documents {#complete-examples-insert-multiple}

```php
require_once "./vendor/autoload.php";

use SleekDB\Store;
use SleekDB\Query;

$databaseDirectory = __DIR__."/database";

// applying the store configuration is optional
$storeConfiguration = [
  "auto_cache" => true,
  "cache_lifetime" => null,
  "timeout" => 120,
  "primary_key" => "_id",
  "search" => [
    "min_length" => 2,
    "mode" => "or",
    "score_key" => "scoreKey",
    "algorithm" => Query::SEARCH_ALGORITHM["hits"]
  ]
];

// creating a new store object
$userStore = new Store("users", $databaseDirectory, $storeConfiguration);

$newUsers = [
  [
    "username" => "Lisa",
    "age" => 17,
    "address" => [
      "street" => "up street",
      "streetNumber" => 48,
      "postalCode" => "1822",
    ],
  ],
  [
    "username" => "Bob",
    "age" => 20,
    "address" => [
      "street" => "down street",
      "streetNumber" => 12,
      "postalCode" => "8174",
    ],
  ]
];

$newUsers = $userStore->insertMany($newUsers);

// Output users with their unique id.
header("Content-Type: application/json");

echo json_encode($newUsers);
```

## Retrieving documents using just the Store object {#complete-examples-retrieve-store}

```php
require_once "./vendor/autoload.php";

use SleekDB\Store;
use SleekDB\Query;

$databaseDirectory = __DIR__."/database";

// applying the store configuration is optional
$storeConfiguration = [
  "auto_cache" => true,
  "cache_lifetime" => null,
  "timeout" => 120,
  "primary_key" => "_id",
  "search" => [
    "min_length" => 2,
    "mode" => "or",
    "score_key" => "scoreKey",
    "algorithm" => Query::SEARCH_ALGORITHM["hits"]
  ]
];

// creating a new store object
$userStore = new Store("users", $databaseDirectory, $storeConfiguration);

$whereCondition = [
  ["location", "IN", ["new york", "london"]],
  "OR",
  ["age", ">", 29]
];

// Pagination
$page = 1;
$limit = 10;
$skip = ($page - 1) * $limit;

// order by _id and limit result to 10
$result = $userStore->findBy($whereCondition, ["_id" => "DESC"], $limit, $skip);

// Output
header("Content-Type: application/json");

echo json_encode($result);
```

## Retrieving documents using the QueryBuilder object {#complete-examples-retrieve-query_builder}

```php
require_once "./vendor/autoload.php";

use SleekDB\Store;
use SleekDB\Query;

$databaseDirectory = __DIR__."/database";

// applying the store configuration is optional
$storeConfiguration = [
  "auto_cache" => true,
  "cache_lifetime" => null,
  "timeout" => 120,
  "primary_key" => "_id",
  "search" => [
    "min_length" => 2,
    "mode" => "or",
    "score_key" => "scoreKey",
    "algorithm" => Query::SEARCH_ALGORITHM["hits"]
  ]
];

// creating a new store object
$userStore = new Store("users", $databaseDirectory, $storeConfiguration);

// Pagination
$page = 1;
$limit = 10;
$skip = ($page - 1) * $limit;

$result = $userStore->createQueryBuilder()
  ->where([
    ["location", "IN", ["new york", "london"]],
    "OR",
    ["age", ">", 29]
  ])
  ->orderBy(["_id" => "DESC"])
  ->limit($limit)
  ->skip($skip)
  ->getQuery()
  ->fetch();

// Output
header("Content-Type: application/json");

echo json_encode($result);
```

## Editing/ Updating documents using the QueryBuilder object {#complete-examples-edit-query_builder}

```php
require_once "./vendor/autoload.php";

use SleekDB\Store;
use SleekDB\Query;

$databaseDirectory = __DIR__."/database";

// applying the store configuration is optional
$storeConfiguration = [
  "auto_cache" => true,
  "cache_lifetime" => null,
  "timeout" => 120,
  "primary_key" => "_id",
  "search" => [
    "min_length" => 2,
    "mode" => "or",
    "score_key" => "scoreKey",
    "algorithm" => Query::SEARCH_ALGORITHM["hits"]
  ]
];

// creating a new store object
$userStore = new Store("users", $databaseDirectory, $storeConfiguration);

$result = $userStore->createQueryBuilder()
  ->where([
    ["location", "IN", ["new york", "london"]],
    "OR",
    ["age", ">", 29]
  ])
  ->orderBy(["_id" => "DESC"])
  ->getQuery()
  ->update(["status" => "VIP"]);

// Output
header("Content-Type: application/json");

echo json_encode($result);
```

## Grouping documents {#complete-examples-group}

```php
require_once "./vendor/autoload.php";

use SleekDB\Store;
use SleekDB\Query;

$databaseDirectory = __DIR__."/database";

// applying the store configuration is optional
$storeConfiguration = [
  "auto_cache" => true,
  "cache_lifetime" => null,
  "timeout" => 120,
  "primary_key" => "_id",
  "search" => [
    "min_length" => 2,
    "mode" => "or",
    "score_key" => "scoreKey",
    "algorithm" => Query::SEARCH_ALGORITHM["hits"]
  ]
];

// creating a new store object
$userStore = new Store("users", $databaseDirectory, $storeConfiguration);

// Pagination
$page = 1;
$limit = 10;
$skip = ($page - 1) * $limit;

$result = $userStore->createQueryBuilder()
  ->where([ "location", "IN", ["new york", "london"] ])
  ->select([ "age", "peopleCount", "followerAmount" => ["SUM" => "followers"] ])
  ->groupBy(["age"], "peopleCount", true)
  ->having([ ["followerAmount", ">", 100], "OR", ["age", "<", 16] ])
  ->orderBy(["followerAmount" => "DESC"])
  ->limit($limit)
  ->skip($skip)
  ->getQuery()
  ->fetch();

// Output
header("Content-Type: application/json");

echo json_encode($result);
```

## Searching documents using just the Store object {#complete-examples-search-store}

```php
require_once "./vendor/autoload.php";

use SleekDB\Store;
use SleekDB\Query;

$databaseDirectory = __DIR__."/database";

// applying the store configuration is optional
$storeConfiguration = [
  "auto_cache" => true,
  "cache_lifetime" => null,
  "timeout" => 120,
  "primary_key" => "_id",
  "search" => [
    "min_length" => 2,
    "mode" => "or",
    "score_key" => "scoreKey",
    "algorithm" => Query::SEARCH_ALGORITHM["hits"]
  ]
];

// creating a new store object
$newsStore = new Store("news", $databaseDirectory, $storeConfiguration);

// Pagination
$page = 1;
$limit = 10;
$skip = ($page - 1) * $limit;

$searchQuery = "SleekDB best database";

$result = $newsStore->search(
    ["title", "content"], 
    $searchQuery, 
    ["scoreKey" => "DESC"],
    $limit,
    $skip
  );

// Output
header("Content-Type: application/json");

echo json_encode($result);
```

## Searching documents using the QueryBuilder object {#complete-examples-search-query_builder}

```php
require_once "./vendor/autoload.php";

use SleekDB\Store;
use SleekDB\Query;

$databaseDirectory = __DIR__."/database";

// applying the store configuration is optional
$storeConfiguration = [
  "auto_cache" => true,
  "cache_lifetime" => null,
  "timeout" => 120,
  "primary_key" => "_id",
  "search" => [
    "min_length" => 2,
    "mode" => "or",
    "score_key" => "scoreKey",
    "algorithm" => Query::SEARCH_ALGORITHM["hits"]
  ]
];

// creating a new store object
$newsStore = new Store("news", $databaseDirectory, $storeConfiguration);

// Pagination
$page = 1;
$limit = 10;
$skip = ($page - 1) * $limit;

$searchQuery = "SleekDB best database";

$result = $newsStore->createQueryBuilder()
  ->search(["title", "content"], $searchQuery)
  ->orderBy(["searchScore" => "DESC"])
  ->except(["searchScore"]) // remove score from result
  ->limit($limit)
  ->skip($skip)
  ->getQuery()
  ->fetch();

// Output
header("Content-Type: application/json");

echo json_encode($result);
```