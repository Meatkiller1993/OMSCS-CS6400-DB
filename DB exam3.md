# SQL 

## SQL History
- SQL-structured query language
- SQL is based on relational tuple calculus and some algebra
- There are both ANSI and ISO standards for SQL. The first version of SQL was standardized in 1986, with a revision in 1989. SQL2 came later in 1992, and SQL3 was published in 1999. There have been several revisions since the release of SQL3: in 2003, 2006, 2008, and 2011.
- Many database products implement the SQL standard completely or partially, including **IBM Db2 (a commercial version of SYSTEM R), Oracle, Sybase, SQLServer, MySQL**, and many more.

## Insert

```sql
INSERT INTO UserInterests(Email, Interest)
 VALUES ('Sihai@gmail.com', 'driving');
```
- This method of insertion only adds a single row at a time into the database. 
- We can augment an insertion statement with a query selection on the database to insert the result of such a selection - zero or more tuples - into a table.

## Delete

Example: delete all tuples from UserInterests where Interest is 'Swimming'

```sql
DELETE FROM UserInterests
WHERE Interest = 'Swimming';
```

## Update
The following statement sets the Interest to 'Rock Music' for every tuple in UserInterests where Email is 'user3@gt.edu' and Interest is 'Music':

```sql
UPDATE UserInterests
SET Interest = 'Rock music'
WHERE Email = 'sihai@gmail.com'
    AND Interest = 'Music';
```

## General SQL Query Syntax
Generally, SQL queries looks like the following format:
```sql
SELECT column1, column2, column3
FROM table1, table2, table3
WHERE condition;
```

When discussing commercial relational databases, as opposed to algebra and calculus, we use the terms **column, table, and row**, instead of **attribute, relation, and tuple**, respectively. 

## Selection -and Wildcard

```sql
SELECT Email, Brithyear, SEX %all contributes
FROM ReugularUsers;
```

which is same to the following:

```sql
SELECT * %all contributes
FROM ReugularUsers;
```

## Selection -and Wildcard

```sql
SELECT Email, Brithyear, SEX %all contributes
FROM ReugularUsers;
```

## Selection -with a WHERE clause

```sql
SELECT * 
FROM ReugularUsers
WHERE HomeTown = 'Atlanta';
```

## Selection -with a composite WHERE clause

```sql
SELECT *
FROM ReugularUsers
WHERE CurrentCity = HomeTown OR HomeTown = 'Atlanta';
```

## Projection
If we only need information from some of the columns of the RegularUser table - namely, Email, BirthYear, and Sex - for all users who live in Atlanta. We can express this query as such:

```sql
SELECT Email, BirthYear, Sex
FROM RegularUser
WHERE HomeTown = 'Atlanta';
```

## Distinct
Relations are sets and that the result of a query is always a relation and, therefore, a set. In SQL, tables may have **_duplicate_** rows.

```sql
SELECT DISTINCT(Sex)
FROM RegularUser
WHERE HomeTown = 'Atlanta';
```

## Natural Inner Join - Dot Notation
Suppose we want to find the email, birth year, and salary for regular users who have a salary by joining the RegularUser table and the YearSalary table, the latter of which has BirthYear and Salary columns. The appropriate SQL query looks as follows:

```sql
SELECT Email, RegularUser.BirthYear, Salary
FROM RegularUser, YearSalary
WHERE RegularUser.BirthYear = YearSalary.BirthYear;
```
In relational algebra, we didn't have to specify the join condition when the attribute names were the same. In SQL, we must specify this condition: RegularUser.BirthYear = YearSalary.BirthYear. However, when the column names are the **_same_**, we also have an alternative syntax available to us:And it is same to the following:

```sql
SELECT Email, RegularUser.BirthYear, Salary
FROM RegularUser NATURAL JOIN YearSalary;
```

## Natural Inner Join - Aliases ##
As before, suppose we want to find the email, birth year, and salary for regular users who have a salary by joining the RegularUser table and the YearSalary table, the latter of which has BirthYear and Salary columns. We can use aliases to rewrite this query as follows: (same example as last one)

