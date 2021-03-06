<!--METADATA
{
    "title": "QueryBuilder",
    "url": "query-builder",
    "icon": "build"
}
!METADATA-->

# QueryBuilder

The QueryBuilder is used to **prepare more complex queries**, **`not to execute the query!`**. You can create a QueryBuilder with the `createQueryBuilder()` method of the `Store` class.

```php
$userQueryBuilder = $userStore->createQueryBuilder();
```

## Summary

- getQuery
- where
- orWhere
- nestedWhere
- in
- notIn
- select
- except
- distinct
- skip
- limit
- orderBy
- search
- Cache management
- join

## getQuery()

With the `getQuery` method of the QueryBuilder class you can **`retrieve the Query object to execute the query`**.

```php
function getQuery(): Query
```

The most important methods of the Query class to execute a query are:

- # fetch()
  Retrieve multiple documents
- # first()
  Retrieve first found document
- # exists()
  Check if a document for given query exist
- # delete()
  Delete all documents that are found with given query
- # update()
  Update all documents that are found with given query

For more details on query execution please visit the <a class="gotoblock" href="/#/query">Query</a> page.

## where()

To filter data we use the where() method of the QueryBuilder object.<br/>If you provide multiple conditions they are connected with an `AND`.

```php
function where(array $criteria): QueryBuilder
```

### Parameters

1. # $criteria
   One or multiple where conditions

- [$fieldName, $condition, $value]
- [ [$fieldName, $condition, $value], [$fieldName, $condition, $value], ... ]

  - # $fieldName: string

      The field name argument is the property that we want to check in our data object.

      As our data object is basically a JSON document so it could have nested properties.

      To target nested properties we use a single dot between the property/field name.

      **Example:** From our above users object if we want to target the "country" property of a user, then we would pass `location.country` in this argument, because "location" is the parent property of the "country" property in our data object.

  - # $condition: string

      To apply the comparison filters we use this argument.

      Allowed conditional operators are:

      - `=` Match equal against data.
      - `!=` Match not equal against data.
      - `>` Match greater than against data.
      - `>=` Match greater equal against data.
      - `<` Match less than against data.
      - `<=` Match less equal against data.
      - `like` Match using wildcards. \
        Supported wildcards:
        - `%` Represents zero or more characters \
          Example: bl% finds bl, black, blue, and blob
        - `_` Represents a single character \
          Example: h_t finds hot, hat, and hit
        - `[]` Represents any single character within the brackets \
          Example: h[oa]t finds hot and hat, but not hit
        - `^` Represents any character not in the brackets \
          Example: h[^oa]t finds hit, but not hot and hat
        - `-` Represents a range of characters \
          Example: c[a-b]t finds cat and cbt

  - # $value
      Data that will be checked against the property value of the JSON documents.

### Examples

To only get the user whose country is equal to "England" we would query like this:

```php
// inline
$users = $userStore
  ->createQueryBuilder()
  ->where( [ "name", "=", "Joshua Edwards" ] )
  ->getQuery()
  ->fetch();

// creating the QueryBuilder
$userQueryBuilder = $userStore->createQueryBuilder();

// preparing the query with the QueryBuilder
$userQueryBuilder->where( [ "name", "=", "Joshua Edwards" ] );

// executing the query
$users = $userQueryBuilder->getQuery()->fetch();
```

You can also use multiple `where` conditions.

Retrieve all users that have `products.totalSaved > 10 AND products.totalBought > 20`.

```php
// inline & using where method multiple times
$users = $userQueryBuilder
  ->where( ["products.totalSaved", ">", 10] )
  ->where( ["products.totalBought", ">", 20] )
  ->getQuery()
  ->fetch();

// inline & using where method once
$users = $userQueryBuilder
  ->where(
    [
      ["products.totalSaved", ">", 10],
      ["products.totalBought", ">", 20]
    ]
  )
  ->getQuery()
  ->fetch();

// retrieve QueryBuilder
$userQueryBuilder = $userStore->createQueryBuilder()

// prepare query
$userQueryBuilder->where(
  [
    ["products.totalSaved", ">", 10],
    ["products.totalBought", ">", 20]
  ]
);

// execute query
$users = $userQueryBuilder->getQuery()->fetch();

```

## orWhere()

`orWhere(...)` works as the OR condition of SQL. SleekDB supports multiple orWhere as object chain.<br/>If you provide multiple conditions they are connected with an `AND`.

```php
function orWhere(array $criteria): QueryBuilder
```

### Properties

1. # $criteria
   One or multiple where conditions

- [$fieldName, $condition, $value]
- [ [$fieldName, $condition, $value], [$fieldName, $condition, $value], ... ]

  - # $fieldName: string

      The field name argument is the property that we want to check in our data object.

      As our data object is basically a JSON document so it could have nested properties.

      To target nested properties we use a single dot between the property/field name.

      **Example:** From our above users object if we want to target the "country" property of a user, then we would pass `location.country` in this argument, because "location" is the parent property of the "country" property in our data object.

  - # $condition: string

      To apply the comparison filters we use this argument.

      Allowed conditional operators are:

      - `=` Match equal against data.
      - `!=` Match not equal against data.
      - `>` Match greater than against data.
      - `>=` Match greater equal against data.
      - `<` Match less than against data.
      - `<=` Match less equal against data.
      - `like` Match using wildcards. \
        Supported wildcards:
        - `%` Represents zero or more characters \
          Example: bl% finds bl, black, blue, and blob
        - `_` Represents a single character \
          Example: h_t finds hot, hat, and hit
        - `[]` Represents any single character within the brackets \
          Example: h[oa]t finds hot and hat, but not hit
        - `^` Represents any character not in the brackets \
          Example: h[^oa]t finds hit, but not hot and hat
        - `-` Represents a range of characters \
          Example: c[a-b]t finds cat and cbt

  - # $value
      Data that will be checked against the property value of the JSON documents.

### Examples

Retrieve all users that have `(products.totalSaved > 10 AND products.totalBought > 20) OR products.shipped = 1`

```php
$users = $userQueryBuilder
  ->where(
    [
      ["products.totalSaved", ">", 10],
      ["products.totalBought", ">", 20]
    ]
  )
  ->orWhere( ["products.shipped", "=", 1] )
  ->getQuery()
  ->fetch();
```

Retrieve all users that have `products.totalSaved > 10 OR (products.totalBought > 20 AND products.shipped = 1) OR totalBought = 0`

```php
$users = $userQueryBuilder
  ->where( ["products.totalSaved", ">", 10] )
  ->orWhere(
    [
      [ "products.totalBought", ">", 20 ],
      [ "products.shipped", "=", 1 ]
    ]
  )
  ->orWhere( ["products.totalBought", "=", 0] )
  ->getQuery()
  ->fetch();
```

## nestedWhere()

With the `nestedWhere(...)` method you are able to make complex nested where statements.

```php
function nestedWhere(array $conditions): QueryBuilder
```

### Conditions array structure
```php
[
  "OUTERMOST_OPERATION" => [
    // conditions
  ]
];
```

### Properties

1. # $conditions
   Multiple where conditions

- Small Example:<br/>
  [ "OUTERMOST_OPERATION" => [ [$fieldName, $condition, $value], "OPERATION", [$fieldName, $condition, $value] ] ]


  - # $fieldName: string

      The field name argument is the property that we want to check in our data object.

      As our data object is basically a JSON document so it could have nested properties.

      To target nested properties we use a single dot between the property/field name.

      **Example:** From our above users object if we want to target the "country" property of a user, then we would pass `location.country` in this argument, because "location" is the parent property of the "country" property in our data object.

  - # $condition: string

      To apply the comparison filters we use this argument.

      Allowed conditional operators are:

      - `=` Match equal against data.
      - `!=` Match not equal against data.
      - `>` Match greater than against data.
      - `>=` Match greater equal against data.
      - `<` Match less than against data.
      - `<=` Match less equal against data.
      - `like` Match using wildcards. \
        Supported wildcards:
        - `%` Represents zero or more characters \
          Example: bl% finds bl, black, blue, and blob
        - `_` Represents a single character \
          Example: h_t finds hot, hat, and hit
        - `[]` Represents any single character within the brackets \
          Example: h[oa]t finds hot and hat, but not hit
        - `^` Represents any character not in the brackets \
          Example: h[^oa]t finds hit, but not hot and hat
        - `-` Represents a range of characters \
          Example: c[a-b]t finds cat and cbt

  - # $value
    Data that will be checked against the property value of the JSON documents.
  - # OUTERMOST_OPERATION: string
    Can be set to `AND` or `OR`.<br/>
    It is **optional** and will **default** to an `AND` operation.<br/>
    The outermost operation is optional and does specify how the given conditions are connected with other conditions, like the ones that are specified using the `where`, `orWhere`, in or `notIn` methods.
  - # OPERATION: string
    Can be set to `AND` or `OR`.<br/>
    The operation is used to connect multiple conditions.

    

