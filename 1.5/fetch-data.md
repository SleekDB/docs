<!--METADATA
{
    "title": "Fetch Data",
    "url": "fetch-data",
    "icon": "book"
}
!METADATA-->

# Fetch Data

To get data from the store we use the `fetch()` method. Example:

```php
$usersDB->fetch();
```

The above command would query into the "users" store to fetch all the data.

## Apply Filters and Conditions

### where()

To filter data we use the where() method.

The where() method takes three arguments, those are:

```php
where( $fieldName, $condition, $value );
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

3. # $value
   Data to be used as against the property value of the JSON documents.

### Example of using where() to filter data

To only get the user whose country is equal to "England" we would query like this:

```php
$user = $usersDB->where( 'name', '=', 'Joshua Edwards' )->fetch();
```

You can use multiple `where()` conditions. Example:

```php
$user = $usersDB
    ->where( 'products.totalSaved', '>', 10 )
    ->where( 'products.totalBought', '>', 20 )
    ->fetch();
```

### orWhere()

`orWhere(...)` works as the OR condition of SQL. SleekDB supports multiple orWhere as object chain.

The `orWhere()` method takes three arguments same as `where()` condition, those are:

```php
orWhere( $fieldName, $condition, $value );
```

**Example:**

```php
$user = $usersDB
    ->where( 'products.totalSaved', '>', 10 )
    ->orWhere( 'products.totalBought', '>', 20 )
    ->fetch();
```

**Multiple orWhere clause example:**

```php
$user = $usersDB->where( 'products.totalSaved', '>', 10 )
    ->orWhere( 'products.totalBought', '>', 20 )
    ->orWhere( 'products.shipped', '=', 1 )
    ->fetch();
```

### in()

in(...) works as the IN clause of SQL. SleekDB supports multiple IN as object chain for different fields.

The in() method takes two arguments, those are:

```php
in($fieldName = '', $values = []);
```

1. # $fieldName

   The field name argument is the property that we want to check in our data object.

   As our data object is basically a JSON document so it could have nested properties.

2. # $values

   This argument takes an array to match documents within its items.

**Example:**

```php
$user = $usersDB->in('country', ['BD', 'CA', 'SE', 'NA'])->fetch();
```

### notIn()

notIn(...) works as the opposite of in() method.

It will filter out all documents that has particular data items from the given array.
SleekDB supports multiple notIn as object chain for different fields.

The notIn() method takes two arguments as in() method, those are:

```php
notIn($fieldName = '', $values = []);
```

**Example:**

```php
$user = $usersDB->notIn('country', ['IN', 'KE', 'OP'])->fetch();
```

**Multiple notIn clause example with nested properties:**

```php
$user = $usersDB
    ->notIn('country', ['IN', 'KE', 'OP'])
    ->notIn('products.totalSaved', [100, 150, 200])
    ->fetch();
```
