# Bootcamp > Week 2 > Day 2

## Overview of the day

You have a data model for a restaurant ordering app. The problem is all the data is lost when your app stops. The data that makes the app persists only in memory, so when the process dies, so does the data. Today is about fixing that.

<hr/>

## Lesson 1 - SQLite3

## Learning Objectives

* Connect to a database
* Execute CRUD statements on a database

## Before we start

* Make sure you have SQLite3 installed `npm i sqlite3` will do it

## Materials needed

* Sqlite3

## Lesson

!(https://docs.google.com/presentation/d/e/2PACX-1vQpmJ3NMHXf3v-uh4nT3O0keOjivstLweqSi7ZUbhvdFI1M6o4b2cDSFKFdz5YfakbewFyNjIdbrmBI/embed)

The `WHERE` clause is really important when querying. Often we want one record, or a set of records only. Use the `WHERE` clause to filter out the data you need. 

## Assignment

1. Compose the SQL queries to do the following:
    * Create table if not exists
    * Implement the restaurants table
    * Create an entry for a restaurant using "INSERT"
    * Read back your restaurant using "SELECT"
    * Update your restaurant using "UPDATE"
    * Delete your restaurant using "DELETE"
    * Use the WHERE clause to filter results

----

## Lesson 2 - JOIN tables

## Learning Objectives

* Execute SQL commands
* Relate data using foreign keys

## Before we start

* Have you SQL queries for creating a table handy

## Materials needed

* Sqlite3

## Lesson

It is great to be able to save a restaurant in our database. However our data model requires our restaurants to also have menu's associated with them. A restaurant might have many menus; for example a main menu, a set menu, a wine menu (wine list) and best of all the dessert menu! How can we store one or more menu items on the restaurant? We could make a new field on the restaurant table called 'menu'. But what would we do if we wanted to add a 2nd menu 'menu2'? And a third 'menu3'?

No. The way this is done is by creating a separate table for the menus. When we create a row in the 'menus' table we create a special field to store the id of the restaurant we want the menu to be associated with. The convention is to call the field something like 'restaurant_id'. This is why databases that have tables like this are called 'relational databases'.

How do we use the association we just created? When we query our database we are going to use the 'JOIN' keyword in SQL. Read the query below:

```sql
SELECT restaurants.name, menus.name 
FROM restaurants 
JOIN menus ON restaurants.id = menus.restaurant_id 
WHERE restaurants.id = 1;
```
This statement selects the columns we want to retrieve from both tables; the restaurants table and the menus table. Notice now we have mixed fields coming from different tables, we have to name our columns with both the `table_namd.field_name` i.e. `menus.name`. Use the `JOIN` keyword to make an association between the primary key from one table and the foreign key of another. In this example the primary key is the restaurants table id and the menus table restaurant_id. Can you explain why 'restaurant_id' is referred to as the foreign key?

## Assignment

* Write some JOIN statements
* SELECT a restaurant, a menu and all the menu items using a JOIN statement
* [extension activity] explore the documentation for SQL the `SUM`, `COUNT` and `GROUP BY` aggregate functions

[attendance log](https://platform.whitehat.org.uk/apprentice/attendance-log/159)
[main](/swe)|[prev](/swe/bootcamp/wk2/day1.html)|[next](/swe/bootcamp/wk2/day3.html)

