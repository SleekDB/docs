<!--METADATA
{
    "title": "Release Notes",
    "url": "release-notes",
    "icon": "megaphone"
}
!METADATA-->

# 🎉 Release Notes

## 📢 Optimizations, new query methods and more control

SleekDB 2.0 comes with so many important optimizations and other features that make it faster and more mature. This is the recommended SleekDB release version for new projects, and if you are using an older version consider upgrading as soon as possible.

### Changes from 2.4 to 2.5

- # ✨ New conditions:
  The following conditions can now be used with the `findBy()`, `findOneBy()`, `deleteBy()`, `where()` and `orWhere()` methods!
  - `BETWEEN`
  - `NOT BETWEEN`

- # 🌈 SleekDB now accepts DateTime objects to filter data!
  The methods that accept DateTime objects as a value to check against are:
  - `findBy`
  - `findOneBy`
  - `deleteBy`
  - `where`
  - `orWhere`

  The conditions you can use DateTime objects with are:
  - `=`
  - `!=`
  - `>`
  - `>=`
  - `<=`
  - `IN`
  - `NOT IN`
  - `BETWEEN`
  - `NOT BETWEEN`

  Visit our new <a class="gotoblock" href="#/dates">Working with Dates documentation</a> page to learn more about date handling.

### Changes from 2.3 to 2.4

- # 🚨 Deprecated in() & notIn() methods
They will be removed with the next major update.<br/>
Please **use the new conditions `"in"` and `"not in"` instead**.<br/>
Available with the `findBy()`, `findOneBy`, `deleteBy()`, `where()` and `orWhere()` methods since version 2.3.<br/>
See <a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/118" target="_blank">#118</a> for more details.

### Changes from 2.2 to 2.3

- # 🚨 Deprecated nestedWhere() method
We are sorry to deprecate the `nestedWhere()` method so short after it's release.<br/>
It will be removed with the next major update.<br/>
Please use `where()` and `orWhere()` instead.

- # 🌟 Nested where statements everywhere!
The `findBy()`, `findOneBy`, `deleteBy()`, `where()` and `orWhere()` methods now can handle nested statements!

  <table>
    <tr>
      <td>
        <ul>
          <li><code>findBy()</code></li>
          <li><code>findOneBy()</code></li>
        </ul>
      </td>
      <td>
        <a class="gotoblock" href="#/fetch-data">Fetch Data documentation</a>
      </td>
    </tr>
    <tr>
      <td>
        <ul>
          <li><code>deleteBy()</code></li>
        </ul>
      </td>
      <td>
        <a class="gotoblock" href="#/delete-data">Delete Data documentation</a>
      </td>
    </tr>
    <tr>
      <td>
        <ul>
          <li><code>where()</code></li>
          <li><code>orWhere()</code></li>
        </ul>
      </td>
      <td>
        <a class="gotoblock" href="#/query-builder">QueryBuilder documentation</a>
      </td>
    </tr>
  </table>

- # ✨ New conditions

  - `not like`
  - `in`
  - `not in`

  These new conditions can now be used with the `findBy()`, `findOneBy()`, `deleteBy()`, `where()` and `orWhere()` methods!

- # 💡 Logical connection
All condition methods are now logically connected and the order when using them is now important.<br/>
See <a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/114" target="_blank">#114</a> for more details.

### Changes from 2.1 to 2.2

- # 🗃 Order by multiple fields
Now the `orderBy()` method of the QueryBuilder accepts multiple fields to sort the result.<br/>
Look at the updated orderBy section of the <a class="gotoblock" href="#/query-builder">QueryBuilder documentation</a> to learn more.

- # 🔍 New nestedWhere() method
With the new added `nestedWhere()` method you can now use much complexer where statements to filter data.<br/>
Look at the new nestedWhere section of the <a class="gotoblock" href="#/query-builder">QueryBuilder documentation</a> to learn more.

### Changes from 2.0 to 2.1

- # ⚙️ New Configuration
With the new `primary_key` configuration you can now change the _id key name to everything you want. To see how to use the new configuration option visit the <a class="gotoblock" href="/#/configurations">Configurations</a> page.

### Changes from 1.5 to 2.0

- # 🔎 Improving document discovery process

  Added better file locking support with reduced nested loops for finding documents. Added methods that can be used to easily find a document without searching for entire available JSON files of a store.

- # Support for PHP >= 7.0

  The support of PHP 5 is dropped to provide more modern features.

- # ✨ New query methods

  - `first()`
  - `exists()`
  - `select()`
  - `except()`
  - `distinct()`
  - `join()`
  - `findAll()`
  - `findById()`
  - `findBy()`
  - `findOneBy()`
  - `updateBy()`
  - `deleteBy()`
  - `deleteById()`

- # Improved Code Quality

  We isolated most of the existing logics into different classes to make the codebase easy to understand and easy to maintain.

- # Unit Testing

  There was some concern among developers as it was lacking UNIT Testing, now its added!

- # SleekDB class now deprecated

  We beliefe that downwards compatibility is very important.
  
  That's why we always try our best to keep SleekDB as downwards compatible as possible and avoid breaking changes.
  
  Unfortunatelly we had to refactor and rewrite the whole SleekDB project to make it future proof and keep it maintainable. As a consequence the SleekDB class is now deprecated with version 2.0 and will be removed with version 3.0.

  ### A new era for SleekDB begins!

- # Better Caching Solution

  Data caching has been improved significantly.

  - Add custom expiry of a cache file with lifetime.
  - Ability of query specific caching rules.

## Issues

- Deprecate SleekDB Class (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/84" target="_blank">#84</a>)
- first and exists methods does not use cache (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/82" target="_blank">#82</a>)
- Make where and orWhere just accept one array (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/80" target="_blank">#80</a>)
- Make dataDir required (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/79" target="_blank">#79</a>)
- Add update & delete to Store class <a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/78" target="_blank">#78</a>)
- Change delete method of Query class to accept return options (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/77" target="_blank">#77</a>)
- Add find methods to new Store class to make the QueryBuilder optional for simple queries. (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/75" target="_blank">#75</a>)
- Allow to query on read-only file system (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/67" target="_blank">#67</a>)
- Use "results" property instead of returning data from methods (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/59" target="_blank">#59</a>)
- Return the first item only (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/56" target="_blank">#56</a>)
- Check if data exists (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/54" target="_blank">#54</a>)
- update return value (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/51" target="_blank">#51</a>)
- delete status (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/48" target="_blank">#48</a>)
- Extend not possible due to private helper methods (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/44" target="_blank">#44</a>)
- JOIN feature (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/42" target="_blank">#42</a>)
- Return distincted value (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/41" target="_blank">#41</a>)
- Suppress Key on fetch (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/38" target="_blank">#38</a>)
- Like Condition (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/34" target="_blank">#34</a>)
- Better code base (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/32" target="_blank">#32</a>)
- OR Condition Query (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/31" target="_blank">#31</a>)
- Possibility of duplicate ID’s (<a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/issues/30" target="_blank">#30</a>)
