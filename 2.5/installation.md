<!--METADATA
{
    "title": "Installation",
    "url": "installation",
    "icon": "play"
}
!METADATA-->

# Installation

To use SleekDB make sure that you have PHP up and running in your system, and SleekDB has write and/or read permission.

## Requirements

- PHP >= 7.0
- ext-json

## Composer Installation (recommended)

To install SleekDB using composer, open a terminal, cd into your project root directory where "composer.json" file lives and run this:

```sh
composer require rakibtg/sleekdb
```

SleekDB should be auto loaded into your app using the composer.
Find SleekDB on **<a rel="noopener nofollow" href="https://packagist.org/packages/rakibtg/sleekdb" target="_blank">packagist</a>**.

## Install Manually (without composer)

- <a rel="noopener nofollow" href=" https://github.com/rakibtg/SleekDB/archive/master.zip">Download</a> the latest version and extract the ZIP file inside your project directory.
- Import the SleekDB.php file where you want to use SleekDB.

  Example:

  ```php
  require_once "../SleekDB/Store.php";
  ```

To download older versions please check the <a rel="noopener nofollow" target="_blank" title="Click here to download old versions" href="https://github.com/rakibtg/SleekDB/releases">releases</a>.

## Using dev version (Not recommended)

We do a lot of experiments on the dev branch, which are not properly documented and the API will not be consistence in some cases. Use from `dev` branch if you have any specific reason or if you want play around with it.

```sh
composer require rakibtg/sleekdb dev-dev
```
