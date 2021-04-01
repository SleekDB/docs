<!--METADATA
{
    "title": "$criteria",
    "url": "criteria",
    "icon": "checkmark-circle-outline"
}
!METADATA-->

# $criteria

The $criteria is an argument accepted by many different methods.<br/>
It is used to define one or multiple conditions to filter documents.

Methods that accept criteria as an argument:

- <a class="gotoblock" href="#/fetch-data#findBy">Store->findBy()</a>
- <a class="gotoblock" href="#/fetch-data#findOneBy">Store->findOneBy()</a>
- <a class="gotoblock" href="#/delete-data#deleteBy">Store->deleteBy()</a>
- <a class="gotoblock" href="#/query-builder#where">QueryBuilder->where()</a>
- <a class="gotoblock" href="#/query-builder#orWhere">QueryBuilder->orWhere()</a>
- <a class="gotoblock" href="#/query-builder#having">QueryBuilder->having()</a>

## Summary

- <a class="gotoblock" href="#/criteria#criteria">$criteria: array</a>
- <a class="gotoblock" href="#/criteria#examples">Examples</a>

## $criteria: array {#criteria-criteria}
   One or multiple conditions.<br/>
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
      The condition is **case insensitive**.

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
      - `CONTAINS` Returns `true` if stored data is an array and the array contains $value.
      - `NOT CONTAINS` Returns `true` if stored data is not an array or the array **not** contains $value.
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

## Examples {#criteria-examples}

### Example 1

Find all news articles of the author "John".

```php
$news = $newsStore->findBy(["author", "=", "John"]);
```

### Example 2

Find all news articles of the author "John", that have "cat" in their title.

```php
$news = $newsStore->findBy([
    ["author", "=", "John"], 
    "AND", // <-- Optional
    ["title", "LIKE", "%cat%"]
  ]);
```
```php
$news = $newsStore->findBy([
    ["author", "=", "John"], 
    ["title", "LIKE", "%cat%"]
  ]);
```

### Example 3

Find all news articles of the author "John" or "Smith", that have "cat" in their title.

```php
$news = $newsStore->findBy([
    [
      ["author", "=", "John"], 
      "OR",
      ["author", "=", "Smith"], 
    ],
    "AND", // <-- Optional
    ["title", "LIKE", "%cat%"]
  ]);
```
```php
$news = $newsStore->findBy([
    [
      ["author", "=", "John"], 
      "OR",
      ["author", "=", "Smith"], 
    ],
    ["title", "LIKE", "%cat%"]
  ]);
```