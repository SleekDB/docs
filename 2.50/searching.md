<!--METADATA
{
    "title": "Searching",
    "url": "searching",
    "icon": "search"
}
!METADATA-->

# Searching

SleekDB introduces with version 2.7 enhanced search functionality with **search scores**, **different search algorithms** and **two search modes**.

Searching through documents was never so easy and good before!

## Summary

- <a class="gotoblock" href="#/searching#configuration-options">Configuration options</a>
- <a class="gotoblock" href="#/searching#mode-or">Mode: "or" (default)</a>
- <a class="gotoblock" href="#/searching#mode-and">Mode: "and"</a>
- <a class="gotoblock" href="#/searching#algorithm-hits">Algorithm: "hits" (default)</a>
- <a class="gotoblock" href="#/searching#algorithm-hits_prioritize">Algorithm: "hits_prioritze"</a>
- <a class="gotoblock" href="#/searching#algorithm-prioritize">Algorithm: "prioritize"</a>
- <a class="gotoblock" href="#/searching#algorithm-prioritize">Algorithm: "prioritize_position"</a>
- <a class="gotoblock" href="#/searching#query_builder">`search()` method of `QueryBuilder` class</a>
- <a class="gotoblock" href="#/searching#store">`search()` method of `Store` class</a>

## Cofiguration options

You can configure the bahaviour of SleekDB regarding searching <a class="gotoblock" href="#/configurations">Store wide</a> and on a query by query bases.


```php
// Store wide configuration when creating a store

$configuration = [
  "search" => [
    "min_length" => 2,
    "mode" => "or",
    "score_key" => "scoreKey",
    "algorithm" => Query::SEARCH_ALGORITHM["hits"]
  ]
];

$store = new Store("users", __DIR__ . "/database", $configuration);
```


```php
// On a query by query bases

$searchOptions = [
  "minLength" => 2,
  "mode" => "or",
  "scoreKey" => "scoreKey",
  "algorithm" => Query::SEARCH_ALGORITHM["hits"]
];

$userStore = new Store("users", __DIR__ . "/database");

$userStore->createQueryBuilder()
  ->search(["profileDescription"], "SleekDB", $searchOptions)
  ->getQuery()
  ->fetch();
```

### "min_length" and "minLength"

It has to be an `integer` > 0.<br/>
The minimum length of a word within the search query that will be used for searching.<br/>
If it is for example set to the value of 2 (default) and the user searches for: "I love SleekDB".<br/>
The words "love" and "SleekDB" will be used for searching and the word "I" will be ignored.<br/>
That means with the configuration of 2 all words >= 2 are considered when searching.

### "mode"

Has to be a `string`.<br/>
SleekDB provides two modes.
- "or"
- "and"

### "score_key" and "scoreKey"

Can be a `string` or `null`.<br/>
If this configuration is `null` no field containing the score will be applied to the documents.<br/>
If it is a `string` a new field with the specified field name will be applied to every document, which will contain the search score and can be used to for example sort the documents.

### algorithm

SleekDB provides 4 search algorithms. `hits`, `hits_prioritize`, `prioritize`, `prioritize_position`<br/>
These algorithms change the score generation.<br/>
They are available as a **constant of the `Query` class**.

Example:
```php
Query::SEARCH_ALGORITHM["hits"] // default
```

## Mode: "or" (default) {#searching-mode-or}

With this mode a field has to contain **one** of the words in the search query to be considered as a search result.<rb/>
For example our query is "Where School" and we have three values to check agains:

- "Where do you think is the best school?"
  - Two search hits
- "School is cool, but where is the best one?"
  - Two search hits
- "Why everybody love School"
  - One search hit

## Mode: "and" {#searching-mode-and}

With this search mode a field has to contain **all** words of the search query to be considered as a search result.<br/>
For example our query is "Where School" and we have two values to check agains:

- "**School** is cool, but **where** is the best one?"
  - Search hit
- "Is your **school** the best?"
  - No Search hit

## Algorithm: "hits" (default) {#searching-algorithm-hits}

```php
Query::SEARCH_ALGORITHM["hits"]
```

This is the default algorithm.<br/>
The score is based on the **amount of search hits**.

## Algorithm: "hits_prioritize" {#searching-algorithm-hits_prioritize}

```php
Query::SEARCH_ALGORITHM["hits_prioritize"]
```

The score is based on the **amount of search hits** but if two or more **documents have the same amount of search hits** the **order of the given fields** to search through is taken into consideration.<br/>
For example we search through two fields: ["title", "content"]<br/>
Then the title field gets more weight.<br/>
If one document has two hits in the title and three in the content field, it will have a lesser score than a document that has three hits in the title and two in the content field.

## Algorithm: "prioritize" {#searching-algorithm-prioritize}

```php
Query::SEARCH_ALGORITHM["prioritize"]
```

