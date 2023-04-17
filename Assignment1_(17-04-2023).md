
# 1. Why do we need Rails Migrations?

Rails Migrations are an essential tool for managing the database schema of a Rails application, and can help make database schema changes more organized, collaborative, and maintainable.

* ## Database schema version control:
With Rails Migrations, we can version control our database schema just like we version control our code. This makes it easier to keep track of changes and rollback to previous versions if needed.

* ## Easy deployment:
Migrations make it easy to deploy changes to the database schema to different environments (e.g. development, staging, production) without having to manually apply SQL scripts.

* ## Data integrity:
We can use migrations to add a new column to a table, and then write a separate migration to populate that column with default values or migrate existing data to the new column.

# 2. What is the difference between a change method and up-down methods in Rails Migration.

In Rails Migration, the change method is a newer way of defining migrations that was introduced in Rails 3.1.<br>

The change method is a single method that defines both the up and down operations of a migration in a reversible way.

For example, here's how we might define a migration using the change method:

```ruby
class AddNameToUsers < ActiveRecord::Migration[6.1]
  def change
    add_column :users, :name, :string
  end
end
```
In this case, the change method adds a new name column to the users table. If we were to run rails db:migrate to apply this migration, Rails would automatically know how to roll back the change by running the down method in reverse, which would remove the name column.

On the other hand, the up and down methods are the older way of defining migrations in Rails. The up method defines the actions to apply the migration, while the down method defines the actions to undo the migration. These methods are not reversible by default, so you must manually define how to undo the migration in the down method.

For example, here's how we might define the same migration using the up and down methods:


```ruby
class AddNameToUsers < ActiveRecord::Migration[6.1]
  def up
    add_column :users, :name, :string
  end

  def down
    remove_column :users, :name
  end
end
```
In this case, the up method adds a new name column to the users table, and the down method removes it. 

So, both the change method and the up/down methods can be used to define migrations in Rails, but the change method is newer and provides a simpler, more concise way to define reversible migrations.

# 3. What are schema files used for?
In Rails, schema files are used to define the current structure of the database schema. The schema file is a snapshot of the current state of the database schema, including all the tables, columns, indexes, and other database objects.

Here are a few ways that schema files are used in Rails:

## Database initialization:
When we create a new Rails application, the schema file is used to create the initial structure of the database schema.

## Database version control:
The schema file is version controlled along with the rest of our application code, so we can keep track of changes to the database schema over time. This makes it easy to roll back to previous versions of the schema if needed.

## Code generation:
Rails uses the schema file to generate models and other database-related code. For example, if we run rails generate scaffold, Rails will use the schema file to generate the appropriate model, migration, and view files.


# 4. What is the usage of change_table and change_column? Explain with an example.
In Rails Migration, change_table and change_column are methods used to modify an existing database table or column respectively.
## change_table
The change_table method allows us to modify an existing database table by adding, removing, or modifying columns.

```ruby
class ModifyUsersTable < ActiveRecord::Migration[6.1]
  def change
    change_table :users do |t|
      t.string :address
      t.change :age, :integer, null: false
    end
  end
end

```
In this example, the change_table method is used to modify the users table by adding a new address column of type string, and modifying the age column to disallow null values.

## change_column
The change_column method allows us to modify an existing column in a database table. We can change the data type, add or remove constraints, and set default values using this method.
```ruby
class ModifyUsersTable < ActiveRecord::Migration[6.1]
  def change
    change_column :users, :name, :string, default: 'Unknown'
  end
end

```

In this example, the change_column method is used to modify the name column in the users table to a string data type and set the default value to 'Unknown'.
