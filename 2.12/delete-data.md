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

## Brief repetition of Store object creation

For a more detailed documentation on `Store` object creation refer to the corresponding <a class="gotoblock" href="#/managing-store">"Managing Store" documentation</a>.

```php
use SleekDB\Store;
$userStore = new Store('users', __DIR__ . "/database");
```

## Summary

- <a class="gotoblock" href="#/delete-data#deleteBy">deleteBy</a>
- <a class="gotoblock" href="#/delete-data#deleteById">deleteById</a>

## Delete one or multiple documents {#delete-data-deleteBy}

```php
function deleteBy(array $criteria, int $returnOption = Query::DELETE_RETURN_BOOL): array|bool|int
```

### Parameters

1. # $criteria: array
   One or multiple where conditions.<br/>
   Visit the <a class="gotoblock" href="#/criteria">$criteria</a> documentation page to learn more on how to define conditions.

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

## Delete one document with its \_id {#delete-data-deleteById}

This method is especially fast because SleekDB uses the \_id to directly delete the document and does not traverse through all files.

```php
function deleteById(int|string $id): bool
```

### Parameters

1. # $id: int|string
   The \_id of a document located in the store.

### Return value

Returns `true` if document does not exist or deletion was successful or `false` on failure.

### Example

```php
$userStore->deleteById(12);
// Returns true
```
