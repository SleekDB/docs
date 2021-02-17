<!--METADATA
{
    "title": "Edit Data",
    "url": "edit-data",
    "icon": "hammer"
}
!METADATA-->

# Edit Data

To edit a data object you can use the `update()` and `updateById()` method of the `Store` class.

> ℹ️ If you need to make more complex updates look into <a class="gotoblock" href="/#/query-builder">QueryBuilder</a> and <a class="gotoblock" href="/#/query">Query</a>.

## Summary

- <a class="gotoblock" href="#/edit-data#updateById">updateById</a>
- <a class="gotoblock" href="#/edit-data#update">update</a>

## updateById() {#edit-data-updateById}

Update parts of a document.<br/>
This method is especially fast because it uses the \_id of the given document to update it directly instead of traversing through all documents.

```php
function updateById(int $id, array $updatable): array|false
```

### Parameters

1. # $id: int
   Id of document to update.
2. # $updatable: array
   Array containing the parts to update.<br/>
   Update of nested values possible by using a dot between fieldNames (Example 3)<br/>
   If a property does not exist in that document, it will be added.

### Return value
Returns `updated document` on success or `false` if document could not be found.

### Example 1

Change the status of the user with \_id = 23.

```php
$userStore->updateById(23, [ "status" => "active" ]);
```

### Example 2

Change the name and the age of the user with \_id = 24.

```php
$userStore->updateById(24, [ "name" => "Georg", "age" => 22 ]);
```

### Example 3

Change the street of the user with \_id = 24.<br/>
**Note**: The street is stored in a nested array.

```php
$userStore->updateById(24, [ "address.street" => "first street" ]);
```

#### Result

```php
[
    "_id" => 24,
    "address" => [
        "street" => "first street",
        "postalCode" => "47129"
    ],
    ...
]
```


## update() {#edit-data-update}

Update a whole document.<br/>
This method is especially fast because it uses the \_id of the given document to update it directly instead of traversing through all documents.

```php
function update(array $updatable): bool;
```

**Note**: This method of the `Store` class updates/overrides entire document/s, not just parts.

### Parameters

1. # $updatable: array

   One or multiple documents

   - ["_id" => 12, "title" => "SleekDB rocks!", ...]
   - [ ["_id" => 12, "title" => "SleekDB rocks!", ...], ["_id" => 13, "title" => "Multiple Updates", ...], ... ]

### Return value

Returns `true` on success or `false` if document with given \_id does not exist.

### Example 1: Update one user that we got beforehand

```php
$user = [
    'name' => 'Willard Bowman',
    'products' => [
        'totalSaved' => 0,
        'totalBought' => 0
    ],
];

//store the user
$userStore->insert($user); // has _id = 1

// retrieve a user
$user = $userStore->findById(1);

// update user
$user["name"] = "Luke Bowman";

$userStore->update( $user ); // updates the user by using his _id
```

### Example 2: Update multiple users

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