### Examples

Retrieve all users whos name start with "a" or "b", that have products.totalSaved > 10 and products totalBought > 10 and are between 16 and 20 or 24 and 28 years old.

```sql
WHERE 
  (products.totalSaved > 10 AND products.totalBought > 20) 
  AND // <- Outermost Operation
  ( 
    (name like 'a%' OR name like 'b%') 
    AND 
    (
      (age >= 16 AND age < 20) 
      OR 
      (age >= 24 AND age < 28)) 
    )
```

```php
$users = $userQueryBuilder
  ->where(
    [
      ["products.totalSaved", ">", 10],
      ["products.totalBought", ">", 20]
    ]
  )
  ->nestedWhere(
    [
      "AND" => [ // <- Outermost operation
        [
          [ "name", "like", "a%" ], "OR", [ "name", "like", "b%" ]
        ],
        "AND",
        [
          [
            [ "age", ">=", 16 ], "AND", [ "age", "<", 20 ]
          ],
          "OR",
          [
            [ "age", ">=", 24 ], "AND", [ "age", "<", 28 ]
          ]
        ]
      ]
    ]
  )
  ->getQuery()
  ->fetch();
```

Retrieve all users that have the status "premium", live in "london" and are between the age of 16 and 20 or older than 30.

```php
$users = $userQueryBuilder
  ->where(
    [
      ["status", "=", "premium"]
    ]
  )
  ->nestedWhere(
    [
      [ // <- Outermost operation is optional (Default: "AND")
        [ "city", "=", "london" ]
        "AND",
        [
          [
            [ "age", ">=", 16 ], "AND", [ "age", "<", 20 ]
          ],
          "OR",
          [ "age", ">", 30 ]
        ]
      ]
    ]
  )
  ->getQuery()
  ->fetch();
```

### in()

in(...) works as the IN clause of SQL. SleekDB supports multiple IN as object chain for different fields.

```php
in(string $fieldName, array $values = []): QueryBuilder
```

### Parameters

1. # $fieldName: string

   The field name argument is the property that we want to check in our data object.

   You can also provide have nested properties separated with a dot ".".

2. # $values: array

   This argument takes an array to match documents within its items.

### Examples

Retrieve all users that are from the country BD, CA, SE or NA.

```php
$users = $userQueryBuilder
  ->in("country", ["BD", "CA", "SE", "NA"])
  ->getQuery()
  ->fetch();
```

Retrieve all users that are from country BD, CA, SE, NA and are at the age of 18, 20, 23 or 30.

```php
$users = $userQueryBuilder
  ->in("country", ["BD", "CA", "SE", "NA"])
  ->in("age", [18, 20, 23, 30])
  ->getQuery()
  ->fetch();
```

## notIn()

notIn(...) works as the opposite of in() method.

It will filter out all documents that has particular data items from the given array.
SleekDB supports multiple notIn as object chain for different fields.

The notIn() method takes two arguments as in() method, those are:

```php
function notIn(string $fieldName, array $values = []): QueryBuilder
```

### Parameters

1. # $fieldName: string

   The field name argument is the property that we want to check in our data object.

   You can also provide have nested properties separated with a dot ".".

2. # $values: array

   This argument takes an array to match documents within its items.

### Examples

Retrieve all users that are not from the coutry IN, KE or OP.

```php
$users = $userQueryBuilder
  ->notIn("country", ["IN", "KE", "OP"])
  ->getQuery()
  ->fetch();
```

Retrieve all users that are not from the country IN, KE or OP and do not have products.totalSaved 100, 150 or 200.

```php
$users = $userQueryBuilder
  ->notIn("country", ["IN", "KE", "OP"])
  ->notIn("products.totalSaved", [100, 150, 200])
  ->getQuery()
  ->fetch();
```

## select()

With select(...) you can specify specific fields to output, like after the SELECT keyword in SQL. SleekDB supports multiple `select()` as object chain for multiple fields.

```php
function select(array $fieldNames): QueryBuilder
```

### Parameters

1. # $fieldNames: array
   Specify specific fields to output.

