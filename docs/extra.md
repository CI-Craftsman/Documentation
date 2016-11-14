## Modular Migrations

If you're familiar with the [Codeigniter Migration Class](https://codeigniter.com/user_guide/libraries/migration.html), it is impossible to maintain separated migration files in your application, you need to merge these files in one directory and fix the `migration file name`.

With Craftsman you can manage your database scheme's evolution through independent versions of your components.

We will assume the following directory structure:

    +- APPPATH/
    | +- migrations/
    | | +- 001_add_blog.php
    | | +- 002_add_posts.php

And an application library uses a database scheme (like Ion Auth, Community Auth, etc). This migrations reside in:

    +- APPPATH
    | +- libraries/
    | | +- fooLib/
    | | | +- migrations
    | | | | +- 001_add_session.php
    | | | | +- 002_add_other_stuff.php

Run the Migration command with the `--path` argument:

    php vendor/bin/craftsman migrate:latest --path="application/libraries/fooLib"

And your migrations are now independent.

In your database you can see that every component have an assigned version:

    mysql> SELECT * FROM ci_migrations;

    +---------------+---------+
    | module        | version |
    +---------------+---------+
    | ci_system     |       2 |
    | foolib        |       2 |
    +---------------+---------+

Also you can change the component stored name in the database with the `--name` argument:

    php vendor/bin/craftsman migrate:latest --name="foo" --path="application/libraries/fooLib"

---
