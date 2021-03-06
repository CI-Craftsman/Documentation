## Console

Craftsman comes with a built in [REPL command](http://psysh.org)  that makes it easy to explore your application. You can start it with:

    php craftsman console

This will bootstrap your application and start an interactive console. At this point you can interact with your application code and execute queries using your application’s models. To quit you can use `CTRL-C` or by typing `exit`.

---

## Serve

Typically, you may use a web server such as Apache or Nginx to serve your Codeigniter applications. If you are on PHP 5.4+ and would like to use PHP's built-in development server, you may use the serve command:

    php craftsman serve

By default the HTTP-server will listen to port 8000. However if that port is already in use or you wish to serve multiple applications this way, you might want to specify what port to use. Just add the --port argument:

    php craftsman serve --port=8080

---

## Migrations

Migration schemes are simple files that hold the commands to apply changes to your database. They may create/update tables or fields, but they are not limited to just changing the schema, you could use them to fix bad data in the database or populate new fields.

!!! warning "Database Connection Required"
    If you're going to use Migration Commands you should configure your "application/config/database.php" settings to access to your database.

### File names

Each Migration may run in numeric order forward or backwards depending on the method taken. Two numbering styles are available:

* **Sequential**: each migration is enumerated in sequence, starting with 001. Each number must be three digits, and there must not be any gaps in the sequence.
* **Timestamp**: each migration is enumerated with a timestamp, in `YYYYMMDDHHIISS` format (e.g. 20121031100537). This helps to prevent conflicts when working in a team environment.

By default Craftsman uses the 'Timestamp' style but you can change to 'Sequential' using the `--sequential` argument.

### Displaying info

You can display the current migration status with the command:

	php craftsman migrate:check

Output:

```
(in ~/workspace/codeigniter/application/)

----------- ----------- ---------------- ------------------
 Name        Type        Local version    Database version  
----------- ----------- ---------------- ------------------
 ci_system   timestamp   20161106010705   20161106010705    
----------- ----------- ---------------- ------------------

Migration directory: migrations/

[OK] Database is up-to-date.    
```

Below the information table, there is a legend witch indicates the action to take. If a database update is available, the legend displays the following message:

    ! [NOTE] The Database is not up-to-date with the latest changes, run:'migrate:latest' to update them.

### Runners

Each migration command shows relevant information about the db scheme changes by default. Here's a list of possible options.

#### Latest

Allows you to migrate the latest version, the migration class will use the very newest migration found in the *Filesystem*.

	php craftsman migrate:latest

#### Version

Allows you to roll back changes or step forwards pro-grammatically to specific versions.

	php craftsman migrate:version <number>

### Rolling-back

Allows you to quickly roll back and forth through the history of the migration schema, so as to work with desired version. Here's a list of possible options.

#### Previous

	php craftsman migrate:rollback

#### Reset

Rollback all migrations.

	php craftsman migrate:reset

#### Refresh

Rollback all migrations and run them all again.

	php craftsman migrate:refresh

---

## Seeders

Craftsman comes with a simple method of seeding your database. Seeders may have any name you wish, but probably should follow the [CodeIgniter Style Guide](https://codeigniter.com/userguide3/general/styleguide.html).

A seeder class only contains the `run()` method by default, this method is called when the `db:seed` command is executed. Within the run method, you may insert data into your database however you wish. Here's an example:

```php

<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');

use Craftsman\Database\Seeder;

class Foo extends Seeder
{
  private $table = '<table_name>';

  public function run()
  {   
    $this->db->insert_batch($this->table, [
      // insert your data here...
    ]);
  }
}

/* End of file Foo.php */
/* Location: /path/to/application/seeders/Foo.php */
?>
```

One you have written your seeder class, you may use the command:

    php craftsman db:seed <name>

Check the [Query Builder Class](https://codeigniter.com/user_guide/query_builder.html) to manually insert data.

## Generators

Craftsman provides a variety of generators to speed up your development process.

### Controller

Generate a controller with the command:

    php craftsman generate:controller <name>

Output:

```php
<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');

class Foo extends CI_Controller
{
    /**
     * Display a listing of the resource.
     * GET /foo
     */
    public function index()
    {
    }

    /**
     * Display the specified resource.
     * GET /foo/read/{id}
     *
     * @param  int  $id
     */
    public function read($id)
    {
    }   

    /**
     * Show the form for creating a new resource.
     * GET /foo/create
     */
    public function create()
    {
    }

    /**
     * Store a newly created resource in storage.
     * POST /foo/store
     */
    public function store()
    {
    }

    /**
     * Show the form for editing the specified resource.
     * GET /foo/edit/{id}
     *
     * @param  int  $id
     */
    public function edit($id)
    {
    }   

    /**
     * Update the specified resource in storage.
     * PUT /foo/update/{id}
     *
     * @param  int  $id
     */
    public function update($id)
    {
    }

    /**
     * Remove the specified resource from storage.
     * DELETE /foo/delete/{id}
     *
     * @param  int  $id
     */
    public function delete($id)
    {
    }
}

/* End of file Foo.php */
/* Location: /path/to/application/controllers/Foo.php */  
?>
```

### Model

Generate a model with the command:

    php craftsman generate:model <name>

Output:

```php
<?php if (! defined('BASEPATH')) exit('No direct script access allowed');

class Foo_model extends CI_Model
{
    public function __construct()
    {
        parent::__construct();
        $this->load->database();    
    }
}

/* End of file Foo_model.php */
/* Location: /path/to/application/models/Foo_model.php */
?>
```

### Migration

Generate a migration with the command:

    php craftsman generate:migration <name>

Regardless of which style you choose to use, the generator command will prefix your files with the migration number. For example:

* 001_add_blog.php (sequential)
* 20121031100537_add_blog.php (timestamp)    

**Name prefixes**

Use the prefix `create_` or `modify_` if you want to create a migration with the appropriate `add_column` and `update_column` statements. Here's an example:

    php craftsman generate:migration create_users firstname:varchar lastname:varchar email:varchar active:smallint

Output:

```php
<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');

class Migration_create_users extends CI_Migration {

    public function __construct()
    {
        $this->load->dbforge();
        $this->load->database();
    }

    public function up()
    {
        $this->create_users_table();
    }

    public function down()
    {
        $this->dbforge->drop_table('ci_users');
    }

    private function create_users_table()
    {
        $this->dbforge->add_field(array(
            'id' => array(
                'type' => 'INT',
                'null' => FALSE,
                'auto_increment' => TRUE
            ),
            'firstname' => array(
                'type' => 'VARCHAR',
                'constraint' => 100,
                'null' => FALSE
            ),
            'lastname' => array(
                'type' => 'VARCHAR',
                'constraint' => 100,
                'null' => TRUE
            ),
            'email' => array(
                'type' => 'VARCHAR',
                'constraint' => 100,
                'null' => TRUE
            ),
            'active' => array(
                'type' => 'SMALLINT',
                'null' => FALSE,
                'default' => 1
            ),
        ));
        $this->dbforge->add_key('id', TRUE);
        $this->dbforge->create_table('ci_users',TRUE);      
    }
}

/* End of file 001_create_users.php.php */
/* Location: path/to/application/migrations/001_create_users.php */
?>
```

Now it's your turn to give the finishing touches before running this scheme. Check the [Database Forge documentation](https://codeigniter.com/user_guide/database/forge.html) for more information about Codeigniter Migrations.

### Seeders

Generate a seeder with the command:

    php craftsman generate:seeder <name>

Output:

```php

<?php

use Craftsman\Database\Seeder;

class Foo extends Seeder implements \Craftsman\Interfaces\Seeder
{
  private $table = 'ci_foo';

  public function run()
  {   
    $this->db->insert_batch($this->table, [
      // insert your data here...
    ]);
  }
}

/* End of file Foo.php */
/* Location: /path/to/application/seeders/Foo.php */
?>
```

All seed classes are stored by default in your `path/to/application/seeders` folder.

---
