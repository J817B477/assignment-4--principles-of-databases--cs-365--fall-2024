# Fall 2024 Principles of Databases — Assignment 4

* **Do not start this project until you’ve read and understood these instructions. If something is not clear, ask.**

---

## ❖ Introduction ❖

For this assignment, you will write responses to nine questions based on different topics from our textbook, *Database Systems — The Complete Book* and to one question based on your notes. Reply to each question in the provided region using Markdown syntax.

---

## ❖ Questions ❖

### 1. [2.4] What is the difference between a Cartesian Product, a Natural Join, and Theta-Joins?
Generally, a join is an operation on two relations that returns a derived relation where some or all of the tuples of both contributing relations are combined in some fashion.

In a cartesian product join, the resulting relation consists of all combinations of tuples between the relations being joined. This results in a relation with a column length that is the sum of the column lengths of the relations in the join, and a row length that is the product of the row lengths of the relations in the join (as each tuple in the first relation will pair with each relation in the second one time). This type of join does not have directionality in that if we have relation A and relation B being crossed, the distinction between the results will be trivial as the component values associated with each attribute will be consistent and only the sequence of the attributes will be altered. This characteristic of having only trivially unique distinctions between the hypothetical resultant relations is called `equivalent relations` and is a characteristic of the other joins discussed here as well.

Example:

R = 
| A | B |
|---|---| 
| 2 | 1 |
| 4 | 3|

S = 
| B| C | D  |
|---|---|----| 
| 6 | 5 | 2 |
| 3 | 7 | 4 |
| 1 | 10 | 9 |

$ R \times S = $
| A | R.B | S.B| C | D  |
|---|---|---|---|----|
| 2 | 1 | 6 | 5 | 2 |
| 2 | 1 | 3 | 7 | 4 |
| 2 | 1 | 1 | 10 | 9 |
| 4 | 3 | 6 | 5 | 2 |
| 4 | 3 | 3 | 7 | 4 |
| 4 | 3 | 1 | 10 | 9 |

In a natural join, the resultant relation is one that consists of a set of conjoined tuples of the operand relations that only includes those tuples that share a value among the attributes that are common to both relations. It is almost a subset of the the cartesian join between the same relations except that the attribute(s) that the relations are joined on are represented once (which reflects the fact that those attributes would be fully redundant if represented for each relation in this kind of join).

Example (using the prior S and R tuples):
$ R \bowtie S$

| A | B | C  | D |
|---|---|----|---|
| 2 | 1 | 10 | 9 |
| 4 | 3 | 7  | 4 |

In theta joins, the resultant relation is effectively a subset of the cartesian product on the same operands where there is a logical condition in the join's definition that governs the restriction on the tuples returned.

$ R = $

|A |B |C |
|---|---|----|
|1 |4 |5 |
|6 |6 |9 |
|9 |6 |9|

$ S = $

|B |C |D |
|---|---|----|
|4 |5 |4 |
|4 |5 |5 |
|6 |9 |10|

$ R\bowtie_{A < D} S =$

|A  |R.B|R.C|S.B|S.C|D  |
|---|---|---|---|---|---|
|1  |4  |5  |4  |5  |4  |
|1  |4  |5  |4  |5  |5  |
|1  |4  |5  |6  |9  |10 |
|6  |6  |9  | 6 |9  |10 |
|9  |6  |9  | 6 |9  |10 |



### 2. [2.5] What is a Referential Integrity Constraint?

Generally, a constraint in a relational data model is a way to limit the value that a given tuple component associated with an attribute can take. A referential integrity constrain is a specific instance of one of these mechanisms where "a value appearing in one context also appears in another, related context" (Ullman, pg.59). The context being referred to is usually in two separate relations, but can be the same relation (pg. 312). Ultimately, this particular restraint has the effect of limiting the set of tuple component values for one attribute to be a subset, or equal to, the set of component values in another attribute. In the context of relational algebra, this can be expressed in two synonymous ways:
1.  $\pi_A(R) \subseteq \pi_B(S)$
1. $\pi_A(R) - \pi_B(S)$ = 0

For both of these relational equations to be equivalent, it is important to understand that sets must be restricted to unique values. In practice however, one of the attributes will often allow for there to be value redundancies while the other will not. The essential factor in an effective referential integrity constraint is that the unique value of the referenced constraint is either set-equivalent, or a superset of, the referencing set. In the implementation we learned in Chapter 7 for foreign keys, the referenced set is one or more attributes that are also a `key constraint` (either a unique or primary key), while the referencing key is established as a `foreign key` consisting of equivalent attributes of the referenced key, with respect to the allowable values. 

###  3. [2.5] What is a Key Constraint?

As alluded to in answer 2, a `key constraint` is one that is self referential to the attribute(s) it is defined on, and restricts the components associated with the defining attributes to "collectively unique" values so that "no two tuples agree on the same component" (pg. 61). To be collectively unique, the groupings of tuple components associated with the key's attributes can be repeated individually between tuples in the table that the key is defined for but their sequence across all the key attributes cannot be repeated among tuples. Thought of differently, any unique tuple of a table cannot have the same subsequence for its key constraint(s). there are different protocols for declaring keys, including `primary keys` and `unique keys`. The primary key is distinguished from a unique key by the former being declarable only once in a table and disallowing NULL values for its associated components, while the latter can have multiple declarations in the same relation and allows for NULLs.

### 4. [4.1] What is an Entity/Relationship Model? What purpose does it serve in the process of creating/designing databases?

An Entity/Relationship (E/R) Model is a higher-level model that is used in the design phase of a database implementation to articulate both the relationships that will exist between entities and the entities themselves so that they can be converted to a relational model. Because relation models are composed of relations that do not necessarily explicitly articulate other concepts that exist in databases, E/R Models are used to establish the components of a database conceptually with a higher level of abstraction so that all of the implicit aspects of the schema for the implemented database can be articulated without specifying the more granular aspects like the tuples that would occupy the tables when the database is stood up and modified. This is achieved by graphically representing relations as entity sets, and relationships along with the attributes that are associated with each. 

Entity sets (graphically, rectangles nodes) represent relations whose identities consist of items that occupy the database while relationships (graphically, diamond nodes) represent relations that represent behaviors between items of a database indicating how they relate to each other. As a convenient distinction, entity sets and relationships can be thought of as nouns and verbs, respectively. The last principle component in E/R models are attributes (graphically, oval nodes) that directly represent the attributes of the eventual relations in the conversion to the relation model and its implementation as a database. 

Entity sets consist of entities that represent the tuples of the tables that entity sets convert to, but are not defined discretely in the design model and are only alluded to by the identity of the entity set they belong to and the attributes that are linked to the entity set. 

Relationship sets consist of the hypothetical conjoined entities comprised of entities from the individual entity sets in a defined relationship with each other. Unlike entities of entity sets, they will not be the tuples that come from table creations but will instead come to fruition through the execution of queries that involve the tables in a relationship as well as table constraints (specifically referential integrity constraints). Therefore, their consideration in the development of the E/R model is for the sake of anticipating those queries. 

The association of the attributes that are connected to entity sets require consideration of what constitutes an entity's identity from the perspective of its components. Relationships' associated attributes include those attributes of the entity sets connected by a given relationship that are necessary to establish relationship sets. They also include any attributes that would not be appropriate members of the entity sets involved in a given relationship, yet whose values distinguish the connected entities of the relationship sets. These relationship-specific attributes can also be used to distinguish different relationships between the same entity sets. 

The last major aspect of E/R models are the connections between the E/R model's principle components. They are represented as edges between entity sets, relationship sets, and attributes as they logically exist in a model. Edges are used to represent multiplicity in relationships where the unadorned entity-set ends of edges indicate that an entity set has a "many" relationship to other entity sets it is in a relationship with, and arrow-tip adorned entity-set ends of edges indicate "one" relationship to other relationships. The combinations of these edges from a relationship to the entity sets that are part of the relationship collectively indicate many-one, one-one, or many-many relationship multiplicity types.

### 5. [4.4] What is a Weak Entity Set?

A weak entity set is any where some or all element attributes of its key belongs to another entity set. It is characterized as belonging to a hierarchy, but is not an "isa" instance of its superset (which has different implications for the identity of its keys). Instead, its entities will either be subunits of the entities of its superset or be a connecting entity set that is utilized to restate a multi-way relationship as multiple binary relationships. An example of a weak set as a subunit is an ingredient entity set that is a subset of a recipe entity set. Its primary keys are at least in part composed of the keys of **supporting entity sets** that the weak entity set relies on to complete its full identity. 

### 6. [5.2.7; 6.3.8] Explain the concepts of Outerjoin, Natural Right Outer Joins, Natural Left Outer Joins, and Full Outer Joins.

Outer joins augment the resultant relation of other join types with dangling tuples that are padded by NULL values in tuples where one of the relations does not pair with a tuple of the other. These augmented tuples reflect an unconstrained referential relationship between the relations that produce them when joined. In natural joins, the resultant relation consists of the conjoined tuples that successfully pair. In this context, the outer join adds incomplete tuples (as described above) so that those tuples are represented in the resultant relation that would otherwise not be revealed as part of the database. The incomplete tuples that are added depend on the specific outer join used. A Natural Right Outer Join will include all of the tuples in the relation to the right (ie, the second relation) of the two relations in the query statement. Any of those tuples that don't pair, due to not having a value common to the variable(s) that it joins on, are completed with NULLs for the attribute of the left attribute. The left join works similarly, but with all of the tuples of the left (first) relation in the query fully represent and the attributes from the right relation being padded with NULLs. For a Full Outer Join, all tuples from both relations are included and any unpaired tuples are padded with NULLs for whichever set of attributes from the relations that is not otherwise occupied.

### 7. [6.6.3] What is the difference between the SQL command `TRANSACTION` and the execution of any statement in SQL?

Transactions are groupings of operations clarified as statements that fall within specific syntax delimiters. They ensure the impact these statements have on the database is only executed if all of the statement effects that are to occur as a result are all effectively prepared and shown to be valid prior to the the changes being applied to the permanent state of the database.(pg. 300) The goal of this is to ensure that events executed on the database maintain both serialization (which ensures the operations of a transaction will run as if they execute sequentially), and atomicity (which prevents the potential instability that a partial completion of a series of operations could otherwise create outside of their being nested in a transaction). 

With this context in mind, statements will execute independently of each other so that one's completion with respect to the permanent storage of what is returned is not contingent on any of the other statements that might be scripted to run in sequence, while transactions will only effect change to permanent storage when all of the statements that comprise them return values that are complete and validated. An individual statement is inherently a transaction when executed outside of another statement (pg. 299). For example,the operations that individually define the attributes of a table do not execute independently. However, a formal transaction includes the command `START TRANSACTION` and is syntactically complete when both `COMMIT` and `ROLLBACK` statements are defined that handle any condition of a transaction. The successful execution of **all** statements prompt the COMMIT statement, and an unsuccessful execution of **any** statement prompts the ROLLBACK statement.

### 8. [8] What is a Virtual View and what are its advantages?
A virtual view (or just view) is one of three types of relations that can exist in a database, with the others being tables and temporary relations. It is a way to make otherwise temporary relations perpetuate for the duration of the rest of a session. This allows it to be queried by the identifier created when the view was created using the `CREATE VIEW` command. Rather than a table being stored in memory so that it can be referenced by name in queries, views follow an identical querying process. However, what is accessed from memory when a view's name is used is the query that was originally used in the definition of the query.

Having views to query in addition to tables allows for the relations of each type to be referenced together in subsequent queries, making otherwise logically complex query syntax easier while producing the same outcomes. Also, as a result of establishing views, queries can be executed more quickly and also accommodate renaming of attributes included in the views schema, allowing for its logical distinction from the relation(s) it is generated from to be more explicit. 

When particular requirements are met, views can be modified, adding to their advantages over temporary queries. These requirements include a single reference to a permanent table in the FROM clause, no reference to this table in the WHERE clause, and the list of attributes in the SELECT clause must include at least all of the referenced table's attributes that cannot accept NULL values while also not having alternative defaults created. These modifiable cases are advantageous because modifications to them actually update their underlying tables, effectively altering both, which saves on the effort associated with updating a table and then querying it to produce what the updated view produces on its own. 

### 9. [8.3] What is an *index* and what are its advantages? 
An index is a data structure produced from an attribute or set of attributes from a table. Effectively, these structures contain key-value pairs where the value-sequences of the indexed attribute(s) constitute the keys, and the addresses where the tuples associated with those keys are located constitute the values.

The advantages of indexes are that they produce efficiency in reading tuples because, instead of accessing all of the pages of all the tuples of a table in queries without reference to an index, queries with indexes first traverse the index structures themselves so that the addresses of matches reduce the number of accessed pages to those containing the matching tuples. This means that less computation is required when traversing the smaller amount of data contained in the totality of the pages required for index queries as compared to those without indexes, especially as the tables in queries get very large. This advantage is compounded when multiple conditions exist in queries that include multiple indexes so that only specific tuples serve as operand values rather than their entire tables. This significantly reduces time complexity for large table sizes. 

### 10. Explain the concept of an MVC, or model, view, controller, framework for designing full stack applications

A Model-View-Controller framework is a software application implementation approach that distinguishes components of a fullstack workflow into particular roles to be implemented. The model refers to the data source and storage faculties of the application. Depending on the application, it can be implemented in a simple flat file structure, like xml, or a robust database type like that of a Relational Database Management System or NoSQL Management System with high levels of manipulability and parameterization built into it to control how and what data can be retrieved and added to the model. 

The view refers to the user interface implementation of the application which can be as simple as a markup language script like html, though many of the components of the user interface can be manipulated by the controller as interaction plays out. Additionally, markup languages like html files can sometimes accommodate controller languages within it, such as javascript. Highly developed stacks like MERN will have built out frameworks specifically for designing graphical interfaces such as its React.js component.

The Controller is the facilitator of the flow of information between, and the actions carried out on, the other components. It will handle user interactions with the interface to channel data to and from the model component using CRUD commands, and update the output of the view component to reflect changes made to the model component and/or information retrieved from it. In the MERN stack, Express.js and Node.js provide the functionality of the controller where Express is built on top of Node's functionality to simplify the controller component's implementation. 
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