```sql
SELECT Email, R.BirthYear, Y. Salary
FROM RegularUser AS R, YearSalary AS Y
WHERE RegularUser.BirthYear = YearSalary.BirthYear;
```

SQL queries can become quite large and complex, and we can use aliases to save on typing. We also use aliases to disambiguate table references; in particular, we must use aliases when joining a table with itself to distinguish between the first and second instances of the table in the join.

## Left Outer Join ##
Suppose we want to find the email, birth year, and salary for regular users who have a salary by joining the RegularUser table and the YearSalary table. We also want to include regular users who have no salary in the result.

```sql
SELECT Email, RegularUser.BirthYear, Salary
FROM RegularUser LEFT OUTER JOIN YearSalary;
```

## String Matching ##
Suppose we want to find information about regular users who currently live in a city that starts with "San". We can express that query as such:

The percent sign, '%', above matches any string, including the empty string. For example, 'San%' matches the literal string 'San' and 'San' plus any number of subsequent characters. 

```sql
SELECT Email, Sex, CurrentCity
FROM RegularUser 
WHERE CurrentCity LIKE 'San%';
```

There are more types of wildcards in string matching. Whereas '%' matches zero or more characters, the '_' character matches exactly one character. For example:

```sql
SELECT Email, Sex, CurrentCity
FROM RegularUser 
WHERE CurrentCity LIKE 'A_____';
```

The '_' character matches exactly one character, so in this case, it will match "Austin" in the table since there are 6 charaters.

## Sorting ##

Sorting is another practical concern that does not have roots in algebra or calculus. Suppose we want to find data about regular male users and need that information sorted by current city. We can express that query as follows

```sql
SELECT Email, Sex, CurrentCity
FROM RegularUser 
WHERE Sex = 'M'
ORDER BY CurrentCity ASC;
```
Sort direction as ascending, ```ASC```, or descending, ```DESC```

## Set Operations - **Union** ##

Suppose we want to find all current cities and hometowns (without duplicates) from the RegularUser table. We form two queries - one that selects all current cities and one that selects all home towns - and then we find their set union using the UNION operator:

```sql
SELECT CurrentCity
FROM RegularUser
Union
SELECT HomeTown
FROM RegularUser;
```

Whereas SQL queries generally may return duplicates, the union, intersection, and set difference operators only return sets. If we want **duplicates** in our result, we would instead reach for the UNION ALL operator:

```sql
SELECT CurrentCity
FROM RegularUser
Union ALL
SELECT HomeTown
FROM RegularUser;
```

## Set Operations - **Intersect** ##

Suppose we want to find all cities that are someone's current city and someone's hometown without including any duplicates. We form two queries - one that selects all current cities and one that selects all home towns - and then we find their set intersection using the INTERSECT operator:

```sql
SELECT CurrentCity
FROM RegularUser
INTERSECT
SELECT HomeTown
FROM RegularUser;
```

Similar to what we saw in ```UNION``` operator, the INTERSECT operator removes duplicates from the resulting table. If we want duplicates in our result, we would instead reach for the ```INTERSECT ALL```:

```sql
SELECT CurrentCity
FROM RegularUser
INTERSECT ALL
SELECT HomeTown
FROM RegularUser;
```

## Set Operations - **Except** ##

Suppose we want to find all cities that are someone's current city but not someone's hometown without including duplicates:

```sql
SELECT CurrentCity
FROM RegularUser
EXCEPT
SELECT HomeTown
FROM RegularUser;
```

Similar to what we saw in ```UNION``` and ```INTERSECT``` operator, the EXCEPT operator removes duplicates from the resulting table. If we want duplicates in our result, we would instead reach for the ```EXCEPT ALL```:

```sql
SELECT CurrentCity
FROM RegularUser
EXCEPT ALL
SELECT HomeTown
FROM RegularUser;
```

