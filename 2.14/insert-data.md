<!--METADATA
{
    "title": "Insert Data",
    "url": "insert-data",
    "icon": "create"
}
!METADATA-->

# Insert Data

To insert data first you make a PHP array, and simply insert that array into a store.

## Brief repetition of Store object creation

For a more detailed documentation on `Store` object creation refer to the corresponding <a class="gotoblock" href="#/managing-store">"Managing Store" documentation</a>.

```php
use SleekDB\Store;
$userStore = new Store('users', __DIR__ . "/database");
```

## Summary

- <a class="gotoblock" href="#/insert-data#insert">insert</a>
- <a class="gotoblock" href="#/insert-data#insertMany">insertMany</a>
- <a class="gotoblock" href="#/insert-data#updateOrInsert">updateOrInsert</a>
- <a class="gotoblock" href="#/insert-data#updateOrInsertMany">updateOrInsertMany</a>

## Insert a single document {#insert-data-insert}

```php
function insert(array $data): array
```

### Parameters

  1. # $data: array
  One document that will be insert into the Store
    * ["name" => "Josh", "age" => 23, "city" => "london"]

### Return value
Returns the inserted document as an `array` including the automatically generated and unique `_id` property.

### Example

```php
// Prepare a PHP array to insert.
$user = [
    'name' => 'Kazi Hasan',
    'products' => [
        'totalSaved' => 19,
        'totalBought' => 27
    ],
    'location' => [
        'town' => 'Nagar',
        'city' => 'Dhaka',
        'country' => 'Bangladesh'
    ]
];
// Insert the data.
$user = $userStore->insert($user);
```

<br/>

## Insert multiple documents {#insert-data-insertMany}

```php
function insertMany(array $data): array
```

### Parameters

  1. # $data: array
  Multiple documents that will be insert into the Store
    * [ ["name" => "Josh", "age" => 23], ["name" => "Mike", "age" => 19], ... ]

### Return value
Returns the inserted documents in an `array` including their automatically generated and unique `_id` property.

### Example

```php
// Prepare users data.
$users = [
    [
        'name' => 'Russell Newman',
        'products' => [
            'totalSaved' => 5,
            'totalBought' => 3
        ],
        'location' => [
            'town' => 'Andreas Ave',
            'city' => 'Maasdriel',
            'country' => 'England'
        ]
    ],
    [
        'name' => 'Willard Bowman',
        'products' => [
            'totalSaved' => 0,
            'totalBought' => 0
        ],
    ],
    [
        'name' => 'Tommy Mendoza',
        'products' => [
            'totalSaved' => 172,
            'totalBought' => 54
        ],
    ],
    [
        'name' => 'Joshua Edwards',
        'phone' => '(382)-450-8197'
    ]
];
// Insert all data.
$users = $userStore->insertMany($users);
```

## updateOrInsert() {#insert-data-updateOrInsert}

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

## updateOrInsertMany() {#insert-data-updateOrInsertMany}

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