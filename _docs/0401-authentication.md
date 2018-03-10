---
title: Users, Authentication and Authorization
toc: true
---
Agencms ships with Users, Roles and permissions out of the box. Typically all official plugins come with CRUD based permissions and we encourage maintaining this structure for custom configurations and when developing your own plugins.

## Defaults

Not everyone needs Roles or Permissions, and especially for smaller sites with a single user, or when getting started, there is no need to make use of them.

Agencms works an open-by-default policy which means unless a user is specifically granted access to a permission, all users have access to it.

That means when you first install Agencms, the default user has access to everything.

### Permissions

```php
[
    'users_read',
    'users_update',
    'users_create',
    'users_delete',
    'roles_read',
    'roles_update',
    'roles_create',
    'roles_delete',
]
```

## Creating permissions

Permissions are created using standard Laravel Gate policies which can then be assigned to Groups in the CMS.

```php
// app/Providers/AppServiceProvider.php

use Agencms\Auth\ValidatesPermissions;

public function register()
{
    Gate::define($permission = 'my-permission', function ($user) use ($permission) {
        // Remove the following block if you want explicit access to be granted
        if ($this->nobodyHasAccess($permission)) {
            return true;
        }

        return $user->hasRoleWithPermission($permission);
    });
}
```

### Enforcing permissions

You may simply use the standard Laravel provided methods to check if a permission is given to the logged in user.

```php
use Illuminate\Support\Facades\Gate;

...

if (Gate::allows('my-permission')) {
    // access granted
}
```

Or using middleware helpers

```php
Route::get('/my/{model}', function (MyModel $model) {
    // The current has access
})->middleware('can:my-permission');
```