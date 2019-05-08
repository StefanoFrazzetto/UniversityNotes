# Introduction

## Relational databases

These are databases where data is organized through the relational model, first described by E. F. Codd in 1969. The model organizes data into tables (or relations), each with a number of columns -- identifying a property of the table -- and rows -- containing the instances of data for that table.

The software used to maintain a relational database is known as Relational Database Management System (RDBMS) and, almost in all cases, make use of SQL (Structured Query Language) for retrieving and maintaining data.

Relational databases are usually chosen over other types of databases because of the certainty they provide through the mathematical principles, i.e. data model, they are based upon. The data model formally defines:
- Data structures
- Operations
- Table keys
- Integrity/consistency constraints

## NoSQL databases

Non-relational databases are referred to as NoSQL databases because they use a different query languages. They are particularly useful for applications involving big data, and generally the storage of non-structured documents. The advantages of these databases, over the relational ones, are performance -- because of the less strict checks on data -- and scalability. Although data also have a broadly defined structure, NoSQL database don't enforce fixed schemas.

### Big data

Big data refers to large amount of complex data that requires a number of techniques to be acquired, analyzed, and stored. This type of data is produced in different context, e.g. online shopping, sensors information, etc., and is rarely organized using a fixed structure.

Relational databases are often not efficient in handling this type of data, and that's why NoSQL databases are used instead: they are 
