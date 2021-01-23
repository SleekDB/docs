<!--METADATA
{
    "title": "Delete Data",
    "url": "delete-data",
    "icon": "trash"
}
!METADATA-->

# Delete Data

To delete a data object we would use the `delete()` method. Example:

Lets delete the user whose name is "Joshua Edwards"

```php
$usersDB->where( 'name', '=', 'Joshua Edwards' )->delete();
```

You can use more than one where condition if required.
