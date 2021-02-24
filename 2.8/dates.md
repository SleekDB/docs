<!--METADATA
{
    "title": "Working with Dates",
    "url": "dates",
    "icon": "alarm"
}
!METADATA-->

# Working with Dates

SleekDB accepts instances of PHP's DateTime class to filter data.<br/>
Means you can check against an DateTime object.

The methods that accept DateTime objects as a value to check against are:

- <a class="gotoblock" href="#/fetch-data#findBy">`findBy`</a>
- <a class="gotoblock" href="#/fetch-data#findOneBy">`findOneBy`</a>
- <a class="gotoblock" href="#/delete-data#deleteBy">`deleteBy`</a>
- <a class="gotoblock" href="#/query-builder#where">`where`</a>
- <a class="gotoblock" href="#/query-builder#orWhere">`orWhere`</a>

The conditions you can use DateTime objects with are:

- `=`
- `===`
- `==` (Type unsafe comparison)
- `!=`
- `!==`
- `<>` (Type unsafe comparison)
- `>`
- `>=`
- `<`
- `<=`
- `IN`
- `NOT IN`
- `BETWEEN`
- `NOT BETWEEN`

## Quick example

```php
require_once './vendor/autoload.php';

use SleekDB\Store;

// create store
$databaseDirectory = __DIR__ . "/database";
$newsStore = new Store("news", $databaseDirectory);

// Convert the date-strings to timestamps
$startDate = new \DateTime("2020-12-01");
$endDate = new \DateTime("2021-01-04");

// Get result
// WHERE releaseDate >= "2020-12-01" AND releaseDate <= "2021-01-04"
$news = $newsStore->findBy([ "releaseDate", "BETWEEN", [ $startDate, $endDate ] ] );
```

## How does SleekDB use DateTime objects to filter data

Internally SleekDB converts the value of the field (**"releaseDate" in the example above**) into a DateTime object and compares it with the given DateTime object.

That means if there is **something stored in the field** (for example "releaseDate") that **<u>can not be converted</u> into a DateTime object** SleekDB will **throw an InvalidArgumentException**.

Refer to the <a rel="noopener nofollow" href="https://www.php.net/manual/en/class.datetime.php" target="_blank">official PHP documentation</a> to learn more about DateTime objects. 


SleekDB already handles the following values correctly, so no error will be thrown and you can use them for example to clarify that there is no release date for that document.

- 0
- 0.0
- "0"
- ""
- NULL
- FALSE
- array()


## Best practice to store dates in the database

You should either store the date and time as a string or as a timestamp.

### Example 1

Store date as a string.

```php
$newArticle = [
  "author" => "John",
  "title" => "Why everybody love SleekDB",
  "content" => "Because it's the best!",
  "releaseDate" => "2021-01-17"
];

$newArticle = $newsStore->insert($newArticle);
```

### Example 2

Store date as a timestamp.

```php
$releaseDate = new \DateTime("2021-01-17");

$newArticle = [
  "author" => "John",
  "title" => "Why everybody love SleekDB",
  "content" => "Because it's the best!",
  "releaseDate" => $releaseDate->getTimestamp()
];

$newArticle = $newsStore->insert($newArticle);
```

### Example 3

Store current date and time.

```php
$releaseDate = new \DateTime();

$newArticle = [
  "author" => "John",
  "title" => "Why everybody love SleekDB",
  "content" => "Because it's the best!",
  "createdAt" => $releaseDate->format("Y-m-d H:i:s")
];

// OR

$newArticle = [
  "author" => "John",
  "title" => "Why everybody love SleekDB",
  "content" => "Because it's the best!",
  "createdAt" => $releaseDate->getTimestamp()
];

$newArticle = $newsStore->insert($newArticle);
```

## Not using DateTime

If you don't want to use DateTime objects to filter data you don't have to.<br/>
To filter data without the need of DateTime objects we **have to store the dates as a timestamp**.

```php
require_once './vendor/autoload.php';

use SleekDB\Store;

// create store
$databaseDirectory = __DIR__ . "/database";
$newsStore = new Store("news", $databaseDirectory);

// Insert an article
$releaseDate = new \DateTime("2021-01-17");
$newArticle = [
  "author" => "John",
  "title" => "Why everybody love SleekDB",
  "content" => "Because it's the best!",
  "createdAt" => $releaseDate->getTimestamp()
];
$newArticle = $newsStore->insert($newArticle);

// retrieve articles
$datesToFilter = [
  (new \DateTime("2020-12-01"))->getTimestamp(),
  (new \DateTime("2021-01-04"))->getTimestamp()
  (new \DateTime("2021-01-19"))->getTimestamp()
];
$news = $newsStore->findBy([ "releaseDate", "IN", $datesToFilter ] );
// WHERE releaseDate IN ("2020-12-01", "2021-01-04", "2021-01-19")
```