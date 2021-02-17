<!--METADATA
{
    "title": "Query",
    "url": "query",
    "icon": "flash"
}
!METADATA-->

# Query

With the Query object you can execute a query.

To get the Query object use the `getQuery` method of the <a class="gotoblock" href="/#/query-builder">QueryBuilder</a> class:

```php
$userQuery = $userQueryBuilder->getQuery();
```

## Summary

- <a class="gotoblock" href="#/query#fetch">fetch</a>
- <a class="gotoblock" href="#/query#first">first</a>
- <a class="gotoblock" href="#/query#exists">exists</a>
- <a class="gotoblock" href="#/query#update">update</a>
- <a class="gotoblock" href="#/query#delete">delete</a>

## fetch() {#query-fetch}

Execute a query and retrieve an array containing all documents found.

```php
function fetch(): array
```

### Return value

An array containing all documents found or an empty array.

### Example

Retrieve all users that are located in london.

```php
$user = $userQueryBuilder
  ->where([ "city", "=", "london" ])
  ->getQuery()
  ->fetch();
```

#### Result

```
[
    [
        "_id" => 1,
        "name" => "John",
        "city" => "london"
    ],
    [
        "_id" => 4,
        "name" => "Max",
        "city" => "london"
    ],
    ...
]
```

## first() {#query-first}

It is more efficient than `fetch` but one caveat is that the `orderBy` will not work when using this method to get the very first item.

```php
function first(): array
```

### Return value

Returns the very first document discovered or an empty array if nothing found.

### Example

```php
$user = $userQueryBuilder
  ->where([ "email", "=", "foo@bar.com" ])
  ->getQuery()
  ->first();
```

#### Result

```
[
    "_id" => 1,
    "name" => "John",
    "email" => "foo@bar.com"
]
```

## exists() {#query-exists}

It is more efficient than using `fetch` to check if some data exists or not. For example, you may use exists method to check if a username or email address already exists or not.

```php
function exists(): bool
```

### Return value

Returns `true` if a document exists and `false` if no document found.

### Example

```php
$userNameExists = $userQueryBuilder
  ->where([ "username", "=", "foobar" ])
  ->getQuery()
  ->exists();
```

## update() {#query-update}

Update one or multiple documents based on a given query.

```php
function update(array $updatable, bool $returnUpdatedDocuments = false): array|bool
```

### Parameters

1. # $updatable: array
   An array containing the properties to update.<br/>
   Update of nested values possible by using a dot between fieldNames (Example 2)<br/>
   If a field does not exist in a document, it will be added.

### Return value
If **$returnUpdatedDocuments = false**:<br/>
Returns `true` on success and `false` if no documents found to update.

If **$returnUpdatedDocuments = true**:<br/>
Returns `updated documents` on success and `false` if no documents found to update.

### Example 1

Set the status of all users that are located in london to VIP.

```php
$userStore
  ->createQueryBuilder()
  ->where([ "city", "=", "london" ])
  ->getQuery()
  ->update([ "status" => "VIP" ]);
```

### Example 2

Change the street of the user with \_id = 24.<br/>
**Note**: The street is in a nested array.

```php
$userStore
  ->createQueryBuilder()
  ->where("city", "=", "london")
  ->update([ "address.street" => "first street" ]);
```

#### Result

```php
[
  [
    "city" => "london".
    "address" => [
      "street" => "first street",
      "postalCode" => "41824"
    ],
    ...
  ],
  ...
]
```

## delete() {#query-delete}

Delete one or multiple documents based on a given query.

```php
function delete(int $returnOption = Query::DELETE_RETURN_BOOL)
```

### Parameters

1. # $returnOption: int
   Different return options provided with constants of the `Query` class
   - `Query::DELETE_RETURN_BOOL` (Default)<br/>Return true or false
   - `Query::DELETE_RETURN_RESULTS`<br/>Retrieve deleted files as an array
   - `Query::DELETE_RETURN_COUNT`<br/>Returns the amount of deleted documents

### Return value

This method returns based on the given return option either `boolean`, `int` or `array`.

### Example 1

Delete all users that are not active.

```php
$result = $userStore
  ->createQueryBuilder()
  ->where([ "active", "=", false ])
  ->getQuery()
  ->delete();
// output: true
```

### Example 2

Delete all users that are not active and retrieve the amount of deleted users.

```php
use SleekDB\Query;

$result = $userStore
  ->createQueryBuilder()
  ->where([ "active", "=", false ])
  ->getQuery()
  ->delete(Query::DELETE_RETURN_COUNT);
// output: 14
```

### Example 3

Delete all users that are not active and retrieve the deleted users.

```php
use SleekDB\Query;

$result = $userStore
  ->createQueryBuilder()
  ->where([ "active", "=", false ])
  ->getQuery()
  ->delete(Query::DELETE_RETURN_RESULT);
// output: [ ["_id" => 1, "name" => "Max"], ["_id" => 4, "name" => "John"], ... ]
```
