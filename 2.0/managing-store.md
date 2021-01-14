<!--METADATA
{
    "title": "Managing Store",
    "url": "managing-store",
    "icon": "cube"
}
!METADATA-->

# Stores

Store is a simple directory where SleekDB will write all your data in JSON documents. "Store" is similar with the idea of "Table" in MySQL or "Collection" in MongoDB. You don't need to create a "store" directory manually.

## Your first store

To start working with a store, at first we need to create an object.

Later, we can use that object to work with data for that store.

- Creating a store

  ```php
  use SleekDB\SleekDB;
  $newsStore = new \SleekDB\Store('news', $dataDir);
  ```

- Creating an additional store; assuming we are working on a community site where need to have a users store too.

  ```php
  $userStore = new \SleekDB\Store('users', $dataDir);
  ```

- Another store to keep all the posts shared by the user.

  ```php
  $postStore = new \SleekDB\Store('posts', $dataDir);
  ```

- Creating a new user
  ```php
  $userStore->insert([
    'name' => 'Mike Doe',
    'email' => 'miked@example.com',
    'avatar' => [
      'sm' => "/img-sm.jpg",
      'lg' => "/img-lg.jpg"
    ]
  ]);
  ```

In the above example we have created a new user to understand the purpose of a store in SleekDB. In this documentation later we will see more examples on this.

## Deleting A Store

To delete a store use the `delete()` method. It deletes a store and wipes all the data and cache it contains.

Example:

```php
$userStore->delete();
```

## Keeping Store Conditions

When you use the `getQueryBuilder()` method you get a new QueryBuilder object, that means your conditions are reset to default.

But if you want to keep the condition parameters for query and perform additional operations then you may want to keep the QueryBuilder and/or Query object.

Here is an example showing how you may fetch data and then update on the discovered documents without running an additional query:

```php
$userQuery = $userStore->getQueryBuilder()
  ->where('products.totalBought', '>', 0)
  ->where('products.totalSaved', '>', 0)
  ->getQuery();

// Fetch data.
$userQuery->fetch();

// Update matched documents.
$userQuery->update([
  'someRandomData' => '123',
]);
```
