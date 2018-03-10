---
title: Basic Setup
toc: true
---
To actually setup your own content structure there is are a handlful of files to prepare. We have included an artisan command to assist with this and essentially all you need to do manually, before adding your content configuration, is to add a single line to your `AppServiceProvider`.

## Running the Artisan Command

Running the `install` artisan command will create a few boilerplate files for you with some dummy content for refernce (typically commented out).

```sh
php artisan agencms:install
```

The following files will be created. They assume you kept the default `App` namespace. If not you will need to rename this where applicable.

```php
"app\Handlers\AgencmsHandler.php" // this is where you configure your CMS
"app\HttpMiddleware\AgencmsConfig.php" // there is no need to edit this file
```

The command is non-destructive, so there is no need to worry about running it multiple times by accident.

## Registering your Agencms plugin

Within your `AppServiceProvider` register your Agencms Configuration.

```php
// app\Providers\AppServiceProvider.php

...

public function register()
{
    \App\Handlers\AgencmsHandler::register();
}

...
```

## Play time

You are now all setup and ready to write your CMS configuration inside the `AgencmsHandler.php`. **Enjoy.**
