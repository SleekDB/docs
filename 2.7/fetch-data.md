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

## Get all documents {#fetch-data-findAll}

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
function findBy(array $criteria, array $orderBy = null, int $limit = null, int $offset = null): array|null
```

### Parameters

1. # $criteria
   One or multiple where conditions.<br/>
   The criteria can be nested as much as needed.

- [$fieldName, $condition, $value]
- [ [$fieldName, $condition, $value], [$fieldName, $condition, $value], ... ]
- [ [$fieldName, $condition, $value], OPERATION ,[$fieldName, $condition, $value], ... ]

  - # $fieldName: string

      The field name argument is the property that we want to check in our data object.

      As our data object is basically a JSON document so it could have nested properties.

      To target nested properties we use a single dot between the property/field name.

      **Example:** From our above users object if we want to target the "country" property of a user, then we would pass `location.country` in this argument, because "location" is the parent property of the "country" property in our data object.

  - # $condition: string

      To apply the comparison filters we use this argument.<br/>
      The condition is case insensitive.

      Allowed conditional operators are:

      - `=` Match equal against data.
      - `===` Match equal against data.
      - `==` Match equal against data. (Type unsafe comparison)
      - `!=` Match not equal against data.
      - `!==` Match not equal against data.
      - `<>` Match not equal against data. (Type unsafe comparison)
      - `>` Match greater than against data.
      - `>=` Match greater equal against data.
      - `<` Match less than against data.
      - `<=` Match less equal against data.
      - `LIKE` Match using wildcards.
      - `NOT LIKE` Match using wildcards. **Negation** of result.<br/>
        Supported wildcards:
          - `%` Represents zero or more characters<br/>
            Example: bl% finds bl, black, blue, and blob
          - `_` Represents a single character<br/>
            Example: h_t finds hot, hat, and hit
          - `[]` Represents any single character within the brackets<br/>
            Example: h[oa]t finds hot and hat, but not hit
          - `^` Represents any character not in the brackets<br/>
            Example: h[^oa]t finds hit, but not hot and hat
          - `-` Represents a range of characters<br/>
            Example: c[a-b]t finds cat and cbt
      - `IN` $value has to be an `array`. Check if data is in given list. 
      - `NOT IN` $value has to be an `array`. Check if data is **not** in given list. 
      - `BETWEEN` $value has to be an `array` with a lengeth of 2.<br/>
        Check if data is within first and second value.<br/>
        Begin and end values are included.
      - `NOT BETWEEN` $value has to be an `array` with a length of 2.<br/>
        Check if data is **not** within first and second value.<br/>
        Begin and end values are included.


  - # $value
      Data that will be checked against the property value of the JSON documents.<br/>
      Can also be a DateTime object if the property value of the JSON document is convertable into a DateTime object. Refer to the <a class="gotoblock" href="#/dates">Working with Dates</a> documentation to learn more about that.

  - # OPERATION: string
    It is used to connect multiple conditions.<br/>
    The operation is optional and can be set to `AND` or `OR`.<br/>
    Default: `AND`

2. # $orderBy: array
   Sort the result by one or multiple fields.

    * ["name" => "asc"]
    * ["name" => "asc", "age" => "desc"]

3. # $limit: int
   Limit the result to a specific amount.
4. # $offset: offset
   Skip a specific amount of documents.

### Return value

Returns found `documents in an array` or `null` if nothing is found.

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

## Get one document. {#fetch-data-findOneBy}

```php
function findOneBy(array $criteria): array|null
```

### Parameters

1. # $criteria
   One or multiple where conditions.<br/>
   The criteria can be nested as much as needed.

- [$fieldName, $condition, $value]
- [ [$fieldName, $condition, $value], [$fieldName, $condition, $value], ... ]
- [ [$fieldName, $condition, $value], OPERATION ,[$fieldName, $condition, $value], ... ]

  - # $fieldName: string

      The field name argument is the property that we want to check in our data object.

      As our data object is basically a JSON document so it could have nested properties.

      To target nested properties we use a single dot between the property/field name.

      **Example:** From our above users object if we want to target the "country" property of a user, then we would pass `location.country` in this argument, because "location" is the parent property of the "country" property in our data object.

  - # $condition: string

      To apply the comparison filters we use this argument.<br/>
      The condition is case insensitive.

      Allowed conditional operators are:

      - `=` Match equal against data.
      - `===` Match equal against data.
      - `==` Match equal against data. (Type unsafe comparison)
      - `!=` Match not equal against data.
      - `!==` Match not equal against data.
      - `<>` Match not equal against data. (Type unsafe comparison)
      - `>` Match greater than against data.
      - `>=` Match greater equal against data.
      - `<` Match less than against data.
      - `<=` Match less equal against data.
      - `LIKE` Match using wildcards.
      - `NOT LIKE` Match using wildcards. **Negation** of result.<br/>
        Supported wildcards:
          - `%` Represents zero or more characters<br/>
            Example: bl% finds bl, black, blue, and blob
          - `_` Represents a single character<br/>
            Example: h_t finds hot, hat, and hit
          - `[]` Represents any single character within the brackets<br/>
            Example: h[oa]t finds hot and hat, but not hit
          - `^` Represents any character not in the brackets<br/>
            Example: h[^oa]t finds hit, but not hot and hat
          - `-` Represents a range of characters<br/>
            Example: c[a-b]t finds cat and cbt
      - `IN` $value has to be an `array`. Check if data is in given list. 
      - `NOT IN` $value has to be an `array`. Check if data is **not** in given list. 
      - `BETWEEN` $value has to be an `array` with a lengeth of 2.<br/>
        Check if data is within first and second value.<br/>
        Begin and end values are included.
      - `NOT BETWEEN` $value has to be an `array` with a length of 2.<br/>
        Check if data is **not** within first and second value.<br/>
        Begin and end values are included.


  - # $value
      Data that will be checked against the property value of the JSON documents.<br/>
      Can also be a DateTime object if the property value of the JSON document is convertable into a DateTime object. Refer to the <a class="gotoblock" href="#/dates">Working with Dates</a> documentation to learn more about that.

  - # OPERATION: string
    It is used to connect multiple conditions.<br/>
    The operation is optional and can be set to `AND` or `OR`.<br/>
    Default: `AND`

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