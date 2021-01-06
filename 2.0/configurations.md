<!--METADATA
{
    "title": "Configurations",
    "url": "configurations",
    "icon": "construct"
}
!METADATA-->

# Configurations

SleekDB allows three configuration options, which are "auto_cache", "cache_lifetime" and "timeout". Configurations should be passed as an array in the third parameter while instantiating a SleekDB Store. This configuration is store wide and will be used on every query.

## Using Custom Configuration

You should pass the configurations array in the third parameter, example:

```php
$newsStore = new \SleekDB\Store('news', $dataDir, [
  'auto_cache' => true,
  'cache_lifetime' => null,
  'timeout' => 120
]);
```

Let's talk about what this configurations do.

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
- `deleteAllCache()`

### cache_lifetime

The `cache_lifetime` is set to `null` by default!

Can be an `int >= 0`, that specifies the lifetime in seconds, or `null` to define that there is no lifetime.

This specifies the default cache time to live store wide.

If set to null the cache files made with that store instance will have no lifetime and will be regenerated on every insert/update/delete.

0 means infinite.

Note that you can specify the cache lifetime on a query by query base by using the useCache method of the QueryBuilder and passing a lifetime.

`QueryBuilder->useCache($lifetime)`

### timeOut

Set timeout value, default value is `120` second.
