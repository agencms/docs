---
title: Hello World Agencms Tutorial
toc: true
---
```sh
laravel new hello-agencms

cd hello-agencms
```

Configure your Database and environment.

Configure your Cloudinary environment settings.

```sh
LARAVELMEDIAMANAGER_DRIVER=cloudinary
LARAVELMEDIAMANAGER_CLOUD_NAME=cloud_name
LARAVELMEDIAMANAGER_API_KEY=api_key
LARAVELMEDIAMANAGER_API_SECRET=api_secret
```

```sh
composer require agencms/core

php artisan migrate

php artisan agencms:make:user
```

If you are  using valet we recommend running via SSL `valet secure`.

Configure your Portal

`https://portal.agencms.com/#/core/admin`

* Create a new site
* Give it a name `Hello Agencms`
* The slug should be automatically completed to `hello-agencms`
* Save

Visit `https://portal.agencms.com/#/TENANT-SLUG/hello-agencms`. If you are not running SSL locally, use `http://` in the URL.

Log in with your credentials and you should now have access to the Agencms GUI. Time to start building your site or application.
