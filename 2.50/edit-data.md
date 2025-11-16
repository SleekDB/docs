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

## Brief repetition of Store object creation

For a more detailed documentation on `Store` object creation refer to the corresponding <a class="gotoblock" href="#/managing-store">"Managing Store" documentation</a>.

```php
use SleekDB\Store;
$userStore = new Store('users', __DIR__ . "/database");
```

## Summary

- <a class="gotoblock" href="#/edit-data#updateById">updateById</a>
- <a class="gotoblock" href="#/edit-data#update">update</a>
- <a class="gotoblock" href="#/edit-data#removeFieldsById">removeFieldsById</a>
- <a class="gotoblock" href="#/edit-data#updateOrInsert">updateOrInsert</a>
- <a class="gotoblock" href="#/edit-data#updateOrInsertMany">updateOrInsertMany</a>

## updateById() {#edit-data-updateById}

Update parts of a document.<br/>
This method is especially fast because it uses the \_id to update the document directly instead of traversing through all documents.

```php
function updateById(int|string $id, array $updatable): array|false
```

### Parameters

1. # $id: int|string
   Id of document to update.
2. # $updatable: array
   Array containing the parts to update.<br/>
   Update of nested values possible by using a dot between fieldNames (Example 3)<br/>
   If a field does not exist in that document, it will be added.

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
This method is especially fast because it uses the \_id of the given document/s to update it directly instead of traversing through all documents.

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

## removeFieldsById() {#edit-data-removeFieldsById}

Update parts of a document.<br/>
This method is especially fast because it uses the \_id to update the document directly instead of traversing through all documents.

```php
function removeFieldsById(int|string $id, array $fieldsToRemove): array|false
```

### Parameters

1. # $id: int|string
   Id of document to remove fields of.
2. # $fieldsToRemove: array
   Array containing fields to remove.<br/>
   Removal of nested fields possible by using a dot between fieldNames (Example 3)

### Return value
Returns `updated document` on success or `false` if document could not be found.

### Example 1

Remove the status field of the user with \_id = 23.

```php
$userStore->updateById(23, [ "status" ]);
```

### Example 2

Remove the name and the age fields of the user with \_id = 24.

```php
$userStore->updateById(24, [ "name", "age" ]);
```

### Example 3

Remove the street field of the user with \_id = 24.<br/>
**Note**: The street is stored in a nested array.

```php
$userStore->updateById(24, [ "address.street" ]);
```

#### Result

```php
[
    "_id" => 24,
    "address" => [
        "postalCode" => "47129"
    ],
    ...
]
```

## updateOrInsert() {#edit-data-updateOrInsert}

Update or insert one document.<br/>
This method is especially fast because it uses the \_id to update or insert the document directly instead of traversing through all documents.

```php
public function updateOrInsert(array $data, bool $autoGenerateIdOnInsert = true): array
```

### Parameters

1. # $data: array
   Document to update or insert.
2. # $autoGenerateIdOnInsert: bool
   Default: `true`<br/>
   If `false` and **the document has a \_id**, that does not already exist (insert), the \_id will be to insert the new document, an auto-generated \_id will be used otherwise.

### Return value
Returns `updated / inserted document`.

### Example 1

Update or Insert a user with \_id = 23, apply an auto-generated \_id if it is an insert.

```php
$user = [
    "_id" => 23,
    "name" => "John",
    ...
];
$userStore->updateOrInsert($user);
```

### Example 2

Update or Insert a user with \_id = 23.

```php
$user = [
    "_id" => 23,
    "name" => "John",
    ...
];
$userStore->updateOrInsert($user, false);
```

### Example 3

Update or Insert a user with no \_id.

```php
$user = [
    "name" => "John",
    ...
];
$userStore->updateOrInsert($user);
```

## updateOrInsertMany() {#edit-data-updateOrInsertMany}

Update or insert multiple documents.<br/>
This method is especially fast because it uses the \_id to update or insert the documents directly instead of traversing through all documents.

```php
public function updateOrInsertMany(array $data, bool $autoGenerateIdOnInsert = true): array
```

### Parameters

1. # $data: array
   Documents to update or insert.

2. # $autoGenerateIdOnInsert: bool
   Default: `true`<br/>
   If `false` and **a document has a \_id**, that does not already exist (insert), the \_id will be to insert the new document, an auto-generated \_id will be used otherwise.

### Return value
Returns `updated / inserted documents`.

### Example 1

Update or Insert multiple users and apply auto-generated \_id's on inserts.

```php
$users = [
    [
        "_id" => 23,
        "name" => "John",
        ...
    ],
    [
        "_id" => 25,
        "name" => "Max",
        ...
    ],
    [
        "name" => "Lisa",
        ...
    ]
    ...
];
$userStore->updateOrInsertMany($users);
```

### Example 2

Update or Insert a users with their \_id's.

```php
$users = [
    [
        "_id" => 23,
        "name" => "John",
        ...
    ],
    [
        "_id" => 25,
        "name" => "Max",
        ...
    ],
    [
        "name" => "Lisa", // <-- will get auto-generated _id
        ...
    ]
    ...
];
$userStore->updateOrInsertMany($users, false);
// Lisa will get a auto-generated _id, because there is no _id in the document!
```