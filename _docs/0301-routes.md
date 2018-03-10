---
title: Routes
toc: true
---
Any endpoint in the CMS needs to registered as a Route. It instructs the GUI to create a new Rest API endpoint for a set of content Groups and Fields.

## Creating a Route

```php
use Agencms\Core\Route;

// Collection (default)
$route = Route::init($slug, $name, (optional) $endpoints, (optional) $type);

// Single helper shortcut
$route = Route::initSingle($slug, $name, (optional) $endpoints);
```

### Naming Routes

The Route name is what will be displayed in the CMS Menu. You may supply an optional Section title to display if you wish to group multiple routes within a sub-menu.

```php
$name = ['Blog' => 'Articles'];
```

The above example will display an `Articles` item inside a `Blog` menu. If, as in this example, `Articles` is the only registered item for `Blog`, the CMS will simply display `Articles` as the main menu item because there is no need to have a sub-menu. This is useful for future-proofing your Routes.

### Route Types

|Type|Description|
|---|---|
|Collection (default)|A list of items which can be edited and created|
|Single|A single endpoint which renders groups and fields|

### Rest API Endpoints

Agencms will generate the endpoints automatically based on the slug, following typical Rest controller methods. You can optionally specify your own endpoints, especially when not all methods are available.

The default endpoints created are:

|Method|URL|
|---|---|
|GET|/api/{$slug}/{$id?}|
|PUT|/api/{$slug}|
|POST|/api/{$slug}/{$id}|
|DELETE|/api/{$slug}/{$id}|

To manually specify route endpoints supply an array with replacements.

```php
$endpoints = [
    'GET' => '/custom/get/url',
    'DELETE' => '/custom/delete/url',
];
```

## Registering a Route

To actually register a Route with Agencms simply call the `registerRoute` method on the Agencms Facade.

```php
use Agencms\Core\Facades\Agencms;

Agencms::registerRoute(Route $route);
```

## Appending Routes

It is also possible to add content to existing Routes. This is typically required when working with existing plugins where you wish to add custom functionality. Instead of calling `init|initSingle` on the group, use the `load` method to indicate that you are appending to an existing Route.

```php
$route = Route::load($slug)->...;

Agencms::appendRoute(Route $route);
```
