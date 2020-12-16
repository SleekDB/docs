<!--METADATA
{
    "title": "Re-generate Cache",
    "url": "regenerate-cache",
    "icon": "sync"
}
!METADATA-->

# Re-generate Cache

To re-generate the cache for a query we would use the `makeCache()` method.

Its more like the `useCache()` method but the only difference is that instead of looking for existing cache data it would replace the old cache by generating a new cache from fresh data fetched. Example,

```php
$user = $usersDB
    ->search( 'bio', 'SleekDB' )
    ->orderBy( 'desc', 'rank' )
    ->skip( 80 )
    ->limit( 20 )
    ->makeCache() // Re-generate the cache data.
    ->fetch();
```
