<!--METADATA
{
    "title": "Cache Management",
    "url": "cache-management",
    "icon": "copy"
}
!METADATA-->

# Cache Management

The `useCache()` method would return the data from the cache storage, if cache dosent exists then it would fetch the result then creates the cache for later use and return the data. Example,

```php
$user = $usersDB
    ->where( 'active', '=', 1 )
    ->where( 'location.country', '=', 'United States' )
    ->search( 'bio', 'PHP developer' )
    ->search( 'bio', 'SleekDB' )
    ->orderBy( 'desc', 'rank' )
    ->limit( 20 )
    ->useCache() // Use the cache data.
    ->fetch();
```
