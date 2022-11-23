[Get Started With Laravel 8](https://code.tutsplus.com/courses/get-started-with-laravel-5)

## XAMPP

`sudo /opt/lampp/lampp start`

![](https://localhost)

http://localhost/phpmyadmin/

Visual XAMPP application launcher: `sudo /opt/lampp/manager-linux-x64.run`

## Tutorial: Debian, XAMPP, and Composer

[XAMPP - Composer - Laravel Installation on GNU/Linux | XAMPP-Composer-Laravel](https://mridvanozcan.github.io/XAMPP-Composer-Laravel/)

## Laravel + Docker

Check the Laravel documentation for that.

## PHP

`/opt/lampp/bin/php -v`

## Composer (to get Laravel)

Download Composer

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

### Run Composer and install Laravel

`/opt/lampp/bin/php composer.phar global require laravel/installer` (from course, but `laravel` command did not work)

`/opt/lampp/bin/php composer.phar create-project laravel/laravel --prefer-dist` !?

[Build a CMS With Laravel](https://code.tutsplus.com/courses/build-a-cms-with-laravel)

# Laravel uses Vagrant / HomeStead (!?)

[Laravel Homestead - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/9.x/homestead)

## If composer is running: create a new Laravel project

`composer create-project --prefer-dist laravel/laravel OpenKarmaCMS`

if `laravel` command is working, just use:

`laravel new OpenKarmaCMS`

# In the `.env` file are all settings

`vi .env`

There make sure these settings are done:

```
dion@e5270:~/git/public_git/php_cms_with_laravel/OpenKarmaCMS$ cat .env | grep DB
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=openkarmacms
DB_USERNAME=root
DB_PASSWORD=root
```

# Database migrations (versions of database settings)

They will be under `database/migrations`

Whenever you create a new project there is a migration for:

* `create_users_table.php`
* `create_password_resets_table.php`

A Migration is nothing more than a Class.

The Migration `extends` the `Migration` class: `return new class extends Migration`

A Migration has two methods. One is called `up` and the other is `down`, see `public function up()`

It is using the `Schema`-builder. It is creating a `users`-table.

```
return new class extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('email')->unique();
            $table->timestamp('email_verified_at')->nullable();
            $table->string('password');
            $table->rememberToken();
            $table->timestamps();
        });
    }
```

The `down` method will just `drop` the `users`-table if it exists:

```
    public function down()
    {
        Schema::dropIfExists('users');
    }
```

# You can manage your Database with

MySQL Workbench (Community)

Otherwise use the phpMyAdmin in the browser (http://localhost/phpmyadmin/)

# To Migrate the database

`php artisan migrate` (make sure the user/password in the `.env` and the MariaDB settings are the same).

In the `migrations` Table you can see a `batch`, this is how many time that migration has been run.

# To create a new migration

`php artisan make:migration create_pages_table`

Above will create a new migration file into `./database/migrations/` and the name of that class will be `CreatePagesTable`

This time it was clever enough to generate that the Table name will be `pages`, but you can overwite this:

```php
  {
        Schema::create('pages', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }`
```

But if you want to give it straight away a different name (lets stay `content` for the Table do:

`php artisan make:migration create_pages_table --create=content`

# I think there is another way: After you created your Migration, you can make a Model. The world will be singualar (so not `pages` but `page`).

`php artisan make:model Page --migration # or '-m' instead of '--migration'

# Run Migration on the just ceated table

`php artisan make:migration create_pages_table --create=content`

# This is a Migrate rollback.

`sudo php artisan migrate:rollback`

# Now there are two Models created

In the folder `./app/Models/`

There are:

`Page.php`
`User.php`

# To add a User to many pages relation we add this to the `User.php` page:

```php
    # DD: This has a User to Pages relation:
    public function pages() {
        return $this->hadMany('App\Page');
    }
```

# To add a relation for many Pages ot one user we add this to the `Page.php` page:

```php
{
    use HasFactory;
    public function user() {
        return $this->belongsTo('App\User');
    }
}
```

Notice above that the `App\User` and `App\Page` are relating to the `app\User.php` and `app\Page.php` file.

## Next

[Build a CMS With Laravel - Setting Up Security](https://code.tutsplus.com/courses/build-a-cms-with-laravel/lessons/setting-up-security)
 



