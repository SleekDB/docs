<!--METADATA
{
    "title": "Delete Cache",
    "url": "delete-cache",
    "icon": "refresh-circle"
}
!METADATA-->

# Delete Cache

To delete the cache of a query we would use the deleteCache() method. Example,

```php
$user = $usersDB
    ->search( 'bio', 'SleekDB' )
    ->orderBy( 'desc', 'rank' )
    ->skip( 80 )
    ->limit( 20 )
    ->deleteCache();
```

Note that if you need to get the data please add the fetch() method.
