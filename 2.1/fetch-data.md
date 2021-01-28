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

## Summary

- findAll
- findById
- findBy
- findOneBy

## Get all documents

```php
function findAll(): array
```

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

## Get a single document with its \_id

With this method SleekDB doesn't traverse through all files. Instead, it accesses the file directly, what makes this method especially fast.

```php
function findById(int $id): array|null
```

### Parameters

1. # $id: int
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

## Get one or multiple documents

```php
function findBy(array $criteria, array $orderBy = null, int $limit = null, int $offset = null): array|null
```

### Parameters

1. # $criteria: array
   One or multiple where clauses.


    * [["city", "=", "london"], ["age", ">", 18]]<br/>
      `WHERE city = "london" AND age > 18`

2. # $orderBy: array
   Order in which the results will be sort.


    * ["name" => "asc"]

3. # $limit: int
   Limit the result to a specific amount.
4. # $offset: offset
   Skip a specific amount of documents.

### Return value

Returns found `documents in an array` or `null` if nothing is found.

### Example

```php
$news = $newsStore->findBy(["author", "=", "John"], ["title" => "asc"], 10, 20);
// First 20 documents skipped, limited to 10 and ascending sort by title where author is John.
```

#### Example result

```
[ ["_id" => 12, "title" => "Best Database"], ["_id" => 4, "title" => "Why SleekDB"], ...]
```

<br/>

## Get one document.

```php
function findOneBy(array $criteria): array|null
```

### Parameters

1. # $criteria: array
   One or multiple where clauses. \* [["city", "=", "london"], ["age", ">", 18]]<br/>`WHERE city = "london" AND age > 18`

### Return value

Returns `one document` or `null` if nothing is found.

### Examples

```php
$news = $newsStore->findOneBy(["author", "=", "Mike"]);
// Returns one news article of the author called Mike
```

#### Example result

```
["_id" => 18, "title" => "SleekDB is super fast", "author" => "Mike"]
```
