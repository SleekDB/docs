<!--METADATA
{
    "title": "Cache",
    "url": "cache",
    "icon": "logo-buffer"
}
!METADATA-->

# Cache

With the Cache class you can control Caching in a deeper way.

That said, this class is **`mainly for internal use`** and normally that kind of deep cache control is not needed.

> ⚠️️ Be careful when using the `Cache` object.

To retrieve the Cache object use the `getCache` method of the `Query` class.

```php
$cache = $userQuery->getCache();
```

## Summary

- <a class="gotoblock" href="#/cache#getToken">getToken</a>
- <a class="gotoblock" href="#/cache#delete">delete</a>
- <a class="gotoblock" href="#/cache#deleteAll">deleteAll</a>
- <a class="gotoblock" href="#/cache#deleteAllWithNoLifetime">deleteAllWithNoLifetime</a>
- <a class="gotoblock" href="#/cache#set">set</a>
- <a class="gotoblock" href="#/cache#get">get</a>
- <a class="gotoblock" href="#/cache#getCachePath">getCachePath</a>
- <a class="gotoblock" href="#/cache#setLifetime">setLifetime</a>
- <a class="gotoblock" href="#/cache#getLifetime">getLifetime</a>

## getToken() {#cache-getToken}

Returns the unique token for the current query, that will be used to save and retrieve a cache file.

```php
function getToken(): string
```

### Return value

The unique token for the current query as a `string`.

## delete() {#cache-delete}

Deletes the cache file for the current query.

```php
function delete()
```

## deleteAll() {#cache-deleteAll}

Delete all cache files of current store.

```php
function deleteAll()
```

## deleteAllWithNoLifetime() {#cache-deleteAllWithNoLifetime}

Delete all cache files that have no lifetime (null) of current store.

```php
function deleteAllWithNoLifetime()
```

## set() {#cache-set}

Set and cache the content for the current query / token.

```php
function set(array $content)
```

### Parameters

1. # $content: array
  The content that will be cached.

## get() {#cache-get}

Retrieve the content of the cache file for the current query / token.

```php
function get(): array|null
```

### Return value

The content as an `array` or `null` if no cache file found.

## getCachePath() {#cache-getCachePath}

Returns the path to the cache directory.

```php
function getCachePath(): string
```

## setLifetime() {#cache-setLifetime}

Set the lifetime for the current query / token.

```php
function setLifetime(int|null $lifetime): Cache
```

### Parameters

1. # $lifetime: int|null
  - `int` in seconds. `0` means infinite
  - `null` no cache lifetime
    - Cache gets deleted on every update / delete / insert.

## getLifetime() {#cache-getLifetime}

Get the lifetime for the current query / token.

```php
function getLifetime(): int|null
```

### Return value

Either `int` >= 0 in seconds, where `0` means infinite, or `null`.