<!--METADATA
{
    "title": "Fetch Data",
    "url": "fetch-data",
    "icon": "book"
}
!METADATA-->

# Fetch Data

To get data from the store SleekDB provides some simple yet powerful methods.

> ℹ️ If you need to make more complex queries look into <a class="gotoblock" href="/#/query-builder">QueryBuilder</a>.

## Brief repetition of Store object creation

For a more detailed documentation on `Store` object creation refer to the corresponding <a class="gotoblock" href="#/managing-store">"Managing Store" documentation</a>.

```php
use SleekDB\Store;
$newsStore = new Store('news', __DIR__ . "/database");
```

## Summary

- <a class="gotoblock" href="#/fetch-data#findAll">findAll</a>
- <a class="gotoblock" href="#/fetch-data#findById">findById</a>
- <a class="gotoblock" href="#/fetch-data#findBy">findBy</a>
- <a class="gotoblock" href="#/fetch-data#findOneBy">findOneBy</a>
- <a class="gotoblock" href="#/fetch-data#count">count</a>

## Get all documents {#fetch-data-findAll}

```php
function findAll(array $orderBy = null, int $limit = null, int $offset = null): array
```

### Parameters

1. # $orderBy: array
   Sort the result by one or multiple fields.

    * ["name" => "asc"]
    * ["name" => "asc", "age" => "desc"]

2. # $limit: int
   Limit the result to a specific amount.

3. # $offset: offset
   Skip a specific amount of documents.

### Return value

Returns either an `array containing all documents` of that store or an `empty array` if there are no documents.

### Example

```php
$allNews = $newsStore->findAll();
```

#### Example result

```
[["_id" => 12, "title" => "We love SleekDB"], ["_id" => 14, "title" => "NoSQL with just PHP"], ...]
```

<br/>

## Get a single document with its \_id {#fetch-data-findById}

With this method SleekDB doesn't traverse through all files. Instead, it accesses the file directly, what makes this method especially fast.

```php
function findById(int|string $id): array|null
```

### Parameters

1. # $id: int|string
   The \_id of a document located in the store.

### Return value

Returns `one document` or `null` if document could not be found.

### Example

```php
$news = $newsStore->findById(12);
```

#### Example result

```
["_id" => 12, "title" => "SleekDB is the Best", ...]
```

<br/>

## Get one or multiple documents {#fetch-data-findBy}

```php
function findBy(array $criteria, array $orderBy = null, int $limit = null, int $offset = null): array
```

### Parameters

1. # $criteria: array
   One or multiple where conditions.<br/>
   Visit the <a class="gotoblock" href="#/criteria">$criteria</a> documentation page to learn more on how to define conditions.

2. # $orderBy: array
   Sort the result by one or multiple fields.

    * ["name" => "asc"]
    * ["name" => "asc", "age" => "desc"]

3. # $limit: int
   Limit the result to a specific amount.
4. # $offset: offset
   Skip a specific amount of documents.

### Return value

Returns found `documents in an array` or `an empty array` if nothing is found.

### Example 1

```php
$news = $newsStore->findBy(["author", "=", "John"], ["title" => "asc"], 10, 20);
// First 20 documents skipped, limited to 10 and ascending sort by title where author is John.
```

#### Result

```
[ ["_id" => 12, ... "title" => "Best Database"], ["_id" => 4, ... "title" => "Why SleekDB"], ...]
```

### Example 2

More complex where clause.

```sql
WHERE ( author = "John" OR author = "Mark" ) AND ( topic like "School%" OR topic like "Work%" )
```

```php
$news = $newsStore->findBy(
      [
         [
            ["author", "=", "John"], "OR", ["author", "=", "Mark"],
         ],
         "AND", // <-- Optional
         [
            ["topic", "like", "School%"], "OR", ["topic", "like", "Work%"]
         ]
      ],
      ["title" => "asc"],
      10,
      20
   );
```

<br/>

## Get one document {#fetch-data-findOneBy}

```php
function findOneBy(array $criteria): array|null
```

### Parameters

1. # $criteria: array
   One or multiple where conditions.<br/>
   Visit the <a class="gotoblock" href="#/criteria">$criteria</a> documentation page to learn more on how to define conditions.

### Return value

Returns `one document` or `null` if nothing is found.

### Example 1

```php
$news = $newsStore->findOneBy(["author", "=", "Mike"]);
// Returns one news article of the author called Mike
```

#### Result

```
["_id" => 18, "title" => "SleekDB is super fast", "author" => "Mike"]
```


### Example 2

More complex where clause.

```sql
WHERE ( author = "John" OR author = "Mark" ) AND ( topic like "School%" OR topic like "Work%" )
```

```php
$news = $newsStore->findOneBy(
      [
         [
            ["author", "=", "John"], "OR", ["author", "=", "Mark"],
         ],
         "AND", // <-- Optional
         [
            ["topic", "like", "School%"], "OR", ["topic", "like", "Work%"]
         ]
      ]
   );
```

## Get the amount of all documents stored {#fetch-data-count}

This method is especially fast because it just counts the amount of files.

```php
function count(): int
```

### Return value

Returns the amount of documents in the store as an `int`.

### Example

```php
$newsCount = $newsStore->count();
// Returns: 27 
```