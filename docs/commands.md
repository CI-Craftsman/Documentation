## Generators

Craftsman provides a variety of generators to speed up your development process.

<!-- Every Generator Command comes with a `--path` argument to control where the generated file will be placed overwriting the default path.

    php craftsman generate:<command> <args> --path="path/to/new" -->

### Controller

Generate a controller with:

    php craftsman generate:controller <name>

Output:

```php
<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');

class Foo extends CI_Controller
{
    /**
     * Display a listing of the resource.
     * GET /Foo
     */
    public function index()
    {
    }

    /**
     * Display the specified resource.
     * GET /Foo/get/{id}
     *
     * @param  int  $id
     */
    public function get($id)
    {
    }   

    /**
     * Show the form for creating a new resource.
     * GET /Foo/create
     */
    public function create()
    {
    }

    /**
     * Store a newly created resource in storage.
     * POST /Foo/store
     */
    public function store()
    {
    }

    /**
     * Show the form for editing the specified resource.
     * GET /Foo/edit/{id}
     *
     * @param  int  $id
     */
    public function edit($id)
    {
    }   

    /**
     * Update the specified resource in storage.
     * PUT /Foo/update/{id}
     *
     * @param  int  $id
     */
    public function update($id)
    {
    }

    /**
     * Remove the specified resource from storage.
     * DELETE /Foo/delete/{id}
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

Generate a model with:

    php craftsman generate:model <name>

Output:

```php
<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');

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

Generate a migration with:

    php craftsman generate:migration <name>

Regardless of which style you choose to use, the generator command will prefix your files with the migration number. For example:

* 001_add_blog.php (sequential)
* 20121031100537_add_blog.php (timestamp)    

If the file name is prefixed with `create_` or `modify_`, then a migration containing the appropriate `add_column` and `update_column` statements will be created.

**Example**

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

---

## Migrations

Migration schemes are simple files that hold the commands to apply changes to your database. They may create/update tables or fields, but they are not limited to just changing the schema, you could use them to fix bad data in the database or populate new fields.

!!! note "Database Settings"
    If you're going to use Migration Commands you should configure your `application/config/database.php` settings needed to access to your database.

### File names

Each Migration may run in numeric order forward or backwards depending on the method taken. Two numbering styles are available:

* **Sequential**: each migration is enumerated in sequence, starting with 001. Each number must be three digits, and there must not be any gaps in the sequence.
* **Timestamp**: each migration is enumerated with a timestamp, in `YYYYMMDDHHIISS` format (e.g. 20121031100537). This helps to prevent conflicts when working in a team environment.

By default Craftsman uses the sequential style but you can change it using the `--timestamp` argument.

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

Where:

* **Name**: migration name version.
* **Database version**: version actually stored in your database.
* **Local version**: latest migration file version founded in the migration directory.
* **Type**: the migration version type.

Below the information table, there is a legend witch indicates the action to take. If a database update is available, the legend displays the following message:

    ! [NOTE] The Database is not up-to-date with the latest changes, run:'migrate:latest' to update them.

### Running migrations

Migrations are designed to be mostly automatic, but you'll need to know when to make migrations and when to run them. Here's a list of possible options.

Each migration command shows relevant information about the db scheme changes using the `--debug` argument.

**Latest**

Migrate to the latest version, the migration class will use the very newest migration found in the *Filesystem*.

	php craftsman migrate:latest

**Version**

Version can be used to roll back changes or step forwards pro-grammatically to specific versions.

	php craftsman migrate:version <number>

### Rolling-back migrations

Allows you to quickly roll back and forth through the history of the migration schema, so as to work with desired version. Here's a list of possible options.

**Rollback the last migration operation**

	php craftsman migrate:rollback

**Rollback all migrations**

	php craftsman migrate:reset

**Rollback all migrations and run them all again**

	php craftsman migrate:refresh

---

## Seeders

Craftsman comes with a simple method of seeding your database with test data using seed classes. All seed classes are stored by default in your `path/to/application/seeders` folder. Seed classes may have any name you wish, but probably should follow the [CodeIgniter Style Guide](https://codeigniter.com/userguide3/general/styleguide.html).

To generate a seeder:

    php craftsman generate:seeder <name>

A seeder class only contains the `run()` method by default. This method is called when the `db:seed` command is executed. Within the run method, you may insert data into your database however you wish. You may use the [Query Builder Class](https://codeigniter.com/user_guide/query_builder.html) to manually insert data.

Example

Add a database insert statement to the run method:

```php

<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');

use Craftsman\Classes\Seeder;

class Foo extends Seeder implements \Craftsman\Interfaces\Seeder
{
  private $table = 'ci_foo';

  public function run()
  {   
    $this->db->insert($this->table, [
      'title' => 'Title 1',
      'name'  => 'Name 1',
      'date'  => date('Y-m-d H:i:s')
    ]);
  }
}

/* End of file Foo.php */
/* Location: /path/to/application/seeders/Foo.php */
?>
```

One you have written your seeder class, you may use the command:

    php craftsman db:seed <name>

---
