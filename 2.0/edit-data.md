<!--METADATA
{
    "title": "Edit Data",
    "url": "edit-data",
    "icon": "hammer"
}
!METADATA-->

# Edit Data

To edit a data object you can use the `update()` method of the `Store` class.

> ℹ️ If you need to make more complex updates look into <a class="gotoblock" href="#/query-builder">QueryBuilder</a> and <a class="gotoblock" href="#/query">Query</a>.

This method is especially fast because it uses the _id of the given document to update it directly instead of traversing through all documents.

```php
function update(array $updatable): bool;
```

Note: This method of the `Store` class updates/overrides entire document/s, not just parts.

### Parameters

  1. # $updatable: array
  One or multiple documents
    * ["_id" => 12, "title" => "SleekDB rocks!", ...]
    * [ ["_id" => 12, "title" => "SleekDB rocks!", ...], ["_id" => 13, "title" => "Multiple Updates", ...], ... ]

### Return value
Returns `true` on success or `false` if document with given _id does not exist.

## Update one user that we got beforehand

```php
$user = [
    'name' => 'Willard Bowman',
    'products' => [
        'totalSaved' => 0,
        'totalBought' => 0
    ],
];

//store the user
$store->insert($user); // has _id = 1

// retrieve a user
$user = $userStore->findById(1);

// update user
$user["name"] = "Luke Bowman";

$userStore->update( $user ); // updates the user by using his _id
```

## Update multiple users

```php
// retrieve users
$users = $userStore->findBy(["name", "=", "Josh"]);

foreach($users as $key => $user){
    // change the properties of the users
    $user["name"] = "Luke Bowman";
    
    // push changed user back to the users array
    $users[$key] = $user;
}

// update all users that had the name Josh
$userStore->update( $users );
```
