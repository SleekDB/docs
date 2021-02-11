<!--METADATA
{
    "title": "Configurations",
    "url": "configurations",
    "icon": "construct"
}
!METADATA-->

# Configurations

SleekDB allows a few configuration options, which are

- <a class="gotoblock" href="#/configurations#auto_cache">auto_cache</a>
- <a class="gotoblock" href="#/configurations#cache_lifetime">cache_lifetime
- <a class="gotoblock" href="#/configurations#timeout">timeout</a>
- <a class="gotoblock" href="#/configurations#primary_key">primary_key</a>

They are store wide, which means they will be used on every query, if not changed on a query by query base.

## Using Custom Configuration

You can pass the configurations array as a third parameter when initializing the Store object:

```php
$configuration = [
  "auto_cache" => true,
  "cache_lifetime" => null,
  "timeout" => 120,
  "primary_key" => "_id"
];
$newsStore = new \SleekDB\Store("news", $dataDir, $configuration);
```

Let's get familiar with the available configuration options.

### auto_cache {#configurations-auto_cache}

The `auto_cache` is set to `true` by default!

This tells SleekDB to use the build in caching system.

To disable build in caching set `auto_cache` to `false` in the config array.

Note that you can manually manage caching on a query by query base with methods that SleekDB provides.
Available caching method's are:

- `QueryBuilder->regenerateCache()`
- `QueryBuilder->useCache()`
- `QueryBuilder->disableCache()`

### cache_lifetime {#configurations-cache_lifetime}

The `cache_lifetime` is set to `null` by default!

Can be an `int >= 0`, that specifies the lifetime in seconds, or `null` to define that there is no lifetime.

This specifies the default cache time to live store wide.

If set to null the cache files made with that store instance will have no lifetime and will be regenerated on every insert/update/delete operation.

> `0` means infinite lifetime.

Note that you can specify the cache lifetime on a query by query base by using the useCache method of the QueryBuilder and pass a lifetime.

- `QueryBuilder->useCache($lifetime)`

> Note: You will find more details on caching at <a class="gotoblock" href="/#/cache-management">Cache Management</a>

### timeout {#configurations-timeout}

Set timeout value. Default value is `120` seconds.

### primary_key {#configurations-primary_key}

Set the primary key into any string you want.<br/>
SleekDB will then use that key instead of the default `_id` key.
