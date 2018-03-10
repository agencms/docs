---
title: Example Setup
toc: true
---
With your Agencms installation all ready to go, here is a simple example of how to create custom content in your new CMS.

## AgencmsHandler

This is the pre-configured location where you can place your configuration, however you are free to split more complex setups into multiple files or move it whereever you want.

```php
<?php

namespace App\Handlers;

use Silvanite\Agencms\Route;
use Silvanite\Agencms\Field;
use Silvanite\Agencms\Group;
use Silvanite\Agencms\Option;
use Silvanite\Agencms\Relationship;
use Illuminate\Support\Facades\Auth;
use Illuminate\Support\Facades\Gate;
use App\Http\Middleware\AgencmsConfig;
use Silvanite\Agencms\Facades\Agencms;
use Illuminate\Support\Facades\Route as Router;

class AgencmsHandler
{
    /**
     * This is a helper method used by your service provider to actually register
     * your Agencms Plugin. You should update the name to something unique to you
     * to avoid potential conflicts with official or third-party plugins.
     *
     * @return void
     */
    private static function register()
    {
        Router::aliasMiddleware('agencms.plugin.name', AgencmsConfig::class);
        Agencms::registerPlugin('agencms.plugin.name');
    }

    /**
     * Register all routes and models for the Admin GUI (AUI). Avoid renaming this
     * method as it is called by the Agencms Middleware.
     *
     * @return void
     */
    public static function registerAdmin()
    {
        /**
         * Only allow access to users with the admin_access priviledge.
         * User without access won't see this area of the CMS at all.
         */
        if (!Gate::allows('admin_access')) {
            return;
        }

        self::registerAgencms();
    }

    /**
     * Register the Agencms endpoints for this Application.
     * This is typically where your main configuration is placed.
     *
     * @return void
     */
    private static function registerAgencms()
    {
        // If you created a custom permission, apply it here.
        if (!Gate::allows('my_permission')) {
            return;
        }

        // Create your custom Route, Group and Fields.
        Agencms::registerRoute(
            Route::init('my_route', ['Menu Section' => 'Menu Item'], '/end/point')
                ->icon('person')
                ->addGroup(
                    Group::large('My Section')->addField(
                        Field::string('my-key', 'My Label')->required()->list()
                    )
                )
        );
    }
}
```
