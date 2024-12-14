# Fall 2024 Principles of Databases — Assignment 4

* **Do not start this project until you’ve read and understood these instructions. If something is not clear, ask.**

---

## ❖ Introduction ❖

For this assignment, you will write responses to nine questions based on different topics from our textbook, *Database Systems — The Complete Book* and to one question based on your notes. Reply to each question in the provided region using Markdown syntax.

---

## ❖ Questions ❖

### 1. [2.4] What is the difference between a Cartesian Product, a Natural Join, and Theta-Joins?
Generally, a join is an operation on two tables that returns a relation where some or all of the tuples of both are combined in some fashion. Some of the table-operands in a join can be themselves defined as a specific type of join between two tables. 

In a cartesian product join, the resulting table consists of all combinations of tuples between the tables being joined. This results in a table with a column length that is the sum of the column length of the tables in the join and a row length of the product of the row lengths of the tables in the cross as reach tuple in the first table will pair with each table in the second one time. This type of join does not have directionality in that if we have table A and Table B being crossed, the distinction between the results will be trivial as the component values associated with each attribute will be consistent and only the sequence of the attributes will be altered. This characteristic of having only trivially unique distinctions between the hypothetical resultant relations is called `equivalent relations`.

Example:

R = 
| A | B |
|---|---| 
| 1 | 2 |
| 3 | 4 |

S = 
| B| C | D  |
|---|---|----| 
| 2 | 5 | 6 |
| 4 | 7 | 8 |
| 9 | 10 | 11 |

$ R \times S = $
| A | R.B | S.B| C | D  |
|---|---|---|---|----|
| 1 | 2 | 2 | 5 | 6 |
| 1 | 2 | 4 | 7 | 8 |
| 1 | 2 | 9 | 10 | 11 |
| 3 | 4 | 2 | 5 | 6 |
| 3 | 4 | 4 | 7 | 8 |
| 3 | 4 | 9 | 10 | 11 |

In a natural join, the resultant table is one that consists of a set conjoined tuples of the operand tables that only includes those tuples from the operands that have that share a value among the attributes that are shared between the tables. It is almost a subset of the the cartesian join between the same tables except that the attribute(s) that the tables are joined on are represented once (which reflects that those attributes would be fully redundant if represented for each table in this kind of join).

Example (using the prior S and R tuples):
$ R \bowtie S$

| A | B| C | D  |
|---|---|----|---|
| 1 | 2 | 5 | 6 |
| 3 | 4 | 7 | 8 |

In theta joins, the resultant table is effectively a subset of the cartesian product on the same operands where there is a logical condition in the join's definition that governs the restriction on the values returned.

$ R = $

|A |B |C |
|---|---|----|
|1 |2 |3 |
|6 |7 |8 |
|9 |7 |8|

$ S = $

|B |C |D |
|---|---|----|
|2 |3 |4 |
|2 |3 |5 |
|7 |8 |10|

$ R\bowtie_{A < D} S =$

|A |R.B |R.C |S.B |S.C |D |
|---|---|----|---|---|----|
|1 |2 |3 |2 |3 |4 |
|1 |2 |3 |2 |3 |5 |
|1 |2 |3 |7 |8 |10|
|6 |7 |8 |7 |8 |10|
|9 |7 |8 |7 |8 |10|


### 2. [2.5] What is a Referential Integrity Constraint?

Generally, a constraint in relational data model is a way to limit the value that a given tuple component associated with an attribute can take. A referential integrity constrain is a specific instance of one of these mechanisms where "a value appearing in one context also appears in another, related context" (Ullman, pg.59). The context being referred to is usually in two separate tables, but can be the same table (pg. 312). Ultimately, this particular restraint has the effect of limiting the set of tuple component values for one attribute to be a subset, or equal to, the set of component values in another attribute. In the context of relational algebra, this can be expressed in two synonymous ways:
1.  $\pi_A(R) \subseteq \pi_B(S)$
1. $\pi_A(R) - \pi_B(S)$ = 0

For both of these relational equations to be equivalent, it is important to understand that sets must be restricted to unique values. In practice however, one of the attributes will often allow for there to be value redundancies while the other will not. The essential factor in an effective referential integrity constraint is that the unique value of the referenced constraint is either set-equivalent, or a superset of, the referencing set. In the implementation we learned in Chapter 7 for foreign keys, the referenced set is one or more attributes that are also a `key constraint` (either a unique or primary key), while the referencing key is established as a `foreign key` consisting of equivalent attributes of the referenced key, with respect to the allowable values. 

###  3. [2.5] What is a Key Constraint?

As alluded to in answer 2, a `key constraint` is one that is self referential to the attribute(s) it is defined on, and restricts the components associated with the defining attributes to collectively unique values so that "no two tuples agree on the same component" (pg. 61). To be collectively unique, the groupings of tuple components associated with the key's attributes can be repeated individually between tuples in the table that the key is defined for but their sequence cannot be repeated. Thought of differently, any unique tuple of a table cannot have the same subsequence for its key constraint(s). We learned two implementations for declaring keys, including `primary keys` and `unique keys`. The primary key is distinguished from a unique key by the former being declarable only once in a table and disallowing NULL values for its associated components, while the latter can have multiple declarations in the same relation and allows for NULLs with the implication that for distinct tuple's, having NULL values for the unique key components is permissible.
### 4. [4.1] What is an Entity/Relationship Model? What purpose does it serve in the process of creating/designing databases?

Replace this content with your answer

### 5. [4.4] What is a Weak Entity Set?

Replace this content with your answer

### 6. [5.2.7; 6.3.8] Explain the concepts of Outerjoin, Natural Right Outer Joins, Natural Left Outer Joins, and Full Outer Joins.

Replace this content with your answer

### 7. [6.6.3] What is the difference between the SQL command `TRANSACTION` and the execution of any statement in SQL?

Transactions are groupings of operations clarified in statements that fall within specific syntax delimiters which ensure the impact these operations have on the database is only executed if all of the transformations that are to occur as a result are all effectively prepared and shown to be valid prior to the the changes they create being applied to the permanent state of the database.(pg. 300) The goal of this is to ensure that events executed on the database maintain both serialization (which ensures the operations of a transaction will run as if they execute sequentially), and atomicity (which prevents the potential instability that a partial completion of a series of operations could otherwise create outside of their being nested in a transaction). 

With this context in mind, statements will execute independently of each other so that one's completion with respect to the permanent storage of what is returned is not contingent on any of the other statements that might be scripted to run in sequence, while a transaction will only effect change to permanent storage when all of the statements that comprise it return values that are complete and validated. An individual statement is inherently a transaction when executed outside of another statement (pg. 299). The operations that individually define the attributes of a table do not execute independently for example. However, a formal transaction includes the command `START TRANSACTION` and is syntactically complete when both `COMMIT` and `ROLLBACK` statements are defined that handle the resolution of a transaction, with successful execution of **all** statements prompting the former and unsuccessful execution of **any** statement prompting the latter.

### 8. [8] What is a Virtual View and what are its advantages?
A virtual view (or just view) is one of three types of relations that can exist in a database, with the others being tables and temporary tables. It is a way to make the temporary tables perpetuate for the duration of the rest of a session. This allows it to be queried by the identifier created when the view was created using the `CREATE VIEW` command. Rather than a table being stored in memory so that it can be referenced by name in queries, views follow an identical querying process, but what is accessed from memory when a view's name is used in a SELECT-FROM-WHERE query is the query that was originally used in the definition of the query. This means that views are always produced from existing tables. 

Having views to query in addition to tables allows for the relations of each type to be referenced in subsequent queries making otherwise logically complex query syntax easier while producing the same outcomes. Also, as a result of establishing views, queries can be executed more quickly and also accommodate renaming of attributes included in the views schema, allowing for its logical distinction from the relation(s) it is generated from to be more explicit. 

In particular cases, views can be modified, adding to their advantages over temporary queries. These cases require that the SELECT-FROM-\<WHERE\> command that serves in the `AS` clause, defining the view in the declaration. These requirements include a single reference to a permanent table in the FROM clause, cannot reference this table in the WHERE clause, and the list of attributes in the SELECT clause must include at least all of the referenced table's attributes that cannot accept NULL values while also not having alternative defaults created. These modifiable cases are advantageous because modifications to them actually update their underlying tables effectively altering both, which saves on the effort associated with recreating queries that produce temporary relations of updated instances of the same table. 
### 9. [8.3] What is an *index* and what are its advantages? 
An index is a data structure produced from an attribute or set of attributes from a table. Effectively, these structures contain key-value pairs where the value-sequences of the indexed attribute(s) constitute the keys, and the addresses where the tuples associated with those keys are located constitute the values.

The advantages of indexes are that they produce efficiency in reading tuples because, instead of accessing all of the pages of all the tuples of a table in queries without reference to an index, queries with indexes first traverse the index structures themselves so that the addresses of matches reduce the number of accessed tables to only those pages containing the matching tuples. This means that less computation is required when traversing the smaller amount of data contained in the totality of the pages required for index queries as compared to those without indexes, especially as the tables in queries get very large. This advantage is compounded when multiple conditions exist in queries that include multiple indexes so that only specific tuples serve as operand values rather than their entire tables, significantly reducing time complexity for large table sizes. 

### 10. Explain the concept of an MVC, or model, view, controller, framework for designing full stack applications

Replace this content with your answer

---

## ❖ Due ❖

Sunday, 15 December 2024, at 10:00 AM.

---

## ❖ Grading ❖

| Item        | Points |
|-------------|:------:|
| Question 1  | `10`   |
| Question 2  | `10`   |
| Question 3  | `10`   |
| Question 4  | `10`   |
| Question 5  | `10`   |
| Question 6  | `10`   |
| Question 7  | `10`   |
| Question 8  | `10`   |
| Question 9  | `10`   |
| Question 10 | `10`   |

---

## ❖ Submission ❖

**NO late submissions will be accepted.**

You will need to issue a pull request back into the original repo, the one from which your fork was created for this project. See the **Issuing Pull Requests** section of [this site](http://code-warrior.github.io/tutorials/git/github/index.html) for help on how to submit your assignment.

**Note**: This assignment may **only** be submitted via GitHub. **No other form of submission will be accepted**.
