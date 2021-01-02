<!--METADATA
{
    "title": "Fetch Data",
    "url": "fetch-data",
    "icon": "book"
}
!METADATA-->

# Fetch Data

To get data from the store we use the `fetch()`, `first()` or `exists()` method the Query Object.

Example:

```php
$usersStore->getQueryBuilder()->getQuery()->fetch();
```

The above command would query into the "users" store to fetch all the data.

## Apply Filters and Conditions

### where()

To filter data we use the where() method of the QueryBuilder object.

The where() method takes three arguments, those are:

```php
where( string $fieldName, string $condition, mixed $value ): QueryBuilder;
```

1. # $fieldName

   The field name argument is the property that we want to check in our data object.

   As our data object is basically a JSON document so it could have nested properties.

   To target nested properties we use a single dot between the property/field name.

   **Example:** From our above users object if we want to target the "country" property of a user, then we would pass `location.country` in this argument, because "location" is the parent property of the "country" property in our data object.

2. # $condition

   To apply the comparison filters we use this argument.

   Allowed comparisonal conditions are:

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

3. # $value
   Data to be used as against the property value of the JSON documents.

### Example of using where() to filter data

To only get the user whose country is equal to "England" we would query like this:

```php
$users = $usersStore->getQueryBuilder()->where( 'name', '=', 'Joshua Edwards' )->getQuery()->fetch();
```

You can use multiple `where()` conditions.

Example:

```php
$users = $usersStore->getQueryBuilder()
    ->where( 'products.totalSaved', '>', 10 )
    ->where( 'products.totalBought', '>', 20 )
    ->getQuery()
    ->fetch();
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
$users = $usersStore->getQueryBuilder()
    ->where( 'products.totalSaved', '>', 10 )
    ->orWhere( 'products.totalBought', '>', 20 )
    ->getQuery()
    ->fetch();
```

**Multiple orWhere clause example:**

```php
$users = $usersStore->getQueryBuilder()
    ->where( 'products.totalSaved', '>', 10 )
    ->orWhere( 'products.totalBought', '>', 20 )
    ->orWhere( 'products.shipped', '=', 1 )
    ->getQuery()
    ->fetch();
```

**One orWhere with multiple where clause, which are connected with and example:**

```php
$users = $usersStore->getQueryBuilder()
    ->where( 'products.totalSaved', '>', 10 )
    ->orWhere( [ ['products.totalBought', '>', 20], [ 'products.shipped', '=', 1 ] ] )
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
$users = $usersStore->getQueryBuilder()->in('country', ['BD', 'CA', 'SE', 'NA'])->getQuery()->fetch();
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
$users = $usersStore->getQueryBuilder()->notIn('country', ['IN', 'KE', 'OP'])->getQuery()->fetch();
```

**Multiple notIn clause example with nested properties:**

```php
$users = $usersStore->getQueryBuilder()
    ->notIn('country', ['IN', 'KE', 'OP'])
    ->notIn('products.totalSaved', [100, 150, 200])
    ->getQuery()
    ->fetch();
```

### select()

With select(...) you can specify specific fields that to output, like after the SELECT keyword in SQL. SleekDB supports multiple select() as object chain for multiple fields.

The select() method takes one argument:

```php
select(array $fieldNames): QueryBuilder
```

**Example:**

```php
$users = $usersStore->getQueryBuilder()->select(['name'])->getQuery()->fetch();
// output: [["_id": 1, "name": "Max"], ["_id": 2, "name": "Hasan"]]
```

### except()

except(...) works as the opposite of select() method.

The except() method takes one argument:

```php
except(array $fieldNames): QueryBuilder
```

**Example:**

```php
$users = $usersStore->getQueryBuilder()->except(['_id', 'name'])->getQuery()->fetch();
// output: [["age": 28], ["age": 18]]
```
