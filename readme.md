# How To Install PostgreSQL on Ubuntu 20.04 [Quickstart]

## Introduction
PostgreSQL, or Postgres, is a relational database management system that provides an implementation of the SQL querying language. It’s standards-compliant and has many advanced features like reliable transactions and concurrency without read locks.

This guide demonstrates how to quickly get Postgres up and running on an Ubuntu 20.04 server, from installing PostgreSQL to setting up a new user and database. If you’d prefer a more in-depth tutorial on installing and managing a PostgreSQL database, see How To Install and Use PostgreSQL on Ubuntu 20.04.

## Prerequisites
To follow along with this tutorial, you will need one Ubuntu 20.04 server that has been configured by following our Initial Server Setup for Ubuntu 20.04 guide. After completing this prerequisite tutorial, your server should have a non-root user with sudo permissions and a basic firewall.

You can also use an interactive terminal that is embedded on this page to experiment with installing and configuring PostgreSQL in this tutorial. Click the following Launch an Interactive Terminal! button to get started.

## Step 1 — Installing PostgreSQL
To install PostgreSQL, first refresh your server’s local package index:

```
sudo apt update

```
Then, install the Postgres package along with a -contrib package that adds some additional utilities and functionality:

```
sudo apt install postgresql postgresql-contrib

```
Ensure that the service is started:

```
sudo systemctl start postgresql.service

```
## Step 2 — Using PostgreSQL Roles and Databases

By default, Postgres uses a concept called “roles” to handle authentication and authorization. These are, in some ways, similar to regular Unix-style users and groups.

Upon installation, Postgres is set up to use ident authentication, meaning that it associates Postgres roles with a matching Unix/Linux system account. If a role exists within Postgres, a Unix/Linux username with the same name is able to sign in as that role.

The installation procedure created a user account called postgres that is associated with the default Postgres role. There are a few ways to utilize this account to access Postgres. One way is to switch over to the postgres account on your server by running the following command:

```
sudo -i -u postgres

```
Set the Default User Password in PostgreSQL

```
\password postgres

```
To List all database:
```
postgres# \l

```
For create databases:
```
postgres# create database myappdb;

```

Create user:
```
CREATE USER user1 WITH ENCRYPTED PASSWORD 'HELLO123';

```
List all USER:
```
\du

```
Give Privilege to an user on a database:

```
GRANT ALL PRIVILEGES ON DATABASE myappdb TO user1;
 
```

Create table 

```
CREATE TABLE playground (
    equip_id serial PRIMARY KEY,
    type varchar (50) NOT NULL,
    color varchar (25) NOT NULL,
    location varchar(25) check (location in ('north', 'south', 'west', 'east', 'northeast', 'southeast', 'southwest', 'northwest')),
    install_date date
);

```
These commands will create a table that inventories playground equipment. This starts with an equipment ID, which is of the serial type. This data type is an auto-incrementing integer. You’ve also given this column the constraint of primary key which means that the values must be unique and not null.

For two of the columns (equip_id and install_date), the commands do not specify a field length. This is because some column types don’t require a set length because the length is implied by the type.

The next two commands create columns for the equipment type and color respectively, each of which cannot be empty (as specified by the NOT NULL constraint applied to each). The line after these creates a location column and adds a constraint requiring this column’s values to be one of eight possible values. The last line within the parentheses creates a date column that records the date on which you installed the equipment.

Note that in SQL, every statement must end in a semicolon (;).

You can find a list of tables within this database by typing:
```
\d

```
If you only want the table returned without the sequence, you can type:
```
\dt

```

## Adding, Querying, and Deleting Data in a Table
Now that you have a table, you can insert some data into it.

As an example, add a slide and a swing by calling the table you want to add to, naming the columns and then providing data for each column, like this:

```
INSERT INTO playground (type, color, location, install_date) VALUES ('slide', 'blue', 'south', '2017-04-28');
INSERT INTO playground (type, color, location, install_date) VALUES ('swing', 'yellow', 'northwest', '2018-08-16');

```
You should take care when entering the data to avoid a few common hangups. For one, do not wrap the column names in quotation marks, but the column values that you enter do need quotes.

Another thing to keep in mind is that you do not enter a value for the equip_id column. This is because this is automatically generated whenever a new row in the table is created.

Retrieve the information you’ve added by typing:

```
SELECT * FROM playground;

```
This output indicates that your equip_id has been filled in successfully and that all of your other data has been organized correctly.

If the slide on the playground breaks and you have to remove it, you can also remove the row from your table by typing:

```
DELETE FROM playground WHERE type = 'slide';

```

Query the table again:

```
SELECT * FROM playground;

```
Notice that your slide is no longer a part of the table.

## Adding and Deleting Columns from a Table
After creating a table, you can modify it to add or remove columns relatively easily. Add a column to show the last maintenance visit for each piece of equipment by typing:

```
ALTER TABLE playground ADD last_maint date;

```
The next time you view your table information again, the new column will have been added (but no data will have been entered):

```
SELECT * FROM playground;

```
To delete a column, you can enter an SQL statement very similar to the one you used to add the last_maint column. If you find that your work crew uses a separate tool to keep track of maintenance history, you can delete of the column by typing:

```
ALTER TABLE playground DROP last_maint;

```

## Updating Data in a Table

So far, you’ve learned how to add records to a table and how to delete them, but this tutorial hasn’t yet covered how to modify existing entries.

You can update the values of an existing entry by querying for the record you want and setting the column to the value you wish to use. You can query for the “swing” record (this will match every swing in your table) and change its color to “red”. This could be useful if you gave the swing set a paint job:
```
UPDATE playground SET color = 'red' WHERE type = 'swing';

```




