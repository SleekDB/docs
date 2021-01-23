<!--METADATA
{
    "title": "Managing Store",
    "url": "managing-store",
    "icon": "cube"
}
!METADATA-->

# Stores

Store is a simple directory where SleekDB will write all your data in JSON documents. "Store" is similar with the idea of "Table" in MySQL or "Collection" in MongoDB. You dont need to create a "store" manually, while you use the "store" method from the SleekDB object it will first check if the "store" directory exists or not, if not then it would be created instantly.

At this moment you can not rename a store, but you can do it manually using the File Browser of your OS or using the terminal.

## Your first store

To start working with a store, at first we need to create an object using the "store" static method.

Later, we can use that object to work with data for that store.

- Create an store

  ```php
  $newsStore = \SleekDB\SleekDB::store('news', $dataDir);
  ```

- Having multiple store; assuming we are working on a community site where need to have a users store.

  ```php
  $userStore = \SleekDB\SleekDB::store('users', $dataDir);
  ```

- Another store to keep all the posts shared by the user.

  ```php
  $postStore = \SleekDB\SleekDB::store('posts', $dataDir);
  ```

- Creating a new user
  ```php
  $userStore->insert([
    'name' => 'Mike Doe',
    'email' => 'miked@example.com'
  ]);
  ```

In the above example we have created an user to understand the use case of a store in SleekDB. In this documentation later we will see more examples on this.

## Deleting A Store

To delete a store use the `deleteStore()` method. It deletes a store and wipes all the data and cache it contains.

Example:

```php
$userStore->deleteStore();
```

## Keeping Store Conditions

When you run a query and use a action method like `fetch`, `update` or `delete` the conditions you have added for the active query will be reset to default.

But if you want to keep the condition parameters for query and perform additional operations then you may want to use the `keepConditions()` method.

Here is an example showing how you may fetch data and then update on the discovered documents without running an additional query:

```php
// Find documents.
$result = $usersDB
  ->keepConditions() // Won't reset the active query state.
  ->where('products.totalBought', '>', 0)
  ->where('products.totalSaved', '>', 0);

// Fetch data.
$result->fetch();

// Update matched documents.
$result->update([
  'someRandomData' => '123',
]);
```
