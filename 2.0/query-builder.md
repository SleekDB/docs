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
$usersQueryBuilder = $usersStore->createQueryBuilder();
```

## getQuery()

With the `getQuery` method of the QueryBuilder class you can **`retrieve the Query object to execute the query`**.

```php
function getQuery(): Query
```

The most important methods of the Query class to execute a query are:
  * # fetch()
    Retrieve multiple documents
  * # first()
    Retrieve first found document
  * # exists()
    Check if a document for given query exist
  * # delete()
    Delete all documents that are found with given query
  * # update()
    Update all documents that are found with given query

For more details on query execution please visit the <a class="gotoblock" href="#/query">Query</a> page.

## where()

To filter data we use the where() method of the QueryBuilder object.<br/>If you provide multiple conditions they are connected with an `AND`.

```php
function where(array $criteria): QueryBuilder
```

### Parameters

1. # $criteria
One or multiple where conditions
  * [$fieldName, $condition, $value]
  * [ [$fieldName, $condition, $value], [$fieldName, $condition, $value], ... ]
    * # $fieldName: string

      The field name argument is the property that we want to check in our data object.

      As our data object is basically a JSON document so it could have nested properties.

      To target nested properties we use a single dot between the property/field name.

      **Example:** From our above users object if we want to target the "country" property of a user, then we would pass `location.country` in this argument, because "location" is the parent property of the "country" property in our data object.

    * # $condition: string

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

    * # $value
      Data that will be checked against the property value of the JSON documents.

### Examples

To only get the user whose country is equal to "England" we would query like this:

```php
// inline
$users = $usersStore
  ->createQueryBuilder()
  ->where( "name", "=", "Joshua Edwards" )
  ->getQuery()
  ->fetch();

// creating the QueryBuilder
$usersQueryBuilder = $usersStore->createQueryBuilder();

// preparing the query with the QueryBuilder
$usersQueryBuilder->where( "name", "=", "Joshua Edwards" );

// executing the query
$users = $usersQueryBuilder->getQuery()->fetch();
```

You can also use multiple `where` conditions.

Retrieve all users that have `products.totalSaved > 10 AND products.totalBought > 20`.

```php
// inline & using where method multiple times
$users = $usersQueryBuilder
  ->where( ["products.totalSaved", ">", 10] )
  ->where( ["products.totalBought", ">", 20] )
  ->getQuery()
  ->fetch();

// inline & using where method once
$users = $usersQueryBuilder
  ->where(
    [ 
      ["products.totalSaved", ">", 10], 
      ["products.totalBought", ">", 20] 
    ]
  )
  ->getQuery()
  ->fetch();

// retrieve QueryBuilder
$usersQueryBuilder = $usersStore->createQueryBuilder()

// prepare query
$usersQueryBuilder->where(
  [ 
    ["products.totalSaved", ">", 10], 
    ["products.totalBought", ">", 20] 
  ]
);

// execute query
$users = $usersQueryBuilder->getQuery()->fetch();

```

## orWhere()

`orWhere(...)` works as the OR condition of SQL. SleekDB supports multiple orWhere as object chain.<br/>If you provide multiple conditions they are connected with an `AND`.

```php
function orWhere(array $criteria): QueryBuilder
```

### Properties

1. # $criteria
One or multiple where conditions
  * [$fieldName, $condition, $value]
  * [ [$fieldName, $condition, $value], [$fieldName, $condition, $value], ... ]
    * # $fieldName: string

      The field name argument is the property that we want to check in our data object.

      As our data object is basically a JSON document so it could have nested properties.

      To target nested properties we use a single dot between the property/field name.

      **Example:** From our above users object if we want to target the "country" property of a user, then we would pass `location.country` in this argument, because "location" is the parent property of the "country" property in our data object.

    * # $condition: string

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

    * # $value
      Data that will be checked against the property value of the JSON documents.

### Examples

Retrieve all users that have `(products.totalSaved > 10 AND products.totalBought > 20) OR products.shipped = 1`

```php
$users = $usersQueryBuilder
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
$users = $usersQueryBuilder
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
$users = $usersQueryBuilder
  ->in("country", ["BD", "CA", "SE", "NA"])
  ->getQuery()
  ->fetch();
```

Retrieve all users that are from country BD, CA, SE, NA and are at the age of 18, 20, 23 or 30.

```php
$users = $usersQueryBuilder
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
$users = $userStore
  ->getQueryBuilder()
  ->notIn("country", ["IN", "KE", "OP"])
  ->getQuery()
  ->fetch();
```

Retrieve all users that are not from the country IN, KE or OP and do not have products.totalSaved 100, 150 or 200.

```php
$users = $userStore
  ->getQueryBuilder()
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
$users = $userStore
  ->getQueryBuilder()
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

Retrieve all information of an user except its _id and name.

```php
$users = $userStore
  ->getQueryBuilder()
  ->except(["_id", "name"])
  ->getQuery()
  ->fetch();
// output: [["age": 28], ["age": 18]]
```

---

## first()

Returns the very first document discovered. It is more efficient than `fetch` but one caveat is that the `orderBy` will not work when using this method to get the very first item.

```php
function first(): QueryBuilder
```

### Examples

```php
$users = $usersQuerybuilder
  ->where("email", "=", "foo@bar.com")
  ->getQuery()
  ->first();
```

## exists()

Returns boolean `true` if data exists, `false` if data does not exists. It is more efficient than using `fetch` to check if some data exists or not. For example, you may use exists method to check if a username or email address is unique or not.

```php
exists(): QueryBuilder
```

**Example:**

```php
$usernameUniqueness = $userStore
  ->getQueryBuilder()
  ->where("username", "=", "foobar")
  ->getQuery()
  ->exists();
```

---

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
$distinctUsers = $usersQueryBuilder
  ->distinct("name")
  ->getQuery()
  ->fetch();

// providing an array
$distinctUsers = $usersQueryBuilder
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
$users = $usersQueryBuilder
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
$users = $usersQueryBuilder
  ->limit(10)
  ->getQuery()
  ->fetch();
```

## orderBy()

Works as the ORDER BY clause of SQL. With this method you can sort the result. At the moment the result can just be sorted by one field.

```php
function orderBy( array $criteria): QueryBuilder
```

### Parameters

1. # $criteria: array
  * [$fieldName => $order]
    * # $fieldName: string
      Name of the field that will be used to sort the result.
    * # $order: string
      Either `desc` for a descending sort or `asc` for a ascending sort.

### Examples

Retrieve all users sorted by their name.

```php
$users = $usersQueryBuilder
  ->orderBy(["name" => "asc"])
  ->getQuery()
  ->fetch();
```

#### Result

```
[["_id" => 13, "name" => "Anton"], ["_id" => 2, "name" => "Berta"], ...]
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
$articles = $articlesQueryBuilder
  ->search(["content"], "SleekDB")
  ->getQuery()
  ->fetch();
```