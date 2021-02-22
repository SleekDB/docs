<!--METADATA
{
    "title": "Features",
    "url": "features",
    "icon": "filing"
}
!METADATA-->

# Features

## Installing SleekDB

- # ⚡ Lightweight & Fast

  Stores data in plain-text utilizing JSON format, no binary conversion needed to store or fetch the data. Default query cache layer.

- # 🔆 Schema free data storage

  SleekDB does not require any schema, so you can insert any types of data you want.

- # 🔍 Query on nested properties

  As it supports schema free data, so you can filter and use conditions on nested properties of the JSON documents!

  If you write this where clause:

  ```php
  where( 'post.author.role', '=', 'admin' )
  ```

  SleekDB will look for data at:

  ```json
  {
    "post": {
      "author": {
        "role": "admin"
      }
    }
  }
  ```

- # ✨ Dependency free, only needs PHP to run

  Supports PHP 7+. Requires no third-party plugins or software.

- # 🚀 Default caching layer

  SleekDB will serve data from cache by default and regenerate cache automatically! Query results will be cached and later reused from a single file instead of traversing all the available files.

- # 🌈 Rich Conditions and Filters

  Use multiple conditional comparisons, text search, sorting on multiple properties and nested properties.

  <table>
    <tbody>
      <tr>
        <td valign="top">
          <ul>
            <li>where</li>
            <li>orWhere</li>
            <li>select</li>
            <li>except</li>
            <li>in</li>
            <li>not in</li>
          </ul>
        </td>
        <td valign="top">
          <ul>
            <li>join</li>
            <li>like</li>
            <li>sort</li>
            <li>skip</li>
            <li>orderBy</li>
            <li>update</li>
          </ul>
        </td>
        <td valign="top">
          <ul>
            <li>limit</li>
            <li>search</li>
            <li>distinct</li>
            <li>exists</li>
            <li>first</li>
            <li>delete</li>
          </ul>
        </td>
        <td valign="top">
          <ul>
            <li>like</li>
            <li>not like</li>
            <li>between</li>
            <li>not between</li>
            <li>group by</li>
            <li>having</li>
          </ul>
        </td>
      </tr>
    </tbody>
  </table>

- # 👍 Process data on demand

  SleekDB does not require any background process or network protocol in order to process data when you use it in a PHP project. All data for a query will be fetched at runtime within the same PHP process.

- # 😍 Runs everywhere

  Runs perfectly on shared-servers or VPS too.

- # 🍰 Easy to learn and implement

  SleekDB provides a very simple elegant API to handle all of your data.

- # 🍰 Easily import/export or backup data.

  SleekDB use files to store information. That makes tasks like backup, import and export very easy.

- # 💪 Actively maintained

  SleekDB is created by <a rel="noopener nofollow" href="https://twitter.com/rakibtg" target="_blank">@rakibtg</a> who is using it in various types of applications which are in production right now. Our other contributor and active maintainer is <a rel="noopener nofollow" href="https://www.goodsoft.de" target="_blank">Timucin</a> who is making SleekDB much better in terms of code quality and new features.

- # 💌 Support

  For support and other requirements <a class="gotoblock" href="/#/contact">contact</a>.
