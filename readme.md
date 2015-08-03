# Introduction #

This is a small wrapper to Composer class loader intended to add functionality similar to static initializers in Java.
You can define static method named `__constructStatic()` and it'll be invoked first time class loaded into project.

Example:

```php
class MyTestClass
{
    private static function __constructStatic()
    {
        //this will be called once after class loaded
    }
}
```

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
through it to Composer. You can use `$loader` from the sample above as original `$composer` object - all methods calls
are proxied to wrapped loader.

# Options #

## Process previously loaded classes ##

If you want to call static constructors on classes that were loaded before wrapper created, you can use `processLoaded`
constructor parameter to do this:

```php
$composer = require_once(__DIR__ . '/vendor/autoload.php');     //get Composer loader
$loader = new ConstructStatic\Loader($composer, true);          //wrap it and process already loaded classes   
```

## Pass custom data to called constructors

You can pass some data to called constructors - for example, inject services or pass DI container. To do this you need
to modify constructor a bit:

```php
class MyTestClass
{
    //Added $params parameter
    private static function __constructStatic($params = [])
    {
        //this will be called once after class loaded
    }
}
```

Then you can set needed data when creating wrapping loader:

```php
$composer = require_once(__DIR__ . '/vendor/autoload.php');         //get Composer loader
$params = [
    //...set any needed data here
];
$loader = new ConstructStatic\Loader($composer, false, $params);    //wrap it
```