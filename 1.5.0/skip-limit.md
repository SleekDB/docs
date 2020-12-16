<!--METADATA
{
    "title": "Skip and Limit Data",
    "url": "skip-limit",
    "icon": "git-pull-request"
}
!METADATA-->

# Skip and Limit

To skip a set of record we will use the `skip()` method. Example:

```php
// Skip the first 5 users.
$users = $usersDB->skip( 5 )->fetch();
```

To limit a set of record we will use the `limit()` method. Example:

```php
// Fetch only 5 users.
$users = $usersDB->limit( 5 )->fetch();
```

## Query Offset

To obtain the query offset feature we can chain skip() and limit() into one query. Example:

```php
$users = $usersDB
    ->where( 'age', '>=', 18 )
    ->skip( 15 )
    ->limit( 5 )
    ->fetch();
```

The above query will skip first 15 records and will limit to next 5 records of data objects. This way we can also perform paginate.
