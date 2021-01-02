<!--METADATA
{
    "title": "Getting Started",
    "url": "getting-started",
    "icon": "rocket"
}
!METADATA-->

# Getting Started

Getting started with SleekDB is super easy. We keep data in a "store", which is similar to MySQL "table" or MongoDB "collection". To start working with data at first we create a new instance of a SleekDB Store.

1. To create the SleekDB instance we need a valid "path" where it can write data. Absolute and relative paths are supported.

```php
$dataDir = "/Users/username/documents/mydb";
```

2. Once we have the data directory lets create the SleekDB Store, which can be treated as the "table" in SQL databases. If the store doesn't exist then it will be created automatically.

```php
$newsStore = new \SleekDB\Store('news', $dataDir);
```

Optionally you can pass a configuration array in the third parameter. Read more about <a class="gotoblock" href="#/configurations">configurations</a>.

3. Let's insert a new item like a news article.

```php
// An array that we want to insert.
$newsInsertable = [
  "title" => "Google Pixel XL",
  "about" => "The unlocked biggest Pixel 2..."
];
$results = $newsStore->insert( $newsInsertable );
```

The results variable would contain all the inserted data and with the `_id` property.