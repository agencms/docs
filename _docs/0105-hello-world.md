---
title: Hello World Agencms Tutorial
toc: false
---
This classic hello world tutorial is a complete walk-through to get a new Agencms up and running with a new Laravel installation. Some of the steps are already covered in the installation and configuration documentation, however this tutorial will go through each step for clarity.

## Installation

### Setup a new  Laravel project

We all know this one ...

```sh
laravel new hello-agencms

cd hello-agencms
```

* Configure your Database and environment.
* Configure your Cloudinary environment settings.

```sh
LARAVELMEDIAMANAGER_DRIVER=cloudinary
LARAVELMEDIAMANAGER_CLOUD_NAME=cloud_name
LARAVELMEDIAMANAGER_API_KEY=api_key
LARAVELMEDIAMANAGER_API_SECRET=api_secret
```

### Install Agencms

Now we have a shiny new Laravel installation up and running, it's time to install the CMS.

```sh
composer require agencms/core
```

We'll need to run some new migrations.

```sh
php artisan migrate
```

Finally, setup a new admin user so we can log-in.

```sh
php artisan agencms:make:user
```

### Configure your Agencms Portal (GUI)

If you are using valet we recommend running via SSL `valet secure`.

Visit `https://portal.agencms.com/#/core/admin`. This is where you can configure all your Agencms Sites. Nothing will work without a site configured here, the hosted admin GUI. The only time you need to access the Agencms Core Admin Portal is to setup new, or update, sites.

Log-in with your Agencms Tenant Credentials (not the site admin credentials you just created).

* Create a new site
* Give it a name `Hello Agencms`
* The slug should be automatically completed to `hello-agencms`
* Select your Tenant account to associate this site with
* Make sure you select the `Active` toggle
* Save

If you want to customise your Tenant name/slug, select Tenants, edit and update any details.

Visit `https://portal.agencms.com/#/TENANT-SLUG/hello-agencms`. If you are not running SSL locally, use `http://` in the URL.

Log in with your credentials and you should now have access to the Agencms GUI. Time to start building your site or application.

## Creating our Hello World site

For this tutorial we'll simply customise the default Laravel welcome screen to show a random welcome message, managed via the CMS. All the code in this section is just standard Laravel stuff and not specific to Agencms. The next section will cover actual code unique to Agencms.

### Setup the model

```sh
php artisan make:model Greeting --mcr
```

### Migrations

We'll just add a basic string message field to the table to store our desired greetings. Open the newly created migration from the `database/migrations` folder.

```php
use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateGreetingsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('greetings', function (Blueprint $table) {
            $table->increments('id');
            $table->string('message');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('greetings');
    }
}
```

Run the new migration to setup our table.

```sh
php artisan migrate
```

### Greeting Model

We need to make sure we can fill the new message field by updating the `app/Greeting.php` Model.

```php
namespace App;

use Illuminate\Database\Eloquent\Model;

class Greeting extends Model
{
    protected $fillable = [
        'message',
    ];
}
```

### Controller

Artisan already created a basic resource controller for us. Let's update the typical Create/Read/Update/Delete (CRUD) methods to ensure the CMS can action all of these.

Edit the `app/Http/Controllers/GreetingController.php`.

```php
namespace App\Http\Controllers;

use App\Greeting;
use Illuminate\Http\Request;

class GreetingController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index(Request $request)
    {
        /**
         * Because this method will be used for both the front-end and back-end, we do need to
         * return a a different result for each though. We can do this by checking if the request
         * is Ajax (expecting json) or a view for the front-end
         */

        if ($request->wantsJson()) {
            return Greeting::all();
        }

        // dd(Greeting::inRandomOrder()->first());

        // Return a single, random greeting to the view to show a random greeting on each pageload.
        return view('welcome')->with('greeting', Greeting::inRandomOrder()->first());
    }

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        /**
         * We aren't going to worry about validation for this tutorials.
         * 
         * For now, let's just store the new greeting.
         */

        return Greeting::create($request->all());
    }

    /**
     * Display the specified resource.
     *
     * @param  \App\Greeting  $greeting
     * @return \Illuminate\Http\Response
     */
    public function show(Request $request, Greeting $greeting)
    {
        /**
         * Because Agencms uses the ID to lookup resources the default resource route DI should
         * work just fine and return the correct entry. We can just send it to the API.
         */

        return $greeting;
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \App\Greeting  $greeting
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, Greeting $greeting)
    {
        /**
         * Here we store the greeitng update. We skipped validation for sake of keeping the
         * tutorial short and to the eseentials required to make things work.
         */

        $greeting->update($request->all());

        return response()->json('Greeting Updated', 200);
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  \App\Greeting  $greeting
     * @return \Illuminate\Http\Response
     */
    public function destroy(Greeting $greeting)
    {
        /**
         * Again, super basic implementation here to enable the required functionality.
         */
        $greeting->delete();

        return response()->json('Greeting Deleted', 200);
    }
}
```

### Welcome View

Keeping it nice and simple, we'll just replace the standard `Laravel` heading with a random greeting and a default incase there are no greetings in the system.

```php
{{ array_get($greeting, 'message') ?? 'Welcome' }}
```

We also removed the default Login header as we don't have the required Routes configured.

```php
// Remove this code

@if (Route::has('login'))
    <div class="top-right links">
        @auth
            <a href="{{ url('/home') }}">Home</a>
        @else
            <a href="{{ route('login') }}">Login</a>
            <a href="{{ route('register') }}">Register</a>
        @endauth
    </div>
@endif
```

The full view code should now be something like this.

```php
<!doctype html>
<html lang="{{ app()->getLocale() }}">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">

        <title>Laravel</title>

        <!-- Fonts -->
        <link href="https://fonts.googleapis.com/css?family=Raleway:100,600" rel="stylesheet" type="text/css">

        <!-- Styles -->
        <style>
            html, body {
                background-color: #fff;
                color: #636b6f;
                font-family: 'Raleway', sans-serif;
                font-weight: 100;
                height: 100vh;
                margin: 0;
            }

            .full-height {
                height: 100vh;
            }

            .flex-center {
                align-items: center;
                display: flex;
                justify-content: center;
            }

            .position-ref {
                position: relative;
            }

            .top-right {
                position: absolute;
                right: 10px;
                top: 18px;
            }

            .content {
                text-align: center;
            }

            .title {
                font-size: 84px;
            }

            .links > a {
                color: #636b6f;
                padding: 0 25px;
                font-size: 12px;
                font-weight: 600;
                letter-spacing: .1rem;
                text-decoration: none;
                text-transform: uppercase;
            }

            .m-b-md {
                margin-bottom: 30px;
            }
        </style>
    </head>
    <body>
        <div class="flex-center position-ref full-height">
            <div class="content">
                <div class="title m-b-md">
                    {{ array_get($greeting, 'message') ?? 'Welcome' }}
                </div>

                <div class="links">
                    <a href="https://laravel.com/docs">Documentation</a>
                    <a href="https://laracasts.com">Laracasts</a>
                    <a href="https://laravel-news.com">News</a>
                    <a href="https://forge.laravel.com">Forge</a>
                    <a href="https://github.com/laravel/laravel">GitHub</a>
                </div>
            </div>
        </div>
    </body>
</html>
```

### Routes

We need to configure both API and Web routes. The CORS middleware is automatically installed by Agencms.

```php
// routes/api.php

Route::middleware(['auth:api', 'cors'])->resource('greeting', 'GreetingController');
```

```php
// routes/web.php

Route::get('/', 'GreetingController@index');
```

## Agencms Configuration

All we have left to do is tell Agencms how to manage our data. Let's start by running the Agencms install command to provide us with some boilderplate.

```sh
php artisan agencms:install
```

We can now setup our configuration inside `app\Handlers\AgencmsHandler.php`. First thing is to give the plugin a name to uniquely identofy it.

```php
private static function register()
{
    Router::aliasMiddleware('agencms.plugin.greeting', AgencmsConfig::class);
    Agencms::registerPlugin('agencms.plugin.greeting');
}
```

We can just uncommend the `registerAdmin` method code, no changes needed.

```php
if (!Gate::allows('admin_access')) {
    return;
}

self::registerAgencms();
```

Inside the `registerAgencms` method we finally define our CMS routes and fields. We left the permissions commented out as we have not configured any custom permission yet.

You can specify any [Material Design](https://material.io/icons/) icon key.

```php
// if (!Gate::allows('my_permission')) {
//     return;
// }

Agencms::registerRoute(
    Route::init('greetings', 'Greetings', '/api/greeting')
        ->icon('record_voice_over')
        ->addGroup(
            Group::full('Welcome Messages')->addField(
                Field::string('message', 'Message')->required()->list()
            )
        )
);
```

### Enable the plugin

Inside the `AppServiceProvider.php` `register` method, call the AgencmsHandler.

```php
use App\Handlers\AgencmsHandler;

...

public function register()
{
    AgencmsHandler::register();
}
```

Reload your CMS and add some greetings. They should randomly appear on page load of the Hello Agencms demo site.
