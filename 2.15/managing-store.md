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

To start working with a store we need to create an object.

Later, we can use that object to work with the data in that store.

- Creating a store

  ```php
  use SleekDB\Store;
  $newsStore = new Store('news', $dataDir);
  ```

- Creating an additional store; assuming we are working on a community platform, where need a users store too.

  ```php
  $userStore = new Store('users', $dataDir);
  ```

- Another store to keep all the posts shared by the user.

  ```php
  $postStore = new Store('posts', $dataDir);
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

To delete a store use the `deleteStore()` method. It deletes a store and wipes all the data and cache it contains.

Example:

```php
$userStore->deleteStore();
```
