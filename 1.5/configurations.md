<!--METADATA
{
    "title": "Configurations",
    "url": "configurations",
    "icon": "construct"
}
!METADATA-->

# Configurations

At this moment SleekDB only allows two configurations, which are "auto_cache" and "timeout". Configurations should be passed as an array in the second parameter while instantiating SleekDB, store's created from this object will follow the configurations we have provided.

## Using Custom Configuration

You should pass the configurations array in the second parameter, example:

```php
$newsStore = \SleekDB\SleekDB::store('news', $dataDir, [
  'auto_cache' => true,
  'timeout' => 120
]);
```

Let's talk about what this configurations do.

### auto_cache

The `auto_cache` is set to `true` by default!

This tells SleekDB to cache the data of an unique database query and later re-use the cache for the same query.
To keep the cached data synced with new data, SleekDB will delete all cache when we insert any new data to a store.

To disable it update the value of `auto_cache` to `false` in the config array.

Note that you can manually manage cache data with methods that SleekDB provides. Available caching method's are: `makeCache()`, `useCache()`, `deleteCache()` and `deleteAllCache()`

### timeOut

Set timeout value, default value is `120` second.
