# Voilab Migrate

SQL and PHP migration tool for your web application.
Put your migration files (.sql or .php) in a migration directory and voilab-migrate tool will execute them
and keep your database up-to-date.

## How to Install

#### using [Composer](http://getcomposer.org/)

Create a composer.json file in your project root:

```json
{
    "require": {
        "voilab/migrate": "0.1.*"
    }
}
```

Then run the following composer command:

```bash
$ php composer.phar install
```

#### Prerequisites
- You need composer (at least its autoload system) to run that library.

#### Installation
- create a 'migrations' directory in your project
- copy the 'migrate.php' file from /lib/install to the root path of your project and rename it the way you want
- run the install.sql script in your database (it will create a small table that keep track of your current migration version).

#### Usage
Actually, Voilab Migrate handle 2 types of migrations:
- SQL migration
- PHP migration

###### SQL migration
Simply create your SQL file with the good naming convention.
Name should be something like [custom name]_[version number].sql (i.e. 2014-10-21_14.sql)

When you have migrations to pass, simply run your (maybe renamed) 'migrate.php' script in command-line mode.

###### PHP migration
Create a PHP file, with the same naming convention as the SQL file above.
Your file should look like this:

```php
<?php
class Migration14 {
    public function go(\voilab\migrate\Migrate $migrate) {

        try {
            if ($_CONFIG["Dsi"]["Actif"] && $_CONFIG['Dsi']['Url']) {
                $sql = "UPDATE mytable SET somefield = '" . $some_custom_value . "' WHERE some_other_field='hehehe'";
                $migrate->run($sql); // the mandatory go() method receive the Migrate instance as a parameter. So you can use it here without connecting again to the database.
                echo 'My migration 14 succeeded. Yipee !'; // this text will appear in the console. This is not mandatory...
            }
        } catch (Exception $e) {
            echo "Migration 14: An error occured during the database update.";
            return false;
        }
    }
}
```
So you simply have to define a classe named after the migration number and define a go() method in it.
The go() method get the Migrate instance as its first parameter.

If you need other things from your application, just include them the way you want...


## Authors

[Joel Poulin](http://www.voilab.org)

## License

MIT Public License
