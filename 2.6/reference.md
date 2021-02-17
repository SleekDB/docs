<!--METADATA
{
    "title": "Reference",
    "url": "reference",
    "icon": "albums"
}
!METADATA-->

# Reference

This page contains a brief overview of all the classes and their methods. For more detailed information please visit the other documentation pages.

## Store

The Store class is the beginning point and handles everything regarding store configuration. It also provides all methods needed for simple queries.

Create a new Store object. (Internally it creates a new store folder if it doesn't exist)
```php
function __construct(string $storeName, string $dataDir, array $configuration = [])
```

Returns a new `QueryBuilder` object.
```php
function createQueryBuilder(): QueryBuilder
```

Create/Insert a new document in the store.<br/>
Returns the inserted document with it's new and unique _id.
```php
function insert(array $data): array
```

Create/Insert many documents in the store.<br/>
Returns the inserted documents with their new and unique _id.
```php
function insertMany(array $data): array
```

Retrieve all documents of that store.
```php
function findAll(): array
```

Retrieve one document by its _id. Very fast because it finds the document by its file path.
```php
function findById(int $id): array|null
```

Retrieve one or multiple documents.
```php
function findBy(array $criteria, array $orderBy = null, int $limit = null, int $offset = null): array
```

Retrieve one document.
```php
function findOneBy(array $criteria): array|null
```

Update parts of one document.
```php
function updateById(int $id, array $updatable): array|false
```

Update one or multiple documents.
```php
function update(array $updatable): bool
```

Delete one or multiple documents.
```php
function deleteBy(array $criteria, int $returnOption = Query::DELETE_RETURN_BOOL): bool|array|null
```

Delete one document by its _id. Very fast because it deletes the document by its file path.
```php
function deleteById(int $id): bool
```

Deletes a store and wipes all the data and cache it contains.
```php
function deleteStore(): bool
```

Change the destination of a Store.
```php
function changeStore(string $storeName, string $dataDir = null, array $configuration = []): Store
```

Get the name of the store.
```php
function getStoreName(): string
```

Get the location (directory path) of the store.
```php
function getDataDirectory(): string
```

Returns if caching is enabled store wide.
```php
function _getUseCache(): bool
```

Returns the store wide default cache lifetime.
```php
function getDefaultCacheLifetime(): null|int
```

Return the last created store object ID.
```php
function getLastInsertedId(): int
```

Get the path to the store, including store name.
```php
function getStorePath(): string
```

## QueryBuilder

The QueryBuilder class handles everything regarding query creation like for example the where or join methods.

Create a new QueryBuilder object.
```php
function __construct(Store $store)
```

Returns a new `Query` object which can be used to execute the query build.
```php
function getQuery(): Query
```

Select specific fields.
```php
function select(string[] $fieldNames): QueryBuilder
```

Exclude specific fields.
```php
function except(string[] $fieldNames): QueryBuilder
```

Add "where" condition to filter data. Can be used multiple times. All additional uses add an "and where" condition.
```php
function where(array $conditions): QueryBuilder
```

Add or-where conditions to filter data.
```php
function orWhere($conditions): QueryBuilder
```

Add nested where conditions to filter data.<br/>
🚨 Deprecated since version 2.3, use `where` and `orWhere` instead
```php
function nestedWhere($conditions): QueryBuilder
```

Add "in" condition to filter data.
```php
function in(string $fieldName, array $values = []): QueryBuilder
```

Add "not in" condition to filter data.
```php
function notIn(string $fieldName, array $values = []): QueryBuilder
```

Set the amount of data record to skip.
```php
function skip(int $skip = 0): QueryBuilder
```

Set the amount of data record to limit.
```php
function limit(int $limit = 0): QueryBuilder
```

Set the sort order.
```php
function orderBy(array $criteria): QueryBuilder
```

Group documents using one or multiple fields.
```php
function groupBy(array $groupByFields, string $counterKeyName = null, bool $allowEmpty = false): QueryBuilder
```

Filter grouped documents.
```php
function having(array $criteria): QueryBuilder
```

Do a fulltext like search against one or more fields.
```php
function search(string|array $field, string $keyword): QueryBuilder
```

Join current store with another one. Can be used multiple times to join multiple stores.
```php
function join(Closure $joinFunction, string $dataPropertyName): QueryBuilder
```

Return distinct values.
```php
function distinct(string|array $fields = []): QueryBuilder
```

Use caching for current query
```php
function useCache(int $lifetime = null): QueryBuilder
```

Disable caching for current query.
```php
function disableCache(): QueryBuilder
```

This method is used internally. It returns an array containing all properties that are used for the cache token generation.
```php
function _getCacheTokenArray(): array
```

This method is used internally. It returns an array that contains all information needed to execute an query.
```php
function _getConditionProperties(): array
```

Re-generate the cache for the query.
```php
function regenerateCache(): QueryBuilder
```

Get the store object used to create the query builder.
```php
function _getStore(): Store
```

## Query

This class handles everything regarding query execution like fetch / first / exists.

Create a new Query object. (Internally it creates a new Cache object)
```php
function __construct(QueryBuilder $queryBuilder)
```

Get the `Cache` object.
```php
function getCache(): Cache
```

Execute Query and get Results.
```php
function fetch(): array
```

Check if data is found.
```php
function exists(): bool
```

Return the first document. (More efficient than `fetch` but `orderBy` does not work)
```php
function first(): array
```

Update one or multiple documents, based on the current query.
```php
function update(array $updatable, bool $returnUpdatedDocuments = false): array|bool
```

Deletes matched documents.
```php
function delete(int $returnOption = Query::DELETE_RETURN_BOOL): bool|array|int
```

## Cache

This class handles everything regarding caching like for example cache deletion.

Create a new Cache object.
```php
function __construct(Query $query, string $storePath)
```

Set the cache lifetime.
```php
function setLifetime(int|null $lifetime): Cache
```

Get the cache lifetime.
```php
function getLifetime(): null|int
```

Get the path to the cache folder.
```php
function getCachePath(): string
```

Get the cache token.
```php
function getToken(): string
```

Delete all cache files for current store.
```php
function deleteAll()
```

Delete all cache files with no lifetime (null) in current store.
```php
function deleteAllWithNoLifetime()
```

Cache content for current query
```php
function set(array $content)
```

Returns cached result for current query if found, else null.
```php
function get(): array|null
```

Delete cache file/s for current query.
```php
function delete()
```