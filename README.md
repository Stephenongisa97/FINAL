# FINAL
-- Database: LibraryManagement
CREATE DATABASE LibraryManagement;

-- Use the database
USE LibraryManagement;

-- Table 1: Authors
CREATE TABLE Authors (
    AuthorID INT AUTO_INCREMENT PRIMARY KEY,
    FirstName VARCHAR(100) NOT NULL,
    LastName VARCHAR(100) NOT NULL,
    BirthDate DATE,
    UNIQUE (FirstName, LastName)  -- Ensuring no duplicate authors with the same name
);

-- Table 2: Books
CREATE TABLE Books (
    BookID INT AUTO_INCREMENT PRIMARY KEY,
    Title VARCHAR(255) NOT NULL,
    AuthorID INT,
    Genre VARCHAR(100),
    PublicationYear INT,
    ISBN VARCHAR(13) UNIQUE,  -- Ensuring ISBN is unique
    FOREIGN KEY (AuthorID) REFERENCES Authors(AuthorID) ON DELETE SET NULL -- One author can write many books
);

-- Table 3: Members (Users)
CREATE TABLE Members (
    MemberID INT AUTO_INCREMENT PRIMARY KEY,
    FirstName VARCHAR(100) NOT NULL,
    LastName VARCHAR(100) NOT NULL,
    Email VARCHAR(255) UNIQUE NOT NULL,
    PhoneNumber VARCHAR(15),
    DateJoined DATE NOT NULL
);

-- Table 4: Book Loans
CREATE TABLE BookLoans (
    LoanID INT AUTO_INCREMENT PRIMARY KEY,
    MemberID INT,
    BookID INT,
    LoanDate DATE NOT NULL,
    ReturnDate DATE,
    FOREIGN KEY (MemberID) REFERENCES Members(MemberID) ON DELETE CASCADE,  -- One member can have multiple loans
    FOREIGN KEY (BookID) REFERENCES Books(BookID) ON DELETE CASCADE  -- One book can have multiple loans
);

-- Sample Data Insertion

-- Insert authors
INSERT INTO Authors (FirstName, LastName, BirthDate) VALUES 
('J.K.', 'Rowling', '1965-07-31'),
('George', 'Orwell', '1903-06-25');

-- Insert books
INSERT INTO Books (Title, AuthorID, Genre, PublicationYear, ISBN) VALUES 
('Harry Potter and the Philosopher\'s Stone', 1, 'Fantasy', 1997, '9780747532743'),
('1984', 2, 'Dystopian', 1949, '9780451524935');

-- Insert members
INSERT INTO Members (FirstName, LastName, Email, PhoneNumber, DateJoined) VALUES
('Alice', 'Smith', 'alice.smith@email.com', '123-456-7890', '2021-01-15'),
('Bob', 'Johnson', 'bob.johnson@email.com', '987-654-3210', '2020-05-10');

-- Insert book loans
INSERT INTO BookLoans (MemberID, BookID, LoanDate, ReturnDate) VALUES
(1, 1, '2023-03-15', '2023-04-15'),  -- Alice borrowed Harry Potter
(2, 2, '2023-04-01', NULL);  -- Bob borrowed 1984, still not returned
