<!--METADATA
{
    "title": "Insert Data",
    "url": "insert-data",
    "icon": "create"
}
!METADATA-->

# Insert Data

To insert data first you make a PHP array, and simply insert that array into a store.

## Insert a single document

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
$user = $usersStore->insert($user);
```

<br/>

## Insert multiple documents

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
$users = $usersStore->insertMany($users);
```