---
title: Installation
toc: true
---
Throughout this documentation we assume that you are working with a fresh Laravel installation and are familiar with the Laravel Framework. You will need to ensure that you have a valid database configured in your `.env`.

The default base install of Agencms includes the **Auth** plugin for Users, Authentication and Authorization using Roles and Permissions. The CMS comes pre-made with views, models and controllers to manage all this.

## Installing with composer

Adding Agencms is super simple by requiring the core package.

```sh
composer require agencms/core
```

### Migrations

All migrations are ready to go and there is no need to publish them first, however you can publish them if you wish to review them, keep them in your repository or make changes. You can publish them.

```sh
php artisan vendor:publish
```

Run the migrations to install the required tables

```sh
php artisan migrate
```

### Default User

In order to access the CMS a user must be active in the system. We have provided a console command to create new users to assist with this process and make it easy to get up-and-running as quickly as possible.

```sh
php artisan agencms:make:user
```

Follow the prompts for the user details.

**Congratulations! You are now ready to start using Agencms.**