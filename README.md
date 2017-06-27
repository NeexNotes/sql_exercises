# SQL basics

Notes about SQL based of Khan Academy SQL courses: [SQL courses](https://www.khanacademy.org/computing/computer-programming/sql) and w3 school [tutorial/cheatsheet](https://www.w3schools.com/sql/default.asp).

[TOC]

## TL;DR

- **SELECT** - extracts data from a database
- **SELECT DISTINCT** - used to return only distinct (different) values
- **UPDATE** - updates data in a database
- **DELETE** - deletes data from a database
- **INSERT INTO** - inserts new data into a database
- **CREATE DATABASE** - creates a new database
- **ALTER DATABASE** - modifies a database
- **CREATE TABLE** - creates a new table
- **ALTER TABLE** - modifies a table
- **DROP TABLE** - deletes a table
- **CREATE INDEX** - creates an index (search key)
- **DROP INDEX** - deletes an index

------

* **WHERE** - adds condition
* **AND, OR, NOT** - more conditions
* **ORDER BY** - column1 or column2, ... **ASC|DESC**; ASC is default
* **NULL** - A field with a NULL value is a field with no value.
* **UPDATE**
  ```sql
  UPDATE table_name
  SET column1 *= value1, column2 = value2*, ...
  WHERE condition;
  ```
* **DELETE**
  ```sql
  DELETE FROM table_name
  WHERE condition;
  ```
* **INSERT INTO**
  ```sql
  INSERT INTO table_name (column1, column2, column3, ...)
  VALUES (value1, value2, value3, ...);

  /*when order is kept*/
  INSERT INTO table_name
  VALUES (value1, value2, value3, ...);
  ```
* **SELECT TOP**
  ```sql
  SELECT column_name(s)
  FROM table_name
  WHERE condition
  LIMIT number;
  ```
   TODO: 
  SQL Min and Max
  SQL Count, Avg, Sum
  SQL Like
  SQL Wildcards
  SQL In
  SQL Between
  SQL Aliases
  SQL Joins
  SQL Inner Join
  SQL Left Join
  SQL Right Join
  SQL Full Join
  SQL Self Join
  SQL Union
  SQL Group By
  SQL Having
  SQL Exists
  SQL Any, All
  SQL Select Into
  SQL Insert Into Select
  SQL Comments

------

```sql
CREATE TABLE groceries (id INTEGER PRIMARY KEY, name TEXT, quantity INTEGER, aisle INTEGER);

INSERT INTO groceries VALUES (1, "bananas", 4, 1);
INSERT INTO groceries VALUES (2, "Peanut Butter", 1, 2);
INSERT INTO groceries VALUES (3, "Chocolate", 2, 7);

SELECT * FROM groceries;

SELECT * FROM groceries ORDER BY aisle;
SELECT * FROM groceries WHERE aisle > 5 ORDER BY aisle;

SELECT SUM(quantity) FROM groceries;
SELECT MAX(quantity) FROM groceries;
SELECT SUM(quantity) FROM groceries GROUP BY aisle;

SELECT aisle, SUM(quantity) FROM groceries GROUP BY aisle;
```



```sql
CREATE TABLE bikes (id INTEGER PRIMARY KEY, brand TEXT, model TEXT, year INTEGER, price INTEGER, inventory INTEGER);

INSERT INTO bikes VALUES (1, "Koho", "Green Apple", 2011, 1729, 7);
INSERT INTO bikes VALUES (2, "Bogu", "Blip", 2012, 600, 77);
INSERT INTO bikes VALUES (3, "Beets", "Prep", 2017, 599, 34 );
INSERT INTO bikes VALUES (4, "Greeereee", "goo", 2011, 799, 89);
INSERT INTO bikes VALUES (5, "Speedy Muse", "gibbon", 2012, 229, 111);
INSERT INTO bikes VALUES (6, "Pupupapp", "Sportsy", 2009, 1200, 33);

SELECT * FROM bikes ORDER BY price;
```



```sql
CREATE TABLE exercise_logs
    (id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT,
    minutes INTEGER, 
    calories INTEGER,
    heart_rate INTEGER);


INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 100, 110);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 30, 105);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);

SELECT * FROM exercise_logs;
SELECT * FROM exercise_logs WHERE calories > 50 ORDERED BY calories;
SELECT * FROM exercise_logs WHERE calories > 50 AND minutes < 30;
SELECT * FROM exercise_logs WHERE calories > 50 OR heart_rate > 100;
```

```sql
CREATE TABLE songs (
    id INTEGER PRIMARY KEY,
    title TEXT,
    artist TEXT,
    mood TEXT,
    duration INTEGER,
    released INTEGER);
    
INSERT INTO songs (title, artist, mood, duration, released)
    VALUES ("Bohemian Rhapsody", "Queen", "epic", 60, 1975);
INSERT INTO songs (title, artist, mood, duration, released)
    VALUES ("Let it go", "Idina Menzel", "epic", 227, 2013);
INSERT INTO songs (title, artist, mood, duration, released)
    VALUES ("I will survive", "Gloria Gaynor", "epic", 198, 1978);
INSERT INTO songs (title, artist, mood, duration, released)
    VALUES ("Twist and Shout", "The Beatles", "happy", 152, 1963);
INSERT INTO songs (title, artist, mood, duration, released)
    VALUES ("La Bamba", "Ritchie Valens", "happy", 166, 1958);
INSERT INTO songs (title, artist, mood, duration, released)
    VALUES ("I will always love you", "Whitney Houston", "epic", 273, 1992);
INSERT INTO songs (title, artist, mood, duration, released)
    VALUES ("Sweet Caroline", "Neil Diamond", "happy", 201, 1969);
INSERT INTO songs (title, artist, mood, duration, released)
    VALUES ("Call me maybe", "Carly Rae Jepsen", "happy", 193, 2011);

SELECT title FROM songs;
SELECT title FROM songs WHERE mood IS "epic" OR released > 1990;
SELECT title FROM songs WHERE mood IS "epic" AND released > 1990 AND duration < 240;
```

```sql
CREATE TABLE exercise_logs
    (id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT,
    minutes INTEGER, 
    calories INTEGER,
    heart_rate INTEGER);

INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 100, 110);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 30, 105);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 25, 72, 80);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("rowing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("hiking", 60, 80, 85);

SELECT * FROM exercise_logs WHERE type = "biking" OR type = "hiking" OR type = "tree climbing" OR type = "rowing";

/* IN */
SELECT * FROM exercise_logs WHERE type IN ("biking", "hiking", "tree climbing", "rowing");

/* everything but outdoor activities */
SELECT * FROM exercise_logs WHERE type NOT IN ("biking", "hiking", "tree climbing", "rowing");
```

### subqueries

Search for data based on a different table:

```sql
CREATE TABLE drs_favorites
    (id INTEGER PRIMARY KEY,
    type TEXT,
    reason TEXT);

INSERT INTO drs_favorites(type, reason) VALUES ("biking", "Improves endurance and flexibility.");
INSERT INTO drs_favorites(type, reason) VALUES ("hiking", "Increases cardiovascular health.");

SELECT type FROM drs_favorites;

SELECT * FROM exercise_logs WHERE type IN (
    SELECT type FROM drs_favorites);
    
SELECT * FROM exercise_logs WHERE type IN (
    SELECT type FROM drs_favorites WHERE reason = "Increases cardiovascular health");
    
/* LIKE */
SELECT * FROM exercise_logs WHERE type IN (
    SELECT type FROM drs_favorites WHERE reason = "Increases cardiovascular health."); 
/* this looks for EXACT MATCH so use LIKE */

SELECT * FROM exercise_logs WHERE type IN (
    SELECT type FROM drs_favorites WHERE reason LIKE "%cardiovascular%");

```

```sql
CREATE TABLE artists (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT,
    country TEXT,
    genre TEXT);

INSERT INTO artists (name, country, genre)
    VALUES ("Taylor Swift", "US", "Pop");
INSERT INTO artists (name, country, genre)
    VALUES ("Led Zeppelin", "US", "Hard rock");
INSERT INTO artists (name, country, genre)
    VALUES ("ABBA", "Sweden", "Disco");
INSERT INTO artists (name, country, genre)
    VALUES ("Queen", "UK", "Rock");
INSERT INTO artists (name, country, genre)
    VALUES ("Celine Dion", "Canada", "Pop");
INSERT INTO artists (name, country, genre)
    VALUES ("Meatloaf", "US", "Hard rock");
INSERT INTO artists (name, country, genre)
    VALUES ("Garth Brooks", "US", "Country");
INSERT INTO artists (name, country, genre)
    VALUES ("Shania Twain", "Canada", "Country");
INSERT INTO artists (name, country, genre)
    VALUES ("Rihanna", "US", "Pop");
INSERT INTO artists (name, country, genre)
    VALUES ("Guns N' Roses", "US", "Hard rock");
INSERT INTO artists (name, country, genre)
    VALUES ("Gloria Estefan", "US", "Pop");
INSERT INTO artists (name, country, genre)
    VALUES ("Bob Marley", "Jamaica", "Reggae");

CREATE TABLE songs (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    artist TEXT,
    title TEXT);

INSERT INTO songs (artist, title)
    VALUES ("Taylor Swift", "Shake it off");
INSERT INTO songs (artist, title)
    VALUES ("Rihanna", "Stay");
INSERT INTO songs (artist, title)
    VALUES ("Celine Dion", "My heart will go on");
INSERT INTO songs (artist, title)
    VALUES ("Celine Dion", "A new day has come");
INSERT INTO songs (artist, title)
    VALUES ("Shania Twain", "Party for two");
INSERT INTO songs (artist, title)
    VALUES ("Gloria Estefan", "Conga");
INSERT INTO songs (artist, title)
    VALUES ("Led Zeppelin", "Stairway to heaven");
INSERT INTO songs (artist, title)
    VALUES ("ABBA", "Mamma mia");
INSERT INTO songs (artist, title)
    VALUES ("Queen", "Bicycle Race");
INSERT INTO songs (artist, title)
    VALUES ("Queen", "Bohemian Rhapsody");
INSERT INTO songs (artist, title)
    VALUES ("Guns N' Roses", "Don't cry");
    
SELECT title FROM songs WHERE artist = "Queen";
SELECT name FROM artists WHERE genre = "Pop";
SELECT title FROM songs WHERE artist IN (SELECT name FROM artists WHERE genre = "Pop");
```

### WHERE vs HAVING

WHERE is used to single row, while HAVING filters GROUPED VALUES

```sql
CREATE TABLE exercise_logs
    (id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT,
    minutes INTEGER, 
    calories INTEGER,
    heart_rate INTEGER);

INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 115, 110);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 45, 105);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 165, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 25, 72, 80);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("rowing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("hiking", 60, 80, 85);

SELECT * FROM exercise_logs;

SELECT type, SUM(calories) AS total_calories FROM exercise_logs GROUP BY type;

SELECT type, SUM(calories) AS total_calories FROM exercise_logs
    GROUP BY type
    HAVING total_calories > 150
    ;

SELECT type, AVG(calories) AS avg_calories FROM exercise_logs
    GROUP BY type
    HAVING avg_calories > 70
    ; 
    
SELECT type FROM exercise_logs GROUP BY type HAVING COUNT(*) >= 
```

```sql
CREATE TABLE books (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    author TEXT,
    title TEXT,
    words INTEGER);
    
INSERT INTO books (author, title, words)
    VALUES ("J.K. Rowling", "Harry Potter and the Philosopher's Stone", 79944);
INSERT INTO books (author, title, words)
    VALUES ("J.K. Rowling", "Harry Potter and the Chamber of Secrets", 85141);
INSERT INTO books (author, title, words)
    VALUES ("J.K. Rowling", "Harry Potter and the Prisoner of Azkaban", 107253);
INSERT INTO books (author, title, words)
    VALUES ("J.K. Rowling", "Harry Potter and the Goblet of Fire", 190637);
INSERT INTO books (author, title, words)
    VALUES ("J.K. Rowling", "Harry Potter and the Order of the Phoenix", 257045);
INSERT INTO books (author, title, words)
    VALUES ("J.K. Rowling", "Harry Potter and the Half-Blood Prince", 168923);
INSERT INTO books (author, title, words)
    VALUES ("J.K. Rowling", "Harry Potter and the Deathly Hallows", 197651);

INSERT INTO books (author, title, words)
    VALUES ("Stephenie Meyer", "Twilight", 118501);
INSERT INTO books (author, title, words)
    VALUES ("Stephenie Meyer", "New Moon", 132807);
INSERT INTO books (author, title, words)
    VALUES ("Stephenie Meyer", "Eclipse", 147930);
INSERT INTO books (author, title, words)
    VALUES ("Stephenie Meyer", "Breaking Dawn", 192196);
    
INSERT INTO books (author, title, words)
    VALUES ("J.R.R. Tolkien", "The Hobbit", 95022);
INSERT INTO books (author, title, words)
    VALUES ("J.R.R. Tolkien", "Fellowship of the Ring", 177227);
INSERT INTO books (author, title, words)
    VALUES ("J.R.R. Tolkien", "Two Towers", 143436);
INSERT INTO books (author, title, words)
    VALUES ("J.R.R. Tolkien", "Return of the King", 134462);
    
SELECT author, SUM(words) AS total_words FROM books GROUP BY author HAVING total_words > 1000000;

SELECT author, AVG(words) AS avg_words FROM books GROUP BY author HAVING avg_words > 150000;
```

 ### CASE

```sql
CREATE TABLE exercise_logs
    (id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT,
    minutes INTEGER, 
    calories INTEGER,
    heart_rate INTEGER);

INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 100, 110);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 30, 105);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 165, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 25, 72, 80);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("rowing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("hiking", 60, 80, 85);

SELECT * FROM exercise_logs;

SELECT COUNT(*) FROM exercise_logs WHERE heart_rate > 220 - 30;

/* 50-90% of max*/
SELECT COUNT(*) FROM exercise_logs WHERE
    heart_rate >= ROUND(0.50 * (220-30)) 
    AND  heart_rate <= ROUND(0.90 * (220-30));
    
/* CASE */
SELECT type, heart_rate,
    CASE 
        WHEN heart_rate > 220-30 THEN "above max"
        WHEN heart_rate > ROUND(0.90 * (220-30)) THEN "above target"
        WHEN heart_rate > ROUND(0.50 * (220-30)) THEN "within target"
        ELSE "below target"
    END as "hr_zone"
FROM exercise_logs;

SELECT COUNT(*),
    CASE 
        WHEN heart_rate > 220-30 THEN "above max"
        WHEN heart_rate > ROUND(0.90 * (220-30)) THEN "above target"
        WHEN heart_rate > ROUND(0.50 * (220-30)) THEN "within target"
        ELSE "below target"
    END as "hr_zone"
FROM exercise_logs
GROUP BY hr_zone;

```

## JOIN

#### Database

```sql
CREATE TABLE students (id INTEGER PRIMARY KEY,
    first_name TEXT,
    last_name TEXT,
    email TEXT,
    phone TEXT,
    birthdate TEXT);

INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ("Peter", "Rabbit", "peter@rabbit.com", "555-6666", "2002-06-24");
INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ("Alice", "Wonderland", "alice@wonderland.com", "555-4444", "2002-07-04");
    
CREATE TABLE student_grades (id INTEGER PRIMARY KEY,
    student_id INTEGER,
    test TEXT,
    grade INTEGER);

INSERT INTO student_grades (student_id, test, grade)
    VALUES (1, "Nutrition", 95);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (2, "Nutrition", 92);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (1, "Chemistry", 85);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (2, "Chemistry", 95);
```

#### Queries

```sql
SELECT * FROM student_grades;

/* cross join */
SELEct * FROM student_grades, students;

/* implicit inner join */
SELECT * FROM student_grades, students WHERE student_grades.student_id = students.id;

/* explicit inner join */
SELECT * FROM students 
	JOIN student_grades
	ON students.id = student_grades.student_id;

SELECT first_name, last_name, email, test, grade FROM students 
	JOIN student_grades
	ON students.id = student_grades.student_id;
```

**ON** cleans things up from duplicates.

#### everything works as before:

```sql
/* explicit inner join - JOIN */
SELECT first_name, last_name, email, test, grade FROM students
    JOIN student_grades
    ON students.id = student_grades.student_id
    WHERE grade > 90;
```

### same name - different meaning

if you had grade for course and grade for student (average) it could be problematic, so to select, prefix columns:

```sql
SELECT students.first_name, students.last_name, students.email, student_grades.test, student_grades.grade FROM students
    JOIN student_grades
    ON students.id = student_grades.student_id
    WHERE grade > 90;
```

#### Joining related tables with left outer joins

continue with previous data

```sql
CREATE TABLE student_projects (id INTEGER PRIMARY KEY,
    student_id INTEGER,
    title TEXT);
    
INSERT INTO student_projects (student_id, title)
    VALUES (1, "Carrotapault");
    
SELECT students.first_name, students.last_name, student_projects.title
    FROM students
    JOIN student_projects
    ON students.id = student_projects.student_id;
```

This only gives one student, who has the project. 

But if you want all students regardless of projects, you must do LEFT OUTER

```sql
/* outer join */ 
SELECT students.first_name, students.last_name, student_projects.title
    FROM students
    LEFT OUTER JOIN student_projects
    ON students.id = student_projects.student_id;
```

* LEFT - table is `students`


* OUTER says you want all.

* RIGHT - keep all in the right table

* FULL - keep all data and fills NULL in all data not matched

### ID in the same table

```sql
CREATE TABLE students (id INTEGER PRIMARY KEY AUTOINCREMENT,
    first_name TEXT,
    last_name TEXT,
    email TEXT,
    phone TEXT,
    birthdate TEXT,
    buddy_id INTEGER);

INSERT INTO students 
    VALUES (1, "Peter", "Rabbit", "peter@rabbit.com", "555-6666", "2002-06-24", 2);
INSERT INTO students 
    VALUES (2, "Alice", "Wonderland", "alice@wonderland.com", "555-4444", "2002-07-04", 1);
INSERT INTO students 
    VALUES (3, "Aladdin", "Lampland", "aladdin@lampland.com", "555-3333", "2001-05-10", 4);
INSERT INTO students 
    VALUES (4, "Simba", "Kingston", "simba@kingston.com", "555-1111", "2001-12-24", 3);
    
SELECT id, first_name, last_name, buddy_id FROM students;
```

how to show e-mails not just the ID => **JOIN TO SELF**

```sql
/* self join */
SELECT students.first_name, students.last_name, buddies.email as buddy_email
    FROM students
    JOIN students buddies
    ON students.buddy_id = buddies.id;
```

You have to nickname one occurrence.

#### SELF JOIN + JOIN at the same time

```sql
CREATE TABLE students (id INTEGER PRIMARY KEY,
    first_name TEXT,
    last_name TEXT,
    email TEXT,
    phone TEXT,
    birthdate TEXT);

INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ("Peter", "Rabbit", "peter@rabbit.com", "555-6666", "2002-06-24");
INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ("Alice", "Wonderland", "alice@wonderland.com", "555-4444", "2002-07-04");
INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ("Aladdin", "Lampland", "aladdin@lampland.com", "555-3333", "2001-05-10");
INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ("Simba", "Kingston", "simba@kingston.com", "555-1111", "2001-12-24");
    
CREATE TABLE student_projects (id INTEGER PRIMARY KEY,
    student_id INTEGER,
    title TEXT);
    
INSERT INTO student_projects (student_id, title)
    VALUES (1, "Carrotapault");
INSERT INTO student_projects (student_id, title)
    VALUES (2, "Mad Hattery");
INSERT INTO student_projects (student_id, title)
    VALUES (3, "Carpet Physics");
INSERT INTO student_projects (student_id, title)
    VALUES (4, "Hyena Habitats");
    
CREATE TABLE project_pairs (id INTEGER PRIMARY KEY,
    project1_id INTEGER,
    project2_id INTEGER);

INSERT INTO project_pairs (project1_id, project2_id)
    VALUES(1, 2);
INSERT INTO project_pairs (project1_id, project2_id)
    VALUES(3, 4);
    
SELECT a.title, b.title 
	FROM project_pairs
    JOIN student_projects a
    ON project_pairs.project1_id = a.id
    JOIN student_projects b
    ON project_pairs.project2_id = b.id;
```

### The lifecycle of a SQL query

We can think of the SQL engine going through these steps for each query we give it:

![](sql_flow.png)

1. The **query parser** makes sure that the query is syntactically correct (e.g. commas out of place) and semantically correct (i.e. the tables exist), and returns errors if not. If it's correct, then it turns it into an algebraic expression and passes it to the next step.
2. The **query planner and optimizer** does the hard thinking work. It first performs straightforward optimizations (improvements that always result in better performance, like simplifying 5*10 into 50). It then considers different "query plans" which may have different optimizations, estimates the cost (CPU and time) of each query plan based on the number of rows in the relevant tables, then it picks the optimal plan and passes it on to the next step.
3. The **query executor** takes the plan and turns it into operations for the database, returning the results back to us if there are any.

# DATABASES

**INSERT** is relatively safe, because all it does is add data, but operations like **UPDATE**, **DELETE**, **DROP**, or **ALTER** can be much more dangerous, because they are updating existing data. That's why it's important to really understand those well, and use them carefully. Keep going to learn how to use them!

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name TEXT);
    
