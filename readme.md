# Introduction #

This is a small wrapper to Composer class loader intended to add functionality similar to static initializers in Java.
You can define static method named `__constructStatic()` and it'll be invoked first time class loaded into project.

# Reqirements #

No external libraries required, just use Composer autoloader in your project.

# Installation #

Add to `composer.json` `require` block: `"vladimmi/construct-static": "dev-master@dev"`

# Usage #

```php
$composer = require_once(__DIR__ . '/vendor/autoload.php');     //get Composer loader
$loader = new ConstructStatic\Loader($composer);                //wrap it   
```

# Details #

Note that autoloaders list is cleared on wrapping and replaced with this loader. Then all class load calls go
through it to Composer. You can use `$loader` from them sample above as original `$composer` object - all methods calls\
are proxied to wrapped loader.

# Options #

## Process previously loaded classes ##

If you want to call static constructors on classes that were loaded before wrapper created, you can use `processLoaded`
constructor parameter to do this:

```php
$composer = require_once(__DIR__ . '/vendor/autoload.php');     //get Composer loader
$loader = new ConstructStatic\Loader($composer, true);          //wrap it and process already loaded classes   
```