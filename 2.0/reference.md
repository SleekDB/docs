<!--METADATA
{
    "title": "Reference",
    "url": "reference",
    "icon": "albums"
}
!METADATA-->

# Reference

## Store

The Store class is the beginning point and handles everything regarding store configuration.

Create a new Store object. (Internally it uses an existing or creates a new store)
  ```php
  __construct( string $storeName, string $dataDir = "", array $configuration = [] );
  ```

Returns a new QueryBuilder object.
  ```php
  getQueryBuilder(): QueryBuilder
  ```

Create/Insert a new object/document in the store. \
Returns the inserted object with _id.
  ```php
  insert(array $data): array
  ```

Create/Insert many objects/documents in the store. \
Returns the inserted objects/documents with _id.
  ```php
  insertMany(array $data): array
  ```

Deletes a store and wipes all the data and cache it contains.
  ```php
  delete(): bool
  ```

Get the name of the store.
  ```php
  getStoreName(): string;
  ```

Get the name of the store.
  ```php
  getStoreName(): string;
  ```

Change the directory where the store data is located.
  ```php
  setDataDirectory(string $directory): Store
  ```
  * `$directory` example: /home/users/max/stores


Get the location (directory path) of the store.
  ```php
  getDataDirectory(): string
  ```

Returns if caching is enabled store wide.
  ```php
  getUseCache(): bool
  ```

Returns the store wide default cache lifetime.
  ```php
  getDefaultCacheLifetime(): null|int
  ```

Return the last created store object ID.
  ```php
  getLastInsertedId(): int
  ```

Get the path to the store, including store name.
  ```php
  getStorePath(): string
  ```

Checks if store path and data directory are set. Throws InvalidStoreBootUpException on failure.
  ```php
  _checkBootUp()
  ```

## QueryBuilder

The QueryBuilder class handles everything regarding query creation like for example the where or join methods.

Create a new QueryBuilder object.
  ```php
  __construct(Store $store)
  ```

Select specific fields.
  ```php
  select(string[] $fieldNames): QueryBuilder
  ```

Exclude specific fields.
  ```php
  except(string[] $fieldNames): QueryBuilder
  ```

Add "where" condition to filter data. Can be used multiple times. All additional uses add an "and where" condition.
  ```php
  where(string $fieldName, string $condition, $value): QueryBuilder
  ```

Add or-where conditions to filter data.
  ```php
  orWhere(...$conditions): QueryBuilder
  ```
* string|array|mixed `...$conditions` \
  Parameter usage:
    * (string fieldName, string condition, mixed value)
    * ([string fieldName, string condition, mixed value],...)
    * (["fieldName" => string fieldName, "condition" => string condition, "value" => mixed value],...)

Add "in" condition to filter data.
  ```php
  in(string $fieldName, array $values = []): QueryBuilder
  ```

Add "not in" condition to filter data.
  ```php
  notIn(string $fieldName, array $values = []): QueryBuilder
  ```

Set the amount of data record to skip.
  ```php
  skip(int $skip = 0): QueryBuilder
  ```
  * `$skip` >= 0

Set the amount of data record to limit.
  ```php
  limit(int $limit = 0): QueryBuilder
  ```
  * `$limit` > 0

Set the sort order.
  ```php
  orderBy(string $order, string $orderBy = '_id'): QueryBuilder
  ```
  * `$order` "asc" or "desc"

Do a fulltext like search against one or more fields.
  ```php
  search(string|array $field, string $keyword): QueryBuilder
  ```

Join current store with another one. Can be used multiple times to join multiple stores.
  ```php
  join(callable $joinFunction, string $dataPropertyName): QueryBuilder
  
  // example with stores
  $result = $userStore->getQueryBuilder()->join(function($user) use ($commentStore){
    return $commentStore->getQueryBuilder()->where('user', '=', $user['_id']);
  }, "comments")->getQuery()->fetch();

  // example with query builders
  $result = $userBuilder->join(function($user) use ($commentBuilder){
    return $commentBuilder->where('user', '=', $user['_id']);
  }, "comments")->getQuery()->fetch();
  ```

Return distinct values.
  ```php
  distinct(string|array $fields = []): QueryBuilder
  ```

Use caching for current query
  ```php
  useCache(int $lifetime = null): QueryBuilder
  ```

Disable caching for current query.
  ```php
  disableCache(): QueryBuilder
  ```

Returns the cache lifetime for the current query.
  ```php
  getCacheLifetime(): int|null
  ```

Returns if caching is used for the current query.
  ```php
  getUseCache(): bool
  ```

This method makes a unique token for the current query. \
We use this hash token as the id/name of the cache file.
  ```php
  getCacheToken(): string
  ```

Get all conditions used for querying as an array.
  ```php
  _getConditionsArray(): array
  ```

Re-generate the cache for the query.
  ```php
  regenerateCache(): QueryBuilder
  ```

Returns a new Query object which can be used to execute the query build.
  ```php
  getQuery(): Query
  ```

Set DataDirectory for current query.
  ```php
  setDataDirectory(string $directory): QueryBuilder
  ```

Get the data directory of the store.
  ```php
  getDataDirectory(): string
  ```

Get the store used to create the query builder.
  ```php
  getStore(): Store
  ```

# Query

This class handles everything regarding query execution like fetch / first / exists.

Create a new Query object. (Internally it creates a new Cache object)
  ```php
  __construct(QueryBuilder $queryBuilder)
  ```

Get the Cache object.
  ```php
  getCache(): Cache
  ```

Execute Query and get Results.
  ```php
  fetch(): array
  ```

Check if data is found.
  ```php
  exists(): bool
  ```

Return the first document. (More efficient than `fetch` but `orderBy` does not work)
  ```php
  first(): array
  ```

Update one or multiple documents, based on the current query.
  ```php
  update(array $updatable): bool
  ```

Deletes matched documents.
  ```php
  delete(bool $returnRecordsCount = false): bool|int
  ```
  * `$returnRecordsCount` if true int will be returned

## Cache

This class handles everything regarding caching like for example cache deletion.

Create a new Cache object.
  ```php
  __construct(QueryBuilder $queryBuilder, string $cacheDir = "")
  ```
  * `$cacheDir` sub folder name of data directory

Set the cache lifetime.
  ```php
  setLifetime(int|null $lifetime): Cache
  ```

Get the cache lifetime.
  ```php
  getLifetime(): null|int
  ```

Get the path to the cache folder.
  ```php
  getCachePath(): string
  ```

Get the cache token.
  ```php
  getToken(): string
  ```

Get the name of the cache directory, which is a sub folder of data directory.
  ```php
  getCacheDir(): string
  ```

Delete all cache files for current store.
  ```php
  deleteAll()
  ```

Delete all cache files with no lifetime (null) in current store.
  ```php
  deleteAllWithNoLifetime()
  ```

Cache content for current query
  ```php
  set(array $content)
  ```

Returns cached result for current query if found, else null.
  ```php
   get(): array|null
  ```

Delete cache file/s for current query.
  ```php
  delete()
  ```