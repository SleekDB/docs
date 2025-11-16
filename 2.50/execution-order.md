<!--METADATA
{
    "title": "Order of Execution",
    "url": "execution-order",
    "icon": "git-network"
}
!METADATA-->

# Order of Execution

## Summary

- <a class="gotoblock" href="#/execution-order#general-life-cycle">General Life Cycle</a>
- <a class="gotoblock" href="#/execution-order#query">Order of query execution</a>
   1. <a class="gotoblock" href="#/execution-order#query-where">WHERE</a>
   2. <a class="gotoblock" href="#/execution-order#query-distinct">DISTINCT</a>
   3. <a class="gotoblock" href="#/execution-order#query-join">JOIN</a>
   4. <a class="gotoblock" href="#/execution-order#query-search">SEARCH</a>
   5. <a class="gotoblock" href="#/execution-order#query-select1">SELECT - Step 1</a>
   6. <a class="gotoblock" href="#/execution-order#query-groupBy">GROUP BY</a>
   7. <a class="gotoblock" href="#/execution-order#query-select2">SELECT - Step 2</a>
   8. <a class="gotoblock" href="#/execution-order#query-having">HAVING</a>
   9. <a class="gotoblock" href="#/execution-order#query-except">EXCEPT (Remove fields)</a>
   10. <a class="gotoblock" href="#/execution-order#query-orderBy">ORDER BY</a>
   11. <a class="gotoblock" href="#/execution-order#query-skip">SKIP</a>
   12. <a class="gotoblock" href="#/execution-order#query-limit">LIMIT</a>

## General Life Cycle {#execution-order-general-life-cycle}

Each query will follow the below steps or execution life cycle.

1. # Store

   The `Store` class is the first, and in most cases also the only part you need to come in contact with.

2. # Query Builder

   The `QueryBuilder` class is used to prepare a query. It can be retrieved with `Store->createQueryBuilder()`.

3. # Query

   The `Query` class contains all information needed to execute the query and is used to do so.<br/>
   The `Query object` can be retrieved using the `QueryBuilder->getQuery()` method.

4. # Cache

   The `Cache` class handles everything regarding caching. It will decide when to cache or not to cache.<br/>
   It is mainly designed for internal use but can be retrieved using `Query->getCache()`.

## Order of query execution {#execution-order-query}

The following diagram represents the internal order of execution when using the `Query->fetch()` method.

  - <a class="gotoblock" href="#/query#fetch">Query->fetch()</a>

<div style="max-width: 400px; padding-left: 15px; padding-right: 15px;">
   <a href="/assets/query-execution-order.png">
      <img src="/assets/query-execution-order.png" style="width: 100%;" />
   </a>
</div>

### Single document 

1. # WHERE {#execution-order-query-where}

   The `where` and `orWhere` conditions are used to determine if a document will be a part of the result set or not.
   - <a class="gotoblock" href="#/query-builder#where">QueryBuilder->where()</a>
   - <a class="gotoblock" href="#/query-builder#orWhere">QueryBuilder->orWhere()</a>

2. # DISTINCT {#execution-order-query-distinct}

   If the `WHERE` resulted in `true` the document is checked against the result set using the fields defined with the `distict()` method.
   - <a class="gotoblock" href="#/query-builder#distinct">QueryBuilder->distinct()</a>

### All documents in result set

   Now we have all documents in a result array.

3. # JOIN {#execution-order-query-join}

   All documents will be joined with one or multiple stores.
   - <a class="gotoblock" href="#/query-builder#join">QueryBuilder->join()</a>

4. # SEARCH {#execution-order-query-search}

   We go through all documents and check if they contain what we search, if not we remove them from the result array.
   - <a class="gotoblock" href="#/query-builder#search">QueryBuilder->search()</a>

5. # SELECT - Step 1 {#execution-order-query-select1}

   Apply select functions that do not reduce the result amount to one and apply their aliases.
   - <a class="gotoblock" href="#/query-builder#select">QueryBuilder->select()</a>

6. # GROUP BY {#execution-order-query-groupBy}

   Group documents by fields defined with `groupBy()`.
   - <a class="gotoblock" href="#/query-builder#groupBy">QueryBuilder->groupBy()</a>

7. # SELECT - Step 2 {#execution-order-query-select2}

   Select specific fields and apply their aliases, if they have one.<br/>
   Apply select functions that reduce result amount to one and their aliases.<br/>
   **Amount will not be reduced when using Group-By.**
   - <a class="gotoblock" href="#/query-builder#select">QueryBuilder->select()</a>

   Functions that reduce the amount of the result set to one:
   - `SUM`
   - `MAX`
   - `MIN`
   - `AVG`

8. # HAVING {#execution-order-query-having}

   Check all documents using the criteria specified with `having`.
   - <a class="gotoblock" href="#/query-builder#having">QueryBuilder->having()</a>

9. # EXCEPT (Remove fields) {#execution-order-query-except}

   Exclude/ Remove fields from all documents.
   - <a class="gotoblock" href="#/query-builder#except">QueryBuilder->except()</a>

10. # ORDER BY {#execution-order-query-orderBy}

   Sort the result array using one or multiple fields.
   - <a class="gotoblock" href="#/query-builder#orderBy">QueryBuilder->orderBy()</a>

11. # SKIP {#execution-order-query-skip}

   Skip a specific amount of documents. (Slice the result array)
   - <a class="gotoblock" href="#/query-builder#skip">QueryBuilder->skip()</a>

12. # LIMIT {#execution-order-query-limit}

   Limit the amount of documents in result. (Slice the result array)
   - <a class="gotoblock" href="#/query-builder#limit">QueryBuilder->limit()</a>