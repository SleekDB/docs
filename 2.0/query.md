<!--METADATA
{
    "title": "Query",
    "url": "query",
    "icon": "flash"
}
!METADATA-->

# query


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
