<!--METADATA
{
    "title": "Edit Data",
    "url": "edit-data",
    "icon": "hammer"
}
!METADATA-->

# Edit Data

To edit a data object we would use the `update()` method.

The update method takes only one argument.

```php
update(array $updatable): bool;
```

Lets update the "totalBought" value of a user whose name is "Joshua Edwards"

```php
$updateable = [
    'products' => [
        'totalBought' => 1
    ]
];
$usersStore->getQueryBuilder()->where( 'name', '=', 'Joshua Edwards' )->getQuery()->update( $updateable );
```

You can use more than one where condition if required.
