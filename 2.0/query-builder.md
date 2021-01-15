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
function where(array $criteria): QueryBuilder;
```

### Parameters

1. # $criteria
One or multiple where conditions
  * [$fieldName, $condition, $value]
  * [ [$fieldName, $condition, $value], [$fieldName, $condition, $value], ... ]
    * # $fieldName

      The field name argument is the property that we want to check in our data object.

      As our data object is basically a JSON document so it could have nested properties.

      To target nested properties we use a single dot between the property/field name.

      **Example:** From our above users object if we want to target the "country" property of a user, then we would pass `location.country` in this argument, because "location" is the parent property of the "country" property in our data object.

    * # $condition

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
      Data to be used as against the property value of the JSON documents.

### Example of using where() to filter data

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

Retrieve all users that have products.totalSaved > 10 AND products.totalBought > 20.

```php
// inline & using where method multiple times
$users = $usersQueryBuilder
  ->where( ["products.totalSaved", ">", 10] )
  ->where( ["products.totalBought", ">", 20] )
  ->getQuery()
  ->fetch();

// inline & using where method once
$users = $usersQueryBuilder
  ->where([ ["products.totalSaved", ">", 10], ["products.totalBought", ">", 20] ])
  ->getQuery()
  ->fetch();

// retrieve QueryBuilder
$usersQueryBuilder = $usersStore->createQueryBuilder()

// prepare query
$usersQueryBuilder->where([ ["products.totalSaved", ">", 10], ["products.totalBought", ">", 20] ]);

// execute query
$users = $usersQueryBuilder->getQuery()->fetch();

```

### orWhere()

`orWhere(...)` works as the OR condition of SQL. SleekDB supports multiple orWhere as object chain.

The `orWhere()` method takes three arguments same as `where()` condition, those are:

```php
orWhere( string $fieldName, string $condition, mixed $value ): QueryBuilder;
```

You can also specify one or multiple where conditions within one or-where condition as an array. These where conditions then are connected with an "and".

```php
orWhere( [[string $fieldName, string $condition, mixed $value], [string $fieldName, string $condition, $value], ...] );
```

**Example:**

```php
$users = $userStore
  ->getQueryBuilder()
  ->where( "products.totalSaved", ">", 10 )
  ->where( "products.totalBought", ">", 20 )
  ->getQuery()
  ->fetch();
```

**Multiple orWhere clause example:**

```php
$users = $userStore
  ->getQueryBuilder()
  ->where( "products.totalSaved", ">", 10 )
  ->where( "products.totalBought", ">", 20 )
  ->orWhere( "products.shipped", "=", 1 )
  ->getQuery()
  ->fetch();
```

**One orWhere with multiple where clause, which are connected with and example:**

```php
$users = $userStore
  ->getQueryBuilder()
  ->where( "products.totalSaved", ">", 10 )
  ->orWhere(
    [
      [ "products.totalBought", ">", 20 ],
      [ "products.shipped", "=", 1 ]
    ]
  )
  ->getQuery()
  ->fetch();
```

### in()

in(...) works as the IN clause of SQL. SleekDB supports multiple IN as object chain for different fields.

The in() method takes two arguments, those are:

```php
in(string $fieldName, array $values = []): QueryBuilder;
```

1. # $fieldName

   The field name argument is the property that we want to check in our data object.

   As our data object is basically a JSON document so it could have nested properties.

2. # $values

   This argument takes an array to match documents within its items.

**Example:**

```php
$users = $userStore
  ->getQueryBuilder()
  ->in("country", ["BD", "CA", "SE", "NA"])
  ->getQuery()
  ->fetch();
```

### notIn()

notIn(...) works as the opposite of in() method.

It will filter out all documents that has particular data items from the given array.
SleekDB supports multiple notIn as object chain for different fields.

The notIn() method takes two arguments as in() method, those are:

```php
notIn(string $fieldName, array $values = []): QueryBuilder;
```

**Example:**

```php
$users = $userStore
  ->getQueryBuilder()
  ->notIn("country", ["IN", "KE", "OP"])
  ->getQuery()
  ->fetch();
```

**Multiple notIn clause example with nested properties:**

```php
$users = $userStore
  ->getQueryBuilder()
  ->notIn("country", ["IN", "KE", "OP"])
  ->notIn("products.totalSaved", [100, 150, 200])
  ->getQuery()
  ->fetch();
```

### select()

With select(...) you can specify specific fields that to output, like after the SELECT keyword in SQL. SleekDB supports multiple `select()` as object chain for multiple fields.

The select() method takes one argument:

```php
select(array $fieldNames): QueryBuilder
```

**Example:**

```php
$users = $userStore
  ->getQueryBuilder()
  ->select(['name'])
  ->getQuery()
  ->fetch();
// output: [["_id": 1, "name": "Max"], ["_id": 2, "name": "Hasan"]]
```

### except()

`except(...)` works as the opposite of `select()` method. Use it when you don't want a property with the data returned. For example, if you don't want the password field in your returned data set then use this method.

The except() method takes one argument:

```php
except(array $fieldNames): QueryBuilder
```

**Example:**

```php
$users = $userStore
  ->getQueryBuilder()
  ->except(["_id", "name"])
  ->getQuery()
  ->fetch();
// output: [["age": 28], ["age": 18]]
```

### first()

Returns the very first document discovered. It is more efficient than `fetch` but one caveat is that the `orderBy` will not work when you this method to get the very first item.

```php
first(): QueryBuilder
```

**Example:**

```php
$users = $userStore
  ->getQueryBuilder()
  ->where("email", "=", "foo@bar.com")
  ->getQuery()
  ->first();
```

### exists()

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

### distinct()

The distinct method is used to retrieve unique values from the store. It will remove all the duplicate documents while fetching data from a store.

The distinct() method takes only one argument,

```php
distinct( array|string $fields ): QueryBuilder;
```

**Example:**

```php
$distinctUsers = $userStore
  ->getQueryBuilder()
  ->distinct(["name"])
  ->getQuery()
  ->fetch()
```
