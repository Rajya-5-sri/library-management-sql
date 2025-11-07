# library-management-sql

# Library Management SQL Database

## Project Description
This is a beginner-level SQL project to design and manage a simple library system database.  
It covers table creation, insertion of sample data, and common SQL operations like selects, joins and updates.

## Requirements
- SQL Server / MySQL / PostgreSQL (Adjust based on your preferred database engine)
- SQL client or IDE to run SQL scripts (e.g., MySQL Workbench, pgAdmin, Azure Data Studio)

## How to Run
1. Create the database and tables by running the `CREATE` statements in the provided SQL script.
2. Insert sample data using the `INSERT` statements.
3. Run example queries included in the script to interact with the data.
4. Modify and extend the database as needed for practice.




create database library_management
use library_management


CREATE TABLE Books (
    BookID INT PRIMARY KEY,
    Title VARCHAR(100),
    Author VARCHAR(50),
    PublishedYear INT,
    Genre VARCHAR(30)
)


CREATE TABLE Members (
    MemberID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    JoinDate DATE
)


CREATE TABLE Loans (
    LoanID INT PRIMARY KEY,
    BookID INT,
    MemberID INT,
    LoanDate DATE,
    ReturnDate DATE,
    FOREIGN KEY (BookID) REFERENCES Books(BookID),
    FOREIGN KEY (MemberID) REFERENCES Members(MemberID)
)


INSERT INTO Books VALUES
(1, 'To Kill a Mockingbird', 'Harper Lee', 1960, 'Fiction'),
(2, '1984', 'George Orwell', 1949, 'Dystopian'),
(3, 'The Great Gatsby', 'F. Scott Fitzgerald', 1925, 'Classic')


INSERT INTO Members VALUES
(101, 'Alice', 'Smith', '2024-01-15'),
(102, 'Bob', 'Brown', '2024-03-22')


INSERT INTO Loans VALUES
(1001, 1, 101, '2025-10-01', '2025-10-15'),
(1002, 2, 102, '2025-10-05', NULL)


--loans that are not yet returened
select * from loans where returndate is NULL

--List members who borrowed a specific book(1964)
SELECT M.FirstName, M.LastName from members M
JOIN Loans L ON M.MemberID = L.MemberID
JOIN Books B ON L.BookID = B.BookID
WHERE B.Title = '1984'

--Inserting new values
INSERT INTO Books (BookID, Title, Author, PublishedYear, Genre)
VALUES (4, 'Pride and Prejudice', 'Jane Austen', 1813, 'Romance')

INSERT INTO Members (MemberID, FirstName, LastName, JoinDate)
VALUES (103, 'Charlie', 'Davis', '2025-02-01')

INSERT INTO Loans (LoanID, BookID, MemberID, LoanDate, ReturnDate)
VALUES (1003, 4, 103, '2025-11-01', NULL)
 
 select * from books
 select * from members
 select * from loans

 --updating a member's last name
 update members set lastname = 'Jonson' where memberid = 101

--Find books loaned out but not yet returned
SELECT B.Title, M.FirstName, M.LastName, L.LoanDate
FROM Loans L
JOIN Books B ON L.BookID = B.BookID
JOIN Members M ON L.MemberID = M.MemberID
WHERE L.ReturnDate IS NULL
