<!--METADATA
{
    "title": "Release Notes",
    "url": "release-notes",
    "icon": "megaphone"
}
!METADATA-->

# 🎉 Release Notes

## 📢 Optimizations, new query methods and more control

SleekDB 2.0 comes with so many important optimizations and with other features that makes it faster and more mature. This is a recommended release if you are using an older version of SleekDB.

### Breaking changes from 1.5 to 2.0

- # Improving document discovery process

  Added better file locking support with reduced nested loops for finding documents. Added methods that can be used to easily find a document without searching for entire available JSON files of a store.

- # Added new query methods

  - `first()`
  - `exists()`
  - `select()`
  - `except()`
  - `distinct()`
  - `join()`

- # Improved Code Quality

  We isolated most of the existing logics into different classes to make the codebase easy to understand and easy to maintain.

- # Unit Testing

  There was some concern among developers as it was lacking UNIT Testing, now its added!

- # SleekDB class now deprecated

  Although, we are providing downwards compatibility in this version, but will remove the old SleekDB object with version 3.0.

- # Better Caching Support

  Data caching has been improved,

  - Added custom expiry of a cache file.
  - Allows query specific caching rules.

## Issues

- Deprecate SleekDB Class (#84)
- first and exists methods does not use cache (#82)
- Make where and orWhere just accept one array (#80)
- Make dataDir required (#79)
- Add update & delete to Store class (#78)
- Change delete method of Query class to accept return options (#77)
- Add find methods to new Store class to make the QueryBuilder optional for simple queries. (#75)
- Allow to query on read-only file system (#67)
- Use "results" property instead of returning data from methods (#59)
- Return the first item only (#56)
- Check if data exists (#54)
- update return value (#51)
- delete status (#48)
- Extend not possible due to private helper methods (#44)
- JOIN feature (#42)
- Return distincted value (#41)
- Suppress Key on fetch (#38)
- Like Condition (#34)
- Better code base (#32)
- OR Condition Query (#31)
- Possibility of duplicate ID’s (#30)