### Examples

Retrieve just the name of all users.

```php
$users = $userQueryBuilder
  ->select(['name'])
  ->getQuery()
  ->fetch();
// output: [["_id" => 1, "name" => "Max"], ["_id" => 2, "name" => "Hasan"]]
```

### except()

`except(...)` works as the opposite of `select()` method. Use it when you don't want a property in the result. For example, if you don't want the password field in your returned data set.

```php
function except(array $fieldNames): QueryBuilder
```

1. # $fieldNames: array
   Specify specific fields to exclude from the output.

### Examples

Retrieve all information of an user except its \_id and name.

```php
$users = $userQueryBuilder
  ->except(["_id", "name"])
  ->getQuery()
  ->fetch();
// output: [["age": 28], ["age": 18]]
```

## distinct()

The distinct method is used to retrieve unique values from the store. It will remove all the duplicate documents while fetching data from a store.

```php
distinct( array|string $fields ): QueryBuilder;
```

### Parameters

1. # $fields: array|string
   Specify one or multiple fields you want to be distinct.

### Examples

Retrieve all users, but just the first user if there is another one with the same name.

```php
// providing a string
$distinctUsers = $userQueryBuilder
  ->distinct("name")
  ->getQuery()
  ->fetch();

// providing an array
$distinctUsers = $userQueryBuilder
  ->distinct(["name"])
  ->getQuery()
  ->fetch();
```

## skip()

Skip works as the OFFSET clause of SQL. You can use this to skip a specific amount of documents.

```php
function skip(int $skip = 0): QueryBuilder
```

### Parameters

1. # $skip: int
   The value how many documents should be skipped.

### Examples

Retrieve all users except the first 10 found.

```php
$users = $userQueryBuilder
  ->skip(10)
  ->getQuery()
  ->fetch();
```

## limit()

Works as the LIMIT clause of SQL. You can use this to limit the results to a specific amount.

```php
function limit($limit = 0): QueryBuilder
```

### Parameters

1. $limit: int
   Limit the amount of values in the result set. Has to be greater than 0.

### Examples

Retrieve just the first ten users.

```php
$users = $userQueryBuilder
  ->limit(10)
  ->getQuery()
  ->fetch();
```

## orderBy()

Works as the ORDER BY clause of SQL. With this method you can sort the result. You can use this method to sort the result by one or multiple fields.

```php
function orderBy( array $criteria): QueryBuilder
```

### Parameters

1. # $criteria: array

- [ $fieldName => $order [, ...] ]
  - # $fieldName: string
    Name of the field that will be used to sort the result.
  - # $order: string
    Either `desc` for a descending sort or `asc` for a ascending sort.

### Examples

Retrieve all users sorted by their name.

```php
$users = $userQueryBuilder
  ->orderBy(["name" => "asc"])
  ->getQuery()
  ->fetch();
```

#### Result

```
[["_id" => 13, "name" => "Anton"], ["_id" => 2, "name" => "Berta"], ...]
```

Retrieve all users sorted by their name and age.

```php
$users = $userQueryBuilder
  ->orderBy(["name" => "asc", "age" => "asc"])
  ->getQuery()
  ->fetch();
```

#### Result

```
[
  ["_id" => 13, "name" => "Anton", "age" => 20],
  ["_id" => 4, "name" => "Aragon", "age" => 16], 
  ["_id" => 2, "name" => "Aragon", "age" => 17], 
  ...
]
```

## search()

Do a fulltext like search against one or multiple fields.

```php
function search(string|array $fields, string $keyword): QueryBuilder
```

### Parameters

1. # $fields: array|string
   One or multiple fields that will be searched.
2. $keyword: string
   Value that will be searched by.

### Examples

Find all articles that include the word "SleekDB" in their description.

```php
$articles = $articleQueryBuilder
  ->search("content", "SleekDB")
  ->getQuery()
  ->fetch();
```

## Cache management

The QueryBuilder provides the `useCache`, `disableCache` and `regenerateCache` methods to manage caching on a query by query base.

Please visit the <a class="gotoblock" href="/#/cache-management">Cache Management</a> page for more details.

## join()

This method is used to join two or multiple stores together.

For more details please visit the <a class="gotoblock" href="/#/join-stores">Join Stores</a> page.

```php
function join(callable $joinedStore, string $dataPropertyName): QueryBuilder
```