**The order of the fields** are a big part of the score.<br/>
For example we search through two fields: ["title", "content"]<br/>
If one document has one search hit in the title field it will have a higher score than a document that has no search hits in the title field but 61 search hits in the content field.

## Algorithm: "prioritize_position" {#searching-algorithm-prioritize_position}

```php
Query::SEARCH_ALGORITHM["prioritize_position"]
```

This algorithm does the same as the "prioritize" algorithm.<br/>
The difference is, that it also **consideres the position of the first search hit** when generating a score.<br/>
For example we have two documents with the same amount of search hits in the same fields.<br/>
The first one has the first search hit at position 3 and the second one at position 4. Then the first document will have a higher score.


## search() method of `QueryBuilder` class {#searching-query_builder}

Do a fulltext like search against one or multiple fields.

```php
function search(array|string $fields, string $query, array $options = []): QueryBuilder
```

### Parameters

1. # $fields: array|string
   An array containg all fields to search against.

    * "title"
    * ["title"]
    * ["title", "content", ...]

   As our documents are basically JSON documents it also could have nested properties.

   To target nested properties we use a single dot between the property/field name.

   **Example:** `address.street`

2. # $query: string
   The search query.

3. # $options: array
   Configure the search behaviour on a query by query base.

### Example 1

Search phrase: "SleekDB is the best database solution".<br/>
Search through all news articles "title", "description" and "content" fields, sort the result by relevance and dont return the score key field.

```php
$newsStore = new Store("news", __DIR__ . "/database");

$searchQuery = "SleekDB is the best database solution";

$result = $newsStore->createQueryBuilder()
  ->search(["title", "description", "content"], $searchQuery)
  ->orderBy(["searchScore" => "DESC"]) // sort result
  ->except(["searchScore"]) // exclude field from result
  ->getQuery()
  ->fetch();
```

### Example 2

Search phrase: "SleekDB is the best database solution".<br/>
Search through all news articles "title.mainTitle" and "content" fields, sort the result by relevance and exclude the score key from the result.<br/>
Also change the search algorithm to "prioritize" for this query and limit the result to 20.

```php
// Example article:
// [
//   "_id" => 1,
//   "title" => [
//     "mainTitle" => "SleekDB is the best database!"
//   ],
//   "content" => "A NoSQL dabase completely made with PHP.",
// ]

$newsStore = new Store("news", __DIR__ . "/database");

$searchQuery = "SleekDB is the best database solution";

$searchOptions = [
  "algorithm" => Query::SEARCH_ALGORITHM["prioritize"]
];

$result = $newsStore
  ->search(["title.mainTitle", "content"], $searchQuery, $searchOptions)
  ->orderBy(["searchScore" => "DESC"])
  ->except(["searchScore"])
  ->limit(20)
  ->getQuery()
  ->fetch();
```

## search() method of `Store` class {#searching-store}

Do a fulltext like search against one or multiple fields.

```php
function search(array $fields, string $query, array $orderBy = null, int $limit = null, int $offset = null): array
```

### Parameters

1. # $fields: array
   An array containg all fields to search against.

    * ["title"]
    * ["title", "content", ...]

   As our documents are basically JSON documents it also could have nested properties.

   To target nested properties we use a single dot between the property/field name.

   **Example:** `address.street`

2. # $query: string
   The search query.    

3. # $orderBy: array
   Sort the result by one or multiple fields.

    * ["name" => "asc"]
    * ["name" => "asc", "age" => "desc"]

4. # $limit: int
   Limit the result to a specific amount.

5. # $offset: offset
   Skip a specific amount of documents.

### Return value

Will return an `array` containg all documents having at least one search hit or an `empty array`.

### Example 1

Search phrase: "SleekDB is the best database solution".<br/>
Search through all news articles "title", "description" and "content" fields and sort the result by relevance.

```php
$newsStore = new Store("news", __DIR__ . "/database");

$searchQuery = "SleekDB is the best database solution";

$result = $newsStore->search(["title", "description", "content"], $searchQuery, ["searchScore" => "DESC"]);
```

### Example 2

Search phrase: "SleekDB is the best database solution".<br/>
Search through all news articles "title.mainTitle", "title.subTitle" and "content" fields and sort the result by relevance.

```php
// Example article:
// [
//   "_id" => 1,
//   "title" => [
//     "mainTitle" => "SleekDB is the best database!",
//     "subTitle" => "Just believe me!"
//   ],
//   "content" => "A NoSQL dabase completely made with PHP.",
// ]

$newsStore = new Store("news", __DIR__ . "/database");

$searchQuery = "SleekDB is the best database solution";

$result = $newsStore->search(
  ["title.mainTitle", "title.subTitle", "content"], // fields
  $searchQuery, // query
  ["searchScore" => "DESC"] // orderBy
);
```