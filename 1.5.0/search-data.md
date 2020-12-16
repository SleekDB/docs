<!--METADATA
{
    "title": "Search Data",
    "url": "search-data",
    "icon": "search"
}
!METADATA-->

# Search Data

We can search data using the search() method. It utilizes the <a href="http://php.net/manual/en/function.similar-text.php" target="_blank">similar_text()</a> function and works better on medium length string.

The search method takes two argument,

```php
search($field, $keyword);
```

1. **$field** argument receives the property name on which we want to perform the search.
2. **$keyword** is the search keyword.

Lets search for users who lives in Canada.

```php
$users = $usersDB
    ->search( 'location.country', 'Canada' )
    ->fetch();
```

To ensure proper search result we can search more than one property at a time. Example,

```php
$users = $usersDB
    ->search( 'bio', 'Manufactured in Canada' )
    ->search( 'location.country', 'Canada' )
    ->where( 'active', '=', 1 )
    ->fetch();
```
