<!--METADATA
{
    "title": "Advanced",
    "url": "advanced",
    "icon": "flame"
}
!METADATA-->

# Advanced

## Keeping QueryBuilder object

When you use the `createQueryBuilder()` method you get a new QueryBuilder object, that means your conditions are reset to default.

You can keep the QueryBuilder object and reuse it to add new conditions.

### Example

```php
$userQueryBuilder = $userStore->createQueryBuilder();

$userQueryBuilder->where([ 'products.totalBought', '>', 0 ]);

$userQueryBuilder->where([ 'products.totalSaved', '>', 0 ]);

// fetch all users that have totalBought > 0 and totalSaved > 0
$users = $userQueryBuilder->getQuery()->fetch();

// add new condition
$userQueryBuilder->where([ 'active', '=', true ]);

// fetch all users that have totalBought > 0, totalSaved > 0 and are active
$usersActive = $userQueryBuilder->getQuery()->fetch();
```

## Keeping Query object

If you want to keep the conditions and perform additional operations then you may want to keep the Query object.

Here is an example showing how you may fetch data and then update on the discovered documents without running an additional query:

### Example

```php
$userQuery = $userStore->createQueryBuilder()
  ->where('products.totalBought', '>', 0)
  ->where('products.totalSaved', '>', 0)
  ->getQuery();

// Fetch data.
$users = $userQuery->fetch();

// Update matched documents.
$userQuery->update([
  'someRandomData' => '123',
]);

// Fetch data again.
$usersUpdated = $userQuery->fetch();
```