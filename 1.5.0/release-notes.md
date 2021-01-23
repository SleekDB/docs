<!--METADATA
{
    "title": "Release Notes",
    "url": "release-notes",
    "icon": "megaphone"
}
!METADATA-->

# 🎉 Release Notes

## 📢 Optimizations, new clause methods and more control

SleekDB 1.5.0 comes with few important optimizations and with other features that will make it more easier to manage data. This is a recommended update if you are using an older version of SleekDB.

### No breaking changes from 1.0.3 to 1.5.0

- # 💪 Improving document discovery process

  Starting from version 1.5.0 SleekDB will discover new documents without affecting the \_id serializer, that means it will need to do a lot less work than before. That makes it more faster less memory hungry than ever before.

- # 🐍 orWhere() clause method

  It will work as the OR condition of SQL. SleekDB supports multiple orWhere as object chain.

  ```php
  $user = $usersDB->where( 'products.totalSaved', '>', 10 )
      ->orWhere( 'products.totalBought', '>', 20 )
      ->orWhere( 'products.shipped', '=', 1 )
      ->fetch();
  ```

- # 😍 in() clause method

  It work like the IN clause of SQL. SleekDB supports multiple in() as object chain for different fields.

  ```php
  $user = $usersDB
      ->in('country', ['BD', 'CA', 'SE', 'NA'])
      ->in('products.totalSaved', [100, 150, 200])
      ->fetch();
  ```

- # 😎 notIn() clause method

  It works as the opposite of in() method.

  It will filter out all documents that has particular data items from the given array. SleekDB supports multiple notIn as object chain for different fields.

  ```php
  $user = $usersDB
      ->notIn('country', ['IN', 'KE', 'OP'])
      ->notIn('products.totalSaved', [100, 150, 200])
      ->fetch();
  ```

- # 🤗 Keeping Query State

  If you want to keep the condition parameters for query and perform additional operations then you may want to use the keepConditions() method.

  Here is an example showing how you may fetch data and then update on the discovered documents without running an additional query:

  ```php
  // Find documents.
  $result = $usersDB
      ->keepConditions() // Won't reset the active query state.
      ->where('products.totalBought', '>', 0)
      ->where('products.totalSaved', '>', 0);

  // Fetch data.
  $result->fetch();

  // Update matched documents.
  $result->update([
      'someRandomData' => '123',
  ]);
  ```

Github release note: <a rel="noopener nofollow" href="https://github.com/rakibtg/SleekDB/releases/tag/1.5.0" target="_blank">https://github.com/rakibtg/SleekDB/releases/tag/1.5.0</a>
