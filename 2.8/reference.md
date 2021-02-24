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
function __construct(string $storeName, string $databasePath, array $configuration = [])
```

Returns a new `QueryBuilder` object.
```php
function createQueryBuilder(): QueryBuilder
```

Change the destination of the store object.
```php
function changeStore(string $storeName, string $databasePath = null, array $configuration = []): Store
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
function findById(int|string $id): array|null
```

Retrieve one or multiple documents.
```php
function findBy(array $criteria, array $orderBy = null, int $limit = null, int $offset = null): array
```

Retrieve one document.
```php
function findOneBy(array $criteria): array|null
```

Do a fulltext like search against one or multiple fields.
```php
function search(array $fields, string $query, array $orderBy = null, int $limit = null, int $offset = null): array
```

Update parts of one document.
```php
function updateById(int|string $id, array $updatable): array|false
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
function deleteById(int|string $id): bool
```

Remove fields from one document by its primary key.
```php
function removeFieldsById($id, array $fieldsToRemove): array|false
```

Returns the amount of documents in the store.
```php
function count(): int
```

Return the last created store object ID.
```php
function getLastInsertedId(): int
```

Deletes a store and wipes all the data and cache it contains.
```php
function deleteStore(): bool
```

Get the name of the store.
```php
function getStoreName(): string
```

Get the path to the database folder.
```php
function getDatabasePath(): string
```

Get the path to the store. (including store name)
```php
function getStorePath(): string
```

Get the name of the field used as the primary key.
```php
function getPrimaryKey(): string
```

This method is used internally. Returns if caching is enabled store wide.
```php
function _getUseCache(): bool
```

This method is used internally. Returns the search options of the store.
```php
function _getSearchOptions(): array
```

This method is used internally. Returns the store wide default cache lifetime.
```php
function _getDefaultCacheLifetime(): null|int
```

Get the location (directory path) of the store.<br/>
🚨 Deprecated since version 2.7, use `getDatabasePath` instead.
```php
function getDataDirectory(): string
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
function search(string|array $fields, string $query, array $options = []): QueryBuilder
```

Join current store with another one. Can be used multiple times to join multiple stores.
```php
function join(Closure $joinFunction, string $propertyName): QueryBuilder
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

Re-generate the cache for the query.
```php
function regenerateCache(): QueryBuilder
```

This method is used internally. Returns a an array used to generate a unique token for the current query.
```php
function _getCacheTokenArray(): array
```

This method is used internally. Returns an array containing all information needed to execute an query.
```php
function _getConditionProperties(): array
```

This method is used internally. Returns the Store object used to create the QueryBuilder object.
```php
function _getStore(): Store
```

Add nested where conditions to filter data.<br/>
🚨 Deprecated since version 2.3, use `where` and `orWhere` instead
```php
function nestedWhere($conditions): QueryBuilder
```

Add "in" condition to filter data.<br/>
🚨 Deprecated since version 2.4, use "in" condition instead
```php
function in(string $fieldName, array $values = []): QueryBuilder
```

Add "not in" condition to filter data.<br/>
🚨 Deprecated since version 2.4, use "not in" condition instead
```php
function notIn(string $fieldName, array $values = []): QueryBuilder
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

Remove fields of one or multiple documents based on current query.
```php
function removeFields(array $fieldsToRemove): bool
```

## Cache

This class handles everything regarding caching like for example cache deletion.

Create a new Cache object.
```php
function __construct(Query $storePath, array &$cacheTokenArray, int|null $cacheLifetime)
```

Retrieve the cache lifetime for current query.
```php
function getLifetime(): null|int
```

Retrieve the path to cache folder of current store.
```php
function getCachePath(): string
```

Retrieve the cache token used as filename to store cache file.
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

Save content for current query as a cache file.
```php
function set(array $content)
```

Retrieve content of cache file.
```php
function get(): array|null
```

Delete cache file/s for current query.
```php
function delete()
```