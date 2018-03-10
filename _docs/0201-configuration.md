---
title: Configuration
toc: true
---
## Media Library

Agencms ships with a media library [thesold/laravel-media-manager](https://github.com/thesold/laravel-media-manager) for managing Image assets.

You can publish the configuration file if you wish or update required variables in your `.env` file.

### Default Driver

The default configuration doesn't require any `.env` variables, but they are shown here, should you want to include them in your source control.

```sh
LARAVELMEDIAMANAGER_DRIVER=default
LARAVELMEDIAMANAGER_CLOUD_NAME=cloud_name
```

### Cloudinary Driver

To use the [Cloudinary](https://cloudinary.com/) Image Library you need to configure some environment variables.

```sh
LARAVELMEDIAMANAGER_DRIVER=cloudinary
LARAVELMEDIAMANAGER_CLOUD_NAME=cloud_name
LARAVELMEDIAMANAGER_API_KEY=api_key
LARAVELMEDIAMANAGER_API_SECRET=api_secret
```

## Users

The standard installation assumes that you are using the default `App\User` model. If you are using a custom implementation please update your `.env` to point the Authentication package to your User model.

Again, if preffered, publish the configuration for the [Silvanite/Brandenburg](https://github.com/Silvanite/brandenburg) package, which is used under the hood to power authentication and authorization or set the required `.env` variables.

```sh
USER_MODEL="App\\User"
```
