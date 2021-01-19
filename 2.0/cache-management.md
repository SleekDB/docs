<!--METADATA
{
    "title": "Cache Management",
    "url": "cache-management",
    "icon": "copy"
}
!METADATA-->

# Cache Management

The `QueryBuilder` class does provide a couple of methods to `manage caching on a query by query bases`.

By default caching is enabled store wide and does not need to be enabled on a query by query bases. For more information please look into the store <a class="gotoblock" href="#/configurations">configurations</a>.

Create a QueryBuilder:

```php
$userQueryBuilder = $userStore->createQueryBuilder();
```

## Summary

- useCache
- disableCache
- regenerateCache

## useCache()

With the useCache method you can `activate caching` and define the `cache lifetime` on a query by query bases.

```php
function useCache(int $lifetime = null): QueryBuilder
```

### Parameters

1. # $lifetime: int|null
  The lifetime of the cache.
  * `null` (Default)<br/>
    Cache will not have a lifetime and will be regenerated on every update, delete and insert.
  * `int`<br/>
    Cache lifetime in seconds. `0` means infinite lifetime.

### Examples

Retrieve and cache 20 users that are active.

The results in this example will use caching untill any documents gets updated/ deleted or inserted.

```php
// fetch and insert in cache
$users = $userQueryBuilder
    ->where( ['active', '=', 1] )
    ->limit( 20 )
    ->useCache() // Use caching with lifetime = null
    ->getQuery()
    ->fetch();

// retrieve from cache
$users = $userQueryBuilder
    ->where( ['active', '=', 1] )
    ->limit( 20 )
    ->useCache()
    ->getQuery()
    ->fetch();

// insert a new user (cache with no lifetime will be deleted)
$newUser = ["name" => "Max", "active" => 0];
$userStore->insert($newUser);

// fetch and insert in cache
$users = $userQueryBuilder
    ->where( ['active', '=', 1] )
    ->limit( 20 )
    ->useCache()
    ->getQuery()
    ->fetch();
```

Retrieve and cache result for 2 minutes, 20 users that are active.

```php
// fetch and insert in cache
$users = $userQueryBuilder
    ->where( ['active', '=', 1] )
    ->limit( 20 )
    ->useCache(120) // 2 minutes = 120 seconds
    ->getQuery()
    ->fetch();
```

Retrieve and cache result forever, 20 users that are active.

```php
// fetch and insert in cache
$users = $userQueryBuilder
    ->where( ['active', '=', 1] )
    ->limit( 20 )
    ->useCache(0) // 0 means infinite caching
    ->getQuery()
    ->fetch();
```

## disableCache()

Disable the build in caching solution on a query by query bases.<br/>
By default caching is enabled store wide. If you want to disable caching store instead of disabling it on a query by query bases visit the store <a class="gotoblock" href="#/configurations">configurations</a> page.

```php
function disableCache(): QueryBuilder
```

### Example

Retrieve 20 users that are active and do not use caching.

```php
// fetch and insert in cache
$users = $userQueryBuilder
    ->where( ['active', '=', 1] )
    ->limit( 20 )
    ->disableCache()
    ->getQuery()
    ->fetch();
```

## regenerateCache()

Regenerate the cache of a query regardless of its lifetime.

```php
function regenerateCache(): QueryBuilder
```

### Examples

```php
// cache with infinite lifetime
$users = $userQueryBuilder
    ->where( ['active', '=', 1] )
    ->limit( 20 )
    ->useCache(0)
    ->getQuery()
    ->fetch();

// returns documents not from cache and caches results for 20 seconds
$users = $userQueryBuilder
    ->where( ['active', '=', 1] )
    ->limit( 20 )
    ->useCache(20)
    ->regenerateCache()
    ->getQuery()
    ->fetch();
```