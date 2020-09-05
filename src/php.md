# PHP

## Setup

- Local Environment

  - `http://localhost/phpmyadmin/`
  - "/xampp/htdocs/" is the server root.
  - Any folder created inside /htdocs/ is a route on "localhost".
  - "index.php" inside /htdocs/ is loaded automatically

- Add Password (Need to user this password when accessing MySQL in script)

  1. Navigate to "localhost/phpmyadmin", click `User Accounts` -> `root localhost` -> `Edit Privileges` -> `Change Password`
  2. Navigate to "/xampp/phpMyAdmin/", find "config.inc.php" -> "$cfg['Servers'][$i]['password']" and change 'password' to match step 1.

- Package Management (Composer)
  - [Download Composer](https://getcomposer.org/download/)
  - Add a `composer.json` file
  - [Download](https://getcomposer.org/composer.phar) `composer.phar` file
  - Search for packages at [Packagist.org](https://packagist.org/) and add them to `composer.json`
  - Run `php composer.phar install`
  - Add `vendor/` to `.gitignore`
  - [Heroku Deploy](https://devcenter.heroku.com/articles/deploying-php)
  - See PHP [workflow process](https://www.codementor.io/jadjoubran/php-tutorial-getting-started-with-composer-8sbn6fb6t)

## Intro

- Must run on a server, usually Apache
- Can be embedded directly into HTML with `<?php` and `?>`
  - Such files MUST have a .php file extension
  - File is processed on server with PHP Interpreter, and the processed file (usually HTML) is sent to client.
- Framework: [Laravel](https://laravel.com/)

## Variables

- Must prefix with `$`
- Must start with a character or \_
- Case sensitive

## Data Types

- String, Integer, Float, Boolean, Array, Object, NULL, Resource.
- Concatenate Strings with period `.`
- Concatenate Strings literately, after parsing variables, with double quotes `""`
- Escape character with `\` as needed
- `define('name', 'value', [opt. case-sensitivity])` is used to create Constants
  <!-- https://www.youtube.com/watch?v=9p9siksrSoU&list=PLillGF-Rfqbap2IB6ZS4BBBcYPagAjpjn&index=4 -->

## Form

- `htmlentities()` prevents script injection.
- `$_GET()` methods sends forms through URL
- `$_POST()` methods sends forms through request `header`
- Can embed request string directly into url by using `<a>` with `href="page.php?name=value"` and output with `<?php echo "{$name}"; ?>`