CREATE TABLE diary_logs (
    id INTEGER PRIMARY KEY,
    user_id INTEGER,
    date TEXT,
    content TEXT
    );
    
/* After user submitted their new diary log */
INSERT INTO diary_logs (user_id, date, content) VALUES (1, "2015-04-01",
    "I had a horrible fight with OhNoesGuy and I buried my woes in 3 pounds of dark chocolate.");
    
INSERT INTO diary_logs (user_id, date, content) VALUES (1, "2015-04-02",
    "We made up and now we're best friends forever and we celebrated with a tub of ice cream.");

SELECT * FROM diary_logs;

UPDATE diary_logs SET content = "I had a horrible fight with OhNoesGuy" WHERE id = 1;

SELECT * FROM diary_logs;

DELETE FROM diary_logs WHERE id = 1;

SELECT * FROM diary_logs;
```

### ALTER

```sql
/* What we used to originally create the table */
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name TEXT);
    
CREATE TABLE diary_logs (
    id INTEGER PRIMARY KEY,
    user_id INTEGER,
    date TEXT,
    content TEXT
    );
    
/* After user submits a diary log */
INSERT INTO diary_logs (user_id, date, content) VALUES (1, "2015-04-02",
    "OhNoesGuy and I made up and now we're best friends forever and we celebrated with a tub of ice cream.");
    
ALTER TABLE diary_logs ADD emotion TEXT default "unknown";

INSERT INTO diary_logs (user_id, date, content, emotion) VALUES (1, "2015-04-03",
    "We went to Disneyland!", "happy");
    
SELECT * FROM diary_logs;
```

## Make your SQL safer

SQL can be a beautiful thing, but it can also be a dangerous thing. If you're using SQL to access a database for an app that's used by hundreds or thousands or even millions of users, you need to be careful - because you could accidentally damage or erase all that data. There are various techniques you can use to make your SQL safer, however.

### Avoiding bad updates/deletes

Before you issue an `UPDATE`, run a `SELECT` with the same `WHERE` to make sure you're updating the right column and row.

For example, before running:

```
UPDATE users SET deleted = true WHERE id = 1;
```

You could run:

```
SELECT id, deleted FROM users WHERE id = 1;
```

Once you decide to run the update, you can use the LIMIT operator to make sure you don't accidentally update too many rows:

```
UPDATE users SET deleted = true WHERE id = 1 LIMIT 1;
```

Or if you were deleting:

```
DELETE users WHERE id = 1 LIMIT 1;
```

### Using transactions

When we issue a SQL command that changes our database in some way, it starts what is called a "transaction." A transaction is a sequence of operations treated as a single logical piece of work (like a bank transaction), and in the world of databases, a transaction must comply to the "ACID" principles to make sure they are processed reliably.

Whenever we issue a command like `CREATE`, `UPDATE`, `INSERT`, or `DELETE`, we are automatically starting a transaction. However, if we want, we can also wrap up multiple commands inside a bigger transaction. It may be that we only want an `UPDATE` to go through if another `UPDATE` goes through as well, so we want to put both of them in the same transaction.

In that case, we can wrap the commands in `BEGIN TRANSACTION` and `COMMIT`:

```
BEGIN TRANSACTION;
UPDATE people SET husband = "Winston" WHERE user_id = 1;
UPDATE people SET wife = "Winnefer" WHERE user_id = 2;
COMMIT;
```

If the database is unable to issue both those `UPDATE` commands for some reason, then it will rollback the transaction and leave the database how it was when it started.

We also use transactions when we want to make sure that all of our commands operate on the same view of the data - when we want to ensure that no other transactions operate on that same data while the sequence of commands is running. When you're looking at a sequence of commands you want to run, ask yourself what would happen if another user issued commands at the same time. Could your data end up in a weird state? In that case, you should run in a transaction.

For example, the following commands create a row denoting that a user earned a badge, and then update the user's recent activity to describe that:

```
INSERT INTO user_badges VALUES (1, "SQL Master", "4pm");
UPDATE user SET recent_activity = "Earned SQL Master badge" WHERE id = 1;
```

At the same time, another user or process might be awarding the user a second badge:

```
INSERT INTO user_badges VALUES (1, "Great Listener", "4:05pm");
UPDATE user SET recent_activity = "Earned Great Listener badge" WHERE id = 1;
```

These commands could now actually be issued in this order:

```
INSERT INTO user_badges VALUES (1, "SQL Master");
INSERT INTO user_badges VALUES (1, "Great Listener");
UPDATE user SET recent_activity = "Earned Great Listener badge" WHERE id = 1;
UPDATE user SET recent_activity = "Earned SQL Master badge" WHERE id = 1;
```

Their recent activity would now be "Earned SQL Master badge" even though the most recently inserted badge was "Great listener". That's not the end of the world, but it's also probably not what we expected.

Instead, we could run those in a transaction, to guarantee that no other transactions happen in the middle:

```
BEGIN TRANSACTION;
INSERT INTO user_badges VALUES (1, "SQL Master");
UPDATE user SET recent_activity = "Earned SQL Master badge" WHERE id = 1;
COMMIT;
```

### Making backups

You should definitely follow all those tips, but sometimes mistakes still happen. Because of that, most companies make backups of their databases - on an hourly, daily, or weekly basis, depending on the size of the database and space available. When something bad happens, they can then import data from the old database for whichever tables were damaged or lost. The data may end up a little outdated, but outdated data is often better than no data at all.

### Replication

A related approach is replication - always storing multiple copies of the databases in different places. If for some reason a particular copy of the database is unavailable (like lightning hit the building that it's in, which has actually happened to me!), then the query can be sent to another copy of the database which is hopefully still available. If the data is very important, then it should probably be replicated to ensure availability. For example, if a doctor is trying to pull up a list of a patient's allergies to determine how to treat them in an emergency situation, then they can't afford to wait for engineers to get the data out of a backup, they need it immediately.

However, it is a lot more effort to replicate databases and it often means slower performance since write operations have to be performed in all of them, so companies must decide whether the benefits of replication are worth the costs, and investigate the best way of setting it up for their environment.

### Granting privileges

Many database systems have users and privileges built into them, because they are stored on a server and accessed by multiple users. There is no concept of user/privilege in the SQL scripts on Khan Academy, because SQLite is typically used in a single-user scenario, and thus you can write to it as long as you have access to the drive it's stored on.

But if you are using a database system on a shared server one day, then you should make sure to set up users and privileges properly from the beginning. As a general rule, there should be only a few users that have full access to the database (like backend engineers), since it can be so dangerous.

For example, here's how we can give full access to a particular user:

```
GRANT FULL ON TABLE users TO super_admin;
```

And here's how we can give only SELECT access to a different user:

```
GRANT SELECT ON TABLE users TO analyzing_user;
```

In a big company, you often don't even want to give `SELECT` access to most users, because there might be private data in a table, like a user's email address or name. Many companies have anonymized versions of their databases that they can query on without worrying about access to private information.