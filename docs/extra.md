## Modular Migrations

If you're familiar with the [Codeigniter Migration Class](https://codeigniter.com/user_guide/libraries/migration.html), it is impossible to maintain separated migration files in your application, you need to merge these files in one directory and update the migration file name. With Craftsman you can manage your database scheme's evolution through independent versions.

For this example we will assume the following directory structure:

    +- APPPATH/
    | +- migrations/
    | | +- 001_add_blog.php
    | | +- 002_add_posts.php

A library could use a database scheme like [Ion Auth](http://benedmunds.com/ion_auth/), [Community Auth](http://community-auth.com/), etc. Here's an example:

    +- APPPATH
    | +- libraries/
    | | +- fooLib/
    | | | +- migrations
    | | | | +- 001_add_session.php
    | | | | +- 002_add_other_stuff.php

In order to run this migrations, include the `--path` argument:

    php craftsman migrate:latest --path="application/libraries/fooLib"

And your migrations are now independent. In your database you can see that every module have an assigned version:

    mysql> SELECT * FROM ci_migrations;

    +---------------+---------+
    | module        | version |
    +---------------+---------+
    | ci_system     |       2 |
    | foolib        |       2 |
    +---------------+---------+

Also you can change the module name in the database with the `--name` argument:

    php craftsman migrate:latest --name="foo" --path="application/libraries/fooLib"

---
