<!--METADATA
{
    "title": "Delete Data",
    "url": "delete-data",
    "icon": "trash"
}
!METADATA-->

# Delete Data

To delete documents you can use the `deleteBy()` and `deleteById()` methods of the `Store` class.

> ℹ️ If you need to make a more complex delete look into <a class="gotoblock" href="/#/query-builder">QueryBuilder</a> and <a class="gotoblock" href="#/query">Query</a>.

## Summary

- deleteBy
- deleteById

## Delete one or multiple documents

```php
function deleteBy(array $criteria, int $returnOption = Query::DELETE_RETURN_BOOL): array|bool|int
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
      - `!=` Match not equal against data.
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

2. # $returnOption: int
   Different return options provided with constants of the `Query` class


    * `Query::DELETE_RETURN_BOOL` (Default)<br/>Return true or false
    * `Query::DELETE_RETURN_RESULTS`<br/>Retrieve deleted files as an array
    * `Query::DELETE_RETURN_COUNT`<br/>Returns the amount of deleted documents

### Return value

This method returns based on the given return option either `boolean`, `int` or `array`.

### Example 1

Lets delete all user whose name is "Joshua Edwards"

```php
$userStore->deleteBy(["name", "=", "Joshua Edwards"]);
// Returns true
```

Lets delete all user whose name is "Joshua Edwards" and retrieve deleted documents.

```php
use SleekDB/Query;
$deletedUsers = $userStore->deleteBy(["name", "=", "Joshua Edwards"], Query::DELETE_RETURN_RESULTS);
```

#### Example result

```
[ ["_id" => 12, "name" => "Joshua Edwards"], ["_id" => 14, "name" => "Joshua Edwards"], ... ]
```

### Example 2

Deletion with more complex where statement.

```sql
WHERE ( name = "Joshua Edwards" OR name = "Mark Schiffer" ) AND ( age > 30 OR age < 10 )
```

```php
$userStore->deleteBy(
    [
      [
        ["name", "=", "Joshua Edwards"],
        "OR",
        ["name", "=", "Mark Shiffer"],
      ],
      "AND", // <-- Optional
      [
        ["age", ">", 30],
        "OR",
        ["age", "<", 10]
      ]
    ]
  );
```

<br/>

## Delete one document with its \_id

This method is especially fast because SleekDB uses the \_id to directly delete the document and does not traverse through all files.

```php
function deleteById(int $id): bool
```

### Parameters

1. # $id: int
   The \_id of a document located in the store.

### Return value

Returns `true` if document does not exist or deletion was successful or `false` on failure.

### Example

```php
$userStore->deleteById(12);
// Returns true
```
