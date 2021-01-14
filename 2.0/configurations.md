<!--METADATA
{
    "title": "Configurations",
    "url": "configurations",
    "icon": "construct"
}
!METADATA-->

# Configurations

SleekDB allows few configuration options, which are

- auto_cache
- cache_lifetime
- timeout

Configurations should be passed as an array in the third parameter while instantiating a SleekDB Store. This configurations are store wide available and will be used on every query by default.

## Using Custom Configuration

You should pass the configurations array in the third parameter, example:

```php
$newsStore = new \SleekDB\Store("news", $dataDir, [
  "auto_cache" => true,
  "cache_lifetime" => null,
  "timeout" => 120
]);
```

Let's get familiarized with available configuration properties.

### auto_cache

The `auto_cache` is set to `true` by default!

This tells SleekDB to use the build in caching system.

To disable build in caching set `auto_cache` to `false` in the config array.

Note that you can manually manage caching on a query by query base with methods that SleekDB provides.
Available caching method's are:

- `QueryBuilder->regenerateCache()`
- `QueryBuilder->useCache()`
- `QueryBuilder->disableCache()`
- `QueryBuilder->deleteCache()`
- `QueryBuilder->deleteAllCache()`

### cache_lifetime

The `cache_lifetime` is set to `null` by default!

Can be an `int >= 0`, that specifies the lifetime in seconds, or `null` to define that there is no lifetime.

This specifies the default cache time to live store wide.

If set to null the cache files made with that store instance will have no lifetime and will be regenerated on every insert/update/delete operations.

> `0` means infinite.

Note that you can specify the cache lifetime on a query by query base by using the useCache method of the QueryBuilder and passing a lifetime.

- `QueryBuilder->useCache($lifetime)`

> Note: You will find more details on caching at <a class="gotoblock" href="/#/cache-management">Cache Management</a>

### timeOut

Set timeout value, default value is `120` second.
