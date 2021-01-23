<!--METADATA
{
    "title": "Release Notes",
    "url": "release-notes",
    "icon": "megaphone"
}
!METADATA-->

# 🎉 Release Notes

## 📢 Optimizations, new query methods and more control

SleekDB 2.0 comes with so many important optimizations and other features that make it faster and more mature. This is the recommended SleekDB release version for new project, and if you are using an older version consider upgrading as fast as possible.

### Breaking changes from 1.5 to 2.0

- # 🔎 Improving document discovery process

  Added better file locking support with reduced nested loops for finding documents. Added methods that can be used to easily find a document without searching for entire available JSON files of a store.

- # Support for PHP >= 7.0

  The support of PHP 5 is dropped to provide more modern features.

- # ✨ Added new query methods

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

  Although, we are providing downwards compatibility in this version, but will remove the old SleekDB class with version 3.0.

- # Better Caching Support

  Data caching has been improved,

  - Added custom expiry of a cache file.
  - Allows query specific caching rules.

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
