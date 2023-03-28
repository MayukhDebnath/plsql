# plsql
1. Library Management System
CREATE PROC dbo.LibraryManagementSystemProcedure
AS
CREATE DATABASE db_LibraryManagement

GO
	/* ======================= TABLES ========================*/


	CREATE TABLE tbl_publisher (
		publisher_PublisherName VARCHAR(100) PRIMARY KEY NOT NULL,
		publisher_PublisherAddress VARCHAR(200) NOT NULL,
		publisher_PublisherPhone VARCHAR(50) NOT NULL,
	);

	CREATE TABLE tbl_book (
		book_BookID INT PRIMARY KEY NOT NULL IDENTITY (1,1),
		book_Title VARCHAR(100) NOT NULL,
		book_PublisherName VARCHAR(100) NOT NULL CONSTRAINT fk_publisher_name1 FOREIGN KEY REFERENCES tbl_publisher(publisher_PublisherName) ON UPDATE CASCADE ON DELETE CASCADE,
	);

	CREATE TABLE tbl_library_branch (
		library_branch_BranchID INT PRIMARY KEY NOT NULL IDENTITY (1,1),
		library_branch_BranchName VARCHAR(100) NOT NULL,
		library_branch_BranchAddress VARCHAR(200) NOT NULL,
	);

	SELECT * FROM tbl_library_branch

	CREATE TABLE tbl_borrower (
		borrower_CardNo INT PRIMARY KEY NOT NULL IDENTITY (100,1),
		borrower_BorrowerName VARCHAR(100) NOT NULL,
		borrower_BorrowerAddress VARCHAR(200) NOT NULL,
		borrower_BorrowerPhone VARCHAR(50) NOT NULL,
	);

	SELECT * FROM tbl_borrower

	CREATE TABLE tbl_book_loans (
		book_loans_LoansID INT PRIMARY KEY NOT NULL IDENTITY (1,1),
		book_loans_BookID INT NOT NULL CONSTRAINT fk_book_id1 FOREIGN KEY REFERENCES tbl_book(book_BookID) ON UPDATE CASCADE ON DELETE CASCADE,
		book_loans_BranchID INT NOT NULL CONSTRAINT fk_branch_id1 FOREIGN KEY REFERENCES tbl_library_branch(library_branch_BranchID) ON UPDATE CASCADE ON DELETE CASCADE,
		book_loans_CardNo INT NOT NULL CONSTRAINT fk_cardno FOREIGN KEY REFERENCES tbl_borrower(borrower_CardNo) ON UPDATE CASCADE ON DELETE CASCADE,
		book_loans_DateOut VARCHAR(50) NOT NULL,
		book_loans_DueDate VARCHAR(50) NOT NULL,
	);

	SELECT * FROM tbl_book_loans
		/* Use GETDATE() to retrieve the date values for Date out. Use DATEADD for the DueDate*/

	CREATE TABLE tbl_book_copies (
		book_copies_CopiesID INT PRIMARY KEY NOT NULL IDENTITY (1,1),
		book_copies_BookID INT NOT NULL CONSTRAINT fk_book_id2 FOREIGN KEY REFERENCES tbl_book(book_BookID) ON UPDATE CASCADE ON DELETE CASCADE,
		book_copies_BranchID INT NOT NULL CONSTRAINT fk_branch_id2 FOREIGN KEY REFERENCES tbl_library_branch(library_branch_BranchID) ON UPDATE CASCADE ON DELETE CASCADE,
		book_copies_No_Of_Copies INT NOT NULL,
	);

	SELECT * FROM tbl_book_copies

	CREATE TABLE tbl_book_authors (
		book_authors_AuthorID INT PRIMARY KEY NOT NULL IDENTITY (1,1),
		book_authors_BookID INT NOT NULL CONSTRAINT fk_book_id3 FOREIGN KEY REFERENCES tbl_book(book_BookID) ON UPDATE CASCADE ON DELETE CASCADE,
		book_authors_AuthorName VARCHAR(50) NOT NULL,
	);

	SELECT * FROM tbl_book_authors

/*======================== END TABLES ======================*/


/*==================== POPULATING TABLES ======================*/

	INSERT INTO tbl_publisher
		(publisher_PublisherName, publisher_PublisherAddress, publisher_PublisherPhone)
		VALUES
		('DAW Books','375 Hudson Street, New York, NY 10014','212-366-2000'),
		('Viking','375 Hudson Street, New York, NY 10014','212-366-2000'),
		('Signet Books','375 Hudson Street, New York, NY 10014','212-366-2000'),
		('Chilton Books','Not Available','Not Available'),
		('George Allen & Unwin','83 Alexander Ln, Crows Nest NSW 2065, Australia','+61-2-8425-0100'),
		('Alfred A. Knopf','The Knopf Doubleday Group Domestic Rights, 1745 Broadway, New York, NY 10019','212-940-7390'),		
		('Bloomsbury','Bloomsbury Publishing Inc., 1385 Broadway, 5th Floor, New York, NY 10018','212-419-5300'),
		('Shinchosa','Oga Bldg. 8, 2-5-4 Sarugaku-cho, Chiyoda-ku, Tokyo 101-0064 Japan','+81-3-5577-6507'),
		('Harper and Row','HarperCollins Publishers, 195 Broadway, New York, NY 10007','212-207-7000'),
		('Pan Books','175 Fifth Avenue, New York, NY 10010','646-307-5745'),
		('Chalto & Windus','375 Hudson Street, New York, NY 10014','212-366-2000'),
		('Harcourt Brace Jovanovich','3 Park Ave, New York, NY 10016','212-420-5800'),
		('W.W. Norton',' W. W. Norton & Company, Inc., 500 Fifth Avenue, New York, New York 10110','212-354-5500'),
		('Scholastic','557 Broadway, New York, NY 10012','800-724-6527'),
		('Bantam','375 Hudson Street, New York, NY 10014','212-366-2000'),
		('Picador USA','175 Fifth Avenue, New York, NY 10010','646-307-5745')		
	;

	SELECT * FROM tbl_publisher

	INSERT INTO tbl_book
		(book_Title, book_PublisherName)
		VALUES 
		('The Name of the Wind', 'DAW Books'),
		('It', 'Viking'),
		('The Green Mile', 'Signet Books'),
		('Dune', 'Chilton Books'),
		('The Hobbit', 'George Allen & Unwin'),
		('Eragon', 'Alfred A. Knopf'),
		('A Wise Mans Fear', 'DAW Books'),
		('Harry Potter and the Philosophers Stone', 'Bloomsbury'),
		('Hard Boiled Wonderland and The End of the World', 'Shinchosa'),
		('The Giving Tree', 'Harper and Row'),
		('The Hitchhikers Guide to the Galaxy', 'Pan Books'),
		('Brave New World', 'Chalto & Windus'),
		('The Princess Bride', 'Harcourt Brace Jovanovich'),
		('Fight Club', 'W.W. Norton'),
		('Holes', 'Scholastic'),
		('Harry Potter and the Chamber of Secrets', 'Bloomsbury'),
		('Harry Potter and the Prisoner of Azkaban', 'Bloomsbury'),
		('The Fellowship of the Ring', 'George Allen & Unwin'),
		('A Game of Thrones', 'Bantam'),
		('The Lost Tribe', 'Picador USA');

	SELECT * FROM tbl_book WHERE book_PublisherName = 'George Allen & Unwin'

	INSERT INTO tbl_library_branch
		(library_branch_BranchName, library_branch_BranchAddress)
		VALUES
		('Sharpstown','32 Corner Road, New York, NY 10012'),
		('Central','491 3rd Street, New York, NY 10014'),
		('Saline','40 State Street, Saline, MI 48176'),
		('Ann Arbor','101 South University, Ann Arbor, MI 48104');

	/*UPDATE tbl_library_branch
	SET library_branch_BranchName = 'Central'
	WHERE library_branch_BranchID = 2;*/

	SELECT * FROM tbl_library_branch

	INSERT INTO tbl_borrower
		(borrower_BorrowerName, borrower_BorrowerAddress, borrower_BorrowerPhone)
		VALUES
		('Joe Smith','1321 4th Street, New York, NY 10014','212-312-1234'),
		('Jane Smith','1321 4th Street, New York, NY 10014','212-931-4124'),
		('Tom Li','981 Main Street, Ann Arbor, MI 48104','734-902-7455'),
		('Angela Thompson','2212 Green Avenue, Ann Arbor, MI 48104','313-591-2122'),
		('Harry Emnace','121 Park Drive, Ann Arbor, MI 48104','412-512-5522'),
		('Tom Haverford','23 75th Street, New York, NY 10014','212-631-3418'),
		('Haley Jackson','231 52nd Avenue New York, NY 10014','212-419-9935'),
		('Michael Horford','653 Glen Avenue, Ann Arbor, MI 48104','734-998-1513');

	SELECT * FROM tbl_borrower

	INSERT INTO tbl_book_loans
		(book_loans_BookID, book_loans_BranchID, book_loans_CardNo, book_loans_DateOut, book_loans_DueDate)
		VALUES
		('1','1','100','1/1/18','2/2/18'),
		('2','1','100','1/1/18','2/2/18'),
		('3','1','100','1/1/18','2/2/18'),
		('4','1','100','1/1/18','2/2/18'),
		('5','1','102','1/3/18','2/3/18'),
		('6','1','102','1/3/18','2/3/18'),
		('7','1','102','1/3/18','2/3/18'),
		('8','1','102','1/3/18','2/3/18'),
		('9','1','102','1/3/18','2/3/18'),
		('11','1','102','1/3/18','2/3/18'),
		('12','2','105','12/12/17','1/12/18'),
		('10','2','105','12/12/17','1/12/17'),
		('20','2','105','2/3/18','3/3/18'),
		('18','2','105','1/5/18','2/5/18'),
		('19','2','105','1/5/18','2/5/18'),
		('19','2','100','1/3/18','2/3/18'),
		('11','2','106','1/7/18','2/7/18'),
		('1','2','106','1/7/18','2/7/18'),
		('2','2','100','1/7/18','2/7/18'),
		('3','2','100','1/7/18','2/7/18'),
		('5','2','105','12/12/17','1/12/18'),
		('4','3','103','1/9/18','2/9/18'),
		('7','3','102','1/3/18','2/3/18'),
		('17','3','102','1/3/18','2/3/18'),
		('16','3','104','1/3/18','2/3/18'),
		('15','3','104','1/3/18','2/3/18'),
		('15','3','107','1/3/18','2/3/18'),
		('14','3','104','1/3/18','2/3/18'),
		('13','3','107','1/3/18','2/3/18'),
		('13','3','102','1/3/18','2/3/18'),
		('19','3','102','12/12/17','1/12/18'),
		('20','4','103','1/3/18','2/3/18'),
		('1','4','102','1/12/18','2/12/18'),
		('3','4','107','1/3/18','2/3/18'),
		('18','4','107','1/3/18','2/3/18'),
		('12','4','102','1/4/18','2/4/18'),
		('11','4','103','1/15/18','2/15/18'),
		('9','4','103','1/15/18','2/15/18'),
		('7','4','107','1/1/18','2/2/18'),
		('4','4','103','1/1/18','2/2/18'),
		('1','4','103','2/2/17','3/2/18'),
		('20','4','103','1/3/18','2/3/18'),
		('1','4','102','1/12/18','2/12/18'),
		('3','4','107','1/13/18','2/13/18'),
		('18','4','107','1/13/18','2/13/18'),
		('12','4','102','1/14/18','2/14/18'),
		('11','4','103','1/15/18','2/15/18'),
		('9','4','103','1/15/18','2/15/18'),
		('7','4','107','1/19/18','2/19/18'),
		('4','4','103','1/19/18','2/19/18'),
		('1','4','103','1/22/18','2/22/18');


	SELECT * FROM tbl_book_loans

	INSERT INTO tbl_book_copies
		(book_copies_BookID, book_copies_BranchID, book_copies_No_Of_Copies)
		VALUES
		('1','1','5'),
		('2','1','5'),
		('3','1','5'),
		('4','1','5'),
		('5','1','5'),
		('6','1','5'),
		('7','1','5'),
		('8','1','5'),
		('9','1','5'),
		('10','1','5'),
		('11','1','5'),
		('12','1','5'),
		('13','1','5'),
		('14','1','5'),
		('15','1','5'),
		('16','1','5'),
		('17','1','5'),
		('18','1','5'),
		('19','1','5'),
		('20','1','5'),
		('1','2','5'),
		('2','2','5'),
		('3','2','5'),
		('4','2','5'),
		('5','2','5'),
		('6','2','5'),
		('7','2','5'),
		('8','2','5'),
		('9','2','5'),
		('10','2','5'),
		('11','2','5'),
		('12','2','5'),
		('13','2','5'),
		('14','2','5'),
		('15','2','5'),
		('16','2','5'),
		('17','2','5'),
		('18','2','5'),
		('19','2','5'),
		('20','2','5'),
		('1','3','5'),
		('2','3','5'),
		('3','3','5'),
		('4','3','5'),
		('5','3','5'),
		('6','3','5'),
		('7','3','5'),
		('8','3','5'),
		('9','3','5'),
		('10','3','5'),
		('11','3','5'),
		('12','3','5'),
		('13','3','5'),
		('14','3','5'),
		('15','3','5'),
		('16','3','5'),
		('17','3','5'),
		('18','3','5'),
		('19','3','5'),
		('20','3','5'),
		('1','4','5'),
		('2','4','5'),
		('3','4','5'),
		('4','4','5'),
		('5','4','5'),
		('6','4','5'),
		('7','4','5'),
		('8','4','5'),
		('9','4','5'),
		('10','4','5'),
		('11','4','5'),
		('12','4','5'),
		('13','4','5'),
		('14','4','5'),
		('15','4','5'),
		('16','4','5'),
		('17','4','5'),
		('18','4','5'),
		('19','4','5'),
		('20','4','5');

	SELECT * FROM tbl_book_copies


	INSERT INTO tbl_book_authors
		(book_authors_BookID,book_authors_AuthorName)
		VALUES
		('1','Patrick Rothfuss'),
		('2','Stephen King'),
		('3','Stephen King'),
		('4','Frank Herbert'),
		('5','J.R.R. Tolkien'),
		('6','Christopher Paolini'),
		('6','Patrick Rothfuss'),
		('8','J.K. Rowling'),
		('9','Haruki Murakami'),
		('10','Shel Silverstein'),
		('11','Douglas Adams'),
		('12','Aldous Huxley'),
		('13','William Goldman'),
		('14','Chuck Palahniuk'),
		('15','Louis Sachar'),
		('16','J.K. Rowling'),
		('17','J.K. Rowling'),
		('18','J.R.R. Tolkien'),
		('19','George R.R. Martin'),
		('20','Mark Lee');

	SELECT * FROM tbl_book_authors
END
	/*============================== END POPULATING TABLES ==============================*/

/* =================== STORED PROCEDURE QUERY QUESTIONS =================================== */

/* #1- How many copies of the book titled "The Lost Tribe" are owned by the library branch whose name is "Sharpstown"? */

CREATE PROC dbo.bookCopiesAtAllSharpstown 
(@bookTitle varchar(70) = 'The Lost Tribe', @branchName varchar(70) = 'Sharpstown')
AS
SELECT copies.book_copies_BranchID AS [Branch ID], branch.library_branch_BranchName AS [Branch Name],
	   copies.book_copies_No_Of_Copies AS [Number of Copies],
	   book.book_Title AS [Book Title]
	   FROM tbl_book_copies AS copies
			INNER JOIN tbl_book AS book ON copies.book_copies_BookID = book.book_BookID
			INNER JOIN tbl_library_branch AS branch ON book_copies_BranchID = branch.library_branch_BranchID
	   WHERE book.book_Title = @bookTitle AND branch.library_branch_BranchName = @branchName
GO
EXEC dbo.bookCopiesAtAllSharpstown 


/* #2- How many copies of the book titled "The Lost Tribe" are owned by each library branch? */

CREATE PROC dbo.bookCopiesAtAllBranches 
(@bookTitle varchar(70) = 'The Lost Tribe')
AS
SELECT copies.book_copies_BranchID AS [Branch ID], branch.library_branch_BranchName AS [Branch Name],
	   copies.book_copies_No_Of_Copies AS [Number of Copies],
	   book.book_Title AS [Book Title]
	   FROM tbl_book_copies AS copies
			INNER JOIN tbl_book AS book ON copies.book_copies_BookID = book.book_BookID
			INNER JOIN tbl_library_branch AS branch ON book_copies_BranchID = branch.library_branch_BranchID
	   WHERE book.book_Title = @bookTitle 
GO
EXEC dbo.bookCopiesAtAllBranches


/* #3- Retrieve the names of all borrowers who do not have any books checked out. */

CREATE PROC dbo.NoLoans
AS
SELECT borrower_BorrowerName FROM tbl_borrower
	WHERE NOT EXISTS
		(SELECT * FROM tbl_book_loans
		WHERE book_loans_CardNo = borrower_CardNo)
GO
EXEC dbo.NoLoans

/* #4- For each book that is loaned out from the "Sharpstown" branch and whose DueDate is today, retrieve the book title, the borrower's name, and the borrower's address.  */

CREATE PROC dbo.LoanersInfo 
(@DueDate date = NULL, @LibraryBranchName varchar(50) = 'Sharpstown')
AS
SET @DueDate = GETDATE()
SELECT Branch.library_branch_BranchName AS [Branch Name],  Book.book_Title [Book Name],
	   Borrower.borrower_BorrowerName AS [Borrower Name], Borrower.borrower_BorrowerAddress AS [Borrower Address],
	   Loans.book_loans_DateOut AS [Date Out], Loans.book_loans_DueDate [Due Date]
	   FROM tbl_book_loans AS Loans
			INNER JOIN tbl_book AS Book ON Loans.book_loans_BookID = Book.book_BookID
			INNER JOIN tbl_borrower AS Borrower ON Loans.book_loans_CardNo = Borrower.borrower_CardNo
			INNER JOIN tbl_library_branch AS Branch ON Loans.book_loans_BranchID = Branch.library_branch_BranchID
		WHERE Loans.book_loans_DueDate = @DueDate AND Branch.library_branch_BranchName = @LibraryBranchName
GO
EXEC dbo.LoanersInfo 

/* #5- For each library branch, retrieve the branch name and the total number of books loaned out from that branch.  */

CREATE PROC dbo.TotalLoansPerBranch
AS
SELECT  Branch.library_branch_BranchName AS [Branch Name], COUNT (Loans.book_loans_BranchID) AS [Total Loans]
		FROM tbl_book_loans AS Loans
			INNER JOIN tbl_library_branch AS Branch ON Loans.book_loans_BranchID = Branch.library_branch_BranchID
			GROUP BY library_branch_BranchName
GO
EXEC dbo.TotalLoansPerBranch

/* #6- Retrieve the names, addresses, and number of books checked out for all borrowers who have more than five books checked out. */

CREATE PROC dbo.BooksLoanedOut
(@BooksCheckedOut INT = 5)
AS
	SELECT Borrower.borrower_BorrowerName AS [Borrower Name], Borrower.borrower_BorrowerAddress AS [Borrower Address],
		   COUNT(Borrower.borrower_BorrowerName) AS [Books Checked Out]
		   FROM tbl_book_loans AS Loans
		   			INNER JOIN tbl_borrower AS Borrower ON Loans.book_loans_CardNo = Borrower.borrower_CardNo
					GROUP BY Borrower.borrower_BorrowerName, Borrower.borrower_BorrowerAddress
		   HAVING COUNT(Borrower.borrower_BorrowerName) >= @BooksCheckedOut
GO
EXEC dbo.BooksLoanedOut



/* #7- For each book authored by "Stephen King", retrieve the title and the number of copies owned by the library branch whose name is "Central".*/

CREATE PROC dbo.BookbyAuthorandBranch
	(@BranchName varchar(50) = 'Central', @AuthorName varchar(50) = 'Stephen King')
AS
	SELECT Branch.library_branch_BranchName AS [Branch Name], Book.book_Title AS [Title], Copies.book_copies_No_Of_Copies AS [Number of Copies]
		   FROM tbl_book_authors AS Authors
				INNER JOIN tbl_book AS Book ON Authors.book_authors_BookID = Book.book_BookID
				INNER JOIN tbl_book_copies AS Copies ON Authors.book_authors_BookID = Copies.book_copies_BookID
				INNER JOIN tbl_library_branch AS Branch ON Copies.book_copies_BranchID = Branch.library_branch_BranchID
			WHERE Branch.library_branch_BranchName = @BranchName AND Authors.book_authors_AuthorName = @AuthorName
GO	
EXEC dbo.BookbyAuthorandBranch

/* ==================================== STORED PROCEDURE QUERY QUESTIONS =================================== */
2. Inventory Control Management
--Create the inventory table
CREATE TABLE inventory (
  item_id NUMBER,
  item_name VARCHAR2(50),
  item_description VARCHAR2(200),
  item_quantity NUMBER,
  item_price NUMBER(10,2),
  CONSTRAINT inventory_pk PRIMARY KEY (item_id)
);

--Procedure to add new items to inventory
CREATE OR REPLACE PROCEDURE add_item (
  p_item_name IN VARCHAR2,
  p_item_description IN VARCHAR2,
  p_item_quantity IN NUMBER,
  p_item_price IN NUMBER
)
AS
  v_item_id NUMBER;
BEGIN
  SELECT MAX(item_id) + 1 INTO v_item_id FROM inventory;
  IF v_item_id IS NULL THEN
    v_item_id := 1;
  END IF;
  INSERT INTO inventory (item_id, item_name, item_description, item_quantity, item_price)
  VALUES (v_item_id, p_item_name, p_item_description, p_item_quantity, p_item_price);
  COMMIT;
  DBMS_OUTPUT.PUT_LINE('Item added successfully');
EXCEPTION
  WHEN OTHERS THEN
    ROLLBACK;
    DBMS_OUTPUT.PUT_LINE('Error adding item: ' || SQLERRM);
END;

--Procedure to update item quantity
CREATE OR REPLACE PROCEDURE update_item_quantity (
  p_item_id IN NUMBER,
  p_item_quantity IN NUMBER
)
AS
BEGIN
  UPDATE inventory SET item_quantity = p_item_quantity WHERE item_id = p_item_id;
  COMMIT;
  DBMS_OUTPUT.PUT_LINE('Item quantity updated successfully');
EXCEPTION
  WHEN NO_DATA_FOUND THEN
    DBMS_OUTPUT.PUT_LINE('Item not found');
  WHEN OTHERS THEN
    ROLLBACK;
    DBMS_OUTPUT.PUT_LINE('Error updating item quantity: ' || SQLERRM);
END;

--Procedure to update item price
CREATE OR REPLACE PROCEDURE update_item_price (
  p_item_id IN NUMBER,
  p_item_price IN NUMBER
)
AS
BEGIN
  UPDATE inventory SET item_price = p_item_price WHERE item_id = p_item_id;
  COMMIT;
  DBMS_OUTPUT.PUT_LINE('Item price updated successfully');
EXCEPTION
  WHEN NO_DATA_FOUND THEN
    DBMS_OUTPUT.PUT_LINE('Item not found');
  WHEN OTHERS THEN
    ROLLBACK;
    DBMS_OUTPUT.PUT_LINE('Error updating item price: ' || SQLERRM);
END;

--Procedure to delete an item from inventory
CREATE OR REPLACE PROCEDURE delete_item (
  p_item_id IN NUMBER
)
AS
BEGIN
  DELETE FROM inventory WHERE item_id = p_item_id;
  COMMIT;
  DBMS_OUTPUT.PUT_LINE('Item deleted successfully');
EXCEPTION
  WHEN NO_DATA_FOUND THEN
    DBMS_OUTPUT.PUT_LINE('Item not found');
  WHEN OTHERS THEN
    ROLLBACK;
    DBMS_OUTPUT.PUT_LINE('Error deleting item: ' || SQLERRM);
END;

--Function to get total inventory value
CREATE OR REPLACE FUNCTION total_inventory_value
RETURN NUMBER
AS
  v_total NUMBER := 0;
BEGIN
  SELECT SUM(item_quantity * item_price) INTO v_total FROM inventory;
  RETURN v_total;
END;
3. Voice-Based Transport Enquiry System
use dbms;
create table UserTable(
ID serial,
Email varchar(40) not null unique,
Name varchar(30),
Password varchar(15)not null,
Usertype varchar(10) not null,
primary key (ID)
);
drop table UserTable;
create table NonAdmin(
ID serial,
Gender varchar(7),
Phone numeric(10,0) not null unique constraint Phone1_chk check(Phone>999999999),
Address varchar(100),
primary key(ID),
foreign key(ID) references UserTable(ID) on delete cascade 
);

create table Admin(
ID serial,
AgencyName  varchar(35) not null,
AgencyPhone varchar(10)not null unique constraint Phone2_chk check(AgencyPhone>999999999),
AgencyOffice varchar(50),
primary key(AgencyName),
foreign key(ID) references UserTable(ID) on delete cascade
);

create table BusInfo(
BusRegnNo varchar(15) not null,
AgencyName varchar(35) not null,
TotalSeats integer default 40,
AC numeric(1) default 0,
LocationName varchar(20),
Latitude numeric(17,10),
Longitude numeric(17,10),
primary key(BusRegnNo),
foreign key(AgencyName) references Admin(AgencyName) on delete cascade
);

create table AgencyDetails(
AgencyName varchar(35) not null,
AgencyAddress varchar(50) not null
);

alter table BusSchedule drop index DriverID;

create table BusSchedule(
BusRegnNo varchar(15) not null,
RouteID integer not null constraint Routeid1_chk check(RouteID>0),
DriverID integer unique constraint Driverid1_chk check(DriverID>0),
StartTime numeric(4,2) constraint Start_chk1 check(StartTime>=0 and StartTime<2400),
Fare integer constraint fair_chk check(Fare>0),
ReservedSeats integer default 0,
TravelTime numeric(10) constraint EstTim1_chk check(Traveltime>0),
primary key(RouteID,DriverID,StartTime),
foreign key(BusRegnNo) references BusInfo(BusRegnNo) on delete cascade
);

create table TimeForTravel(
TravelTime numeric(10) constraint EstTim2_chk check(Traveltime>0),
StartTime numeric(4,2) constraint Start1_check check(StartTime>=0 and StartTime<24),
EndTime numeric(4,2) constraint End1_chk check(EndTime>=0 and EndTime<24),
primary key(TravelTime,StartTime)
);

create table RouteDetails(
RouteID integer constraint Routeid4_chk check(RouteID>0),
RouteName varchar(30) not null,
Source varchar(30) not null,
Destination varchar(30) not null
);

create table BusStops(
RouteID integer not null constraint Routeid_chk check(RouteID>0),
IntermediateStops varchar(20) not null,
StopNumber integer not null constraint Stop_num check(StopNumber>0)
);

create table DriverDetails(
DriverID serial,
DriverName varchar(20) not null,
DriverPhone numeric(10) constraint Phone3_chk check(DriverPhone>999999999),
Age numeric(3) constraint check(Age>0),
Date_Of_Join date,
primary key(DriverID)
);
drop table DriverDetails;
create table Ticket(
BusRegnNo varchar(15) not null,
TicketPNR serial,
BookingDate date,
TravelDate date, 
primary key(TicketPNR),
constraint dat_chk check(TravelDate>BookingDate+2),
foreign key(BusRegnNo) references BusInfo(BusRegnNo)
);

create table SeatsBooked(
TicketPNR integer,
BookedSeats integer,
primary key(TicketPNR,BookedSeats)
);
drop table SeatsBooked;
create table SeatInfo(
BusRegnNo varchar(15) not null unique, 
SeatNo integer constraint SeatNo_chk check(SeatNo>40),
Sleeper numeric(1) default 0,
foreign key(BusRegnNo) references BusInfo(BusRegnNo) on delete cascade
);

create table Passenger(
ID serial,
BusRegnNo varchar(15) not null,
PassengerID integer constraint passid_chk check(PassengerID>0),
PassengerName varchar(20),
PassengeGender varchar(7),
Age integer constraint age2_chk check(age>5),
primary key(PassengerID),
foreign key(ID) references NonAdmin(ID) on delete cascade,
foreign key(BusRegnNo) references BusInfo(BusRegnNo)
);
drop table Through;
create table Through(
RouteID integer,
DriverID integer,
StartTime numeric(4,2),
BusRegnNo integer not null,
TicketPNR integer not null constraint pnr4_chk check(TicketPNR>0),
primary key(RouteID,DriverID,StartTime,TicketPNR)
);

create assertion AT_LEAST_ONE as CHECK(
not exists
(select count(*) as TRIPS from through group by BusRegnNo where TRIPS=0)
);
alter table RouteDetails add constraint check(AproxDistance>0);
insert into DriverDetails values('126', 'Prithvi', '9834534565','29','2017-09-22');
insert into BusStops values('4','Chennai','7');

delete from TimeForTravel;
insert into RouteDetails values('4','BGL-CNI','Bangalore','Chennai','345');
insert into RouteDetails values('3','CNI-BGL','Chennai','Bangalore','345');

insert into RouteDetails values('2','BGL-TRR','Bangalore','Tirupur','322');
insert into TimeForTravel values('12','17.30','05.30');

alter table Through modify column BusRegnNo varchar(50); 
insert into TimeForTravel values('15','19.00','10.00');
insert into TimeForTravel values('14','19.45','9.45');
insert into TimeForTravel values('15','19.15','10.15');
insert into TimeForTravel values('17','19.00','12.00');
insert into TimeForTravel values('15','20.00','11.00');
insert into TimeForTravel values('1','1.00','2.00');
drop table Ticket;
insert into SeatsBooked values('1006','25');
insert into SeatsBooked values('1006','26');
insert into SeatsBooked values('1006','27');
insert into SeatsBooked values('1007','20');
insert into SeatsBooked values('1007','21');
insert into SeatsBooked values('1008','25');
insert into SeatsBooked values('1009','14');
insert into SeatsBooked values('1009','15');
insert into SeatsBooked values('1009','16');
insert into SeatsBooked values('1009','17');
insert into SeatsBooked values('1010','1');
insert into SeatsBooked values('1011','10');
insert into SeatsBooked values('1011','12');
insert into SeatsBooked values('1011','19');

alter table TimeForTravel modify column StartTime numeric(4,2); 
insert into TimeForTravel values('15','19.00','10.00');
insert into TimeForTravel values('14','19.45','9.45');
insert into TimeForTravel values('15','19.15','10.15');
insert into TimeForTravel values('17','19.00','12.00');
insert into TimeForTravel values('15','20.00','11.00');
insert into TimeForTravel values('1','1.00','2.00');


insert into Through values('1', '121', '17.30', '0', '1003');
insert into Through values('1', '122', '19.00', '0', '1004');
insert into Through values('2', '123', '19.45', '0', '1001');
insert into Through values('3', '124', '19.15', '0', '1005');
insert into Through values('4', '125', '1.00', '0', '1002');

delimiter $$
create trigger TimeTravel after insert on BusSchedule
for each row 
begin
if(new.TravelTime+new.StartTime>24.00) then
	insert into TimeForTravel(TravelTime,StartTime,EndTime)
	values(new.TravelTime,new.StartTime,new.TravelTime+new.StartTime-24);
else
	insert into TimeForTravel(TravelTime,StartTime,EndTime)
	values(new.TravelTime,new.StartTime,new.TravelTime+new.StartTime);
end if;
end$$
delimiter ;

create procedure totalrevenue()
begin
select AgencyName,sum(fare) from BusInfo natural join BusSchedule group by AgencyName;
end$$
delimiter ;
call totalrevenue();

show function status where db='dbms';
select AgencyName,sum(fare) from BusInfo natural join BusSchedule group by AgencyName;
4. SMS-based Remote Server Monitoring System
-- Create a table to store server information
CREATE TABLE server_info (
    server_name VARCHAR2(50),
    server_ip VARCHAR2(15),
    status VARCHAR2(10),
    last_checked DATE,
    CONSTRAINT server_info_pk PRIMARY KEY (server_name)
);

-- Procedure to update server status
CREATE OR REPLACE PROCEDURE update_server_status (
    p_server_name IN VARCHAR2,
    p_status IN VARCHAR2
)
AS
BEGIN
    UPDATE server_info
    SET status = p_status, last_checked = SYSDATE
    WHERE server_name = p_server_name;
    COMMIT;
    DBMS_OUTPUT.PUT_LINE(p_server_name || ' status updated to ' || p_status);
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE(p_server_name || ' not found in server_info table');
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Error updating ' || p_server_name || ' status: ' || SQLERRM);
END;

-- Procedure to check server status and send SMS alert if down
CREATE OR REPLACE PROCEDURE check_server_status (
    p_server_name IN VARCHAR2,
    p_server_ip IN VARCHAR2,
    p_phone_number IN VARCHAR2
)
AS
    l_ping_res PLS_INTEGER;
BEGIN
    SELECT DBMS_NETWORK_UTIL.PING(p_server_ip, 1000) INTO l_ping_res FROM dual;
    IF l_ping_res = 0 THEN
        update_server_status(p_server_name, 'DOWN');
        send_sms_alert(p_phone_number, p_server_name);
    ELSE
        update_server_status(p_server_name, 'UP');
    END IF;
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error checking ' || p_server_name || ' status: ' || SQLERRM);
END;

-- Procedure to send SMS alert
CREATE OR REPLACE PROCEDURE send_sms_alert (
    p_phone_number IN VARCHAR2,
    p_server_name IN VARCHAR2
)
AS
    l_sms_message VARCHAR2(100);
BEGIN
    l_sms_message := p_server_name || ' is down. Please check.';
    -- Code to send SMS message goes here
    -- For example, using a third-party SMS gateway API
    -- Or using an SMS modem connected to the server
    DBMS_OUTPUT.PUT_LINE('SMS alert sent to ' || p_phone_number);
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error sending SMS alert for ' || p_server_name || ': ' || SQLERRM);
END;
5. LyFi
-- Create a table to store light switch information
CREATE TABLE light_switches (
    switch_id NUMBER,
    switch_name VARCHAR2(50),
    switch_location VARCHAR2(50),
    switch_status VARCHAR2(10),
    CONSTRAINT light_switches_pk PRIMARY KEY (switch_id)
);

-- Procedure to turn on/off a light switch
CREATE OR REPLACE PROCEDURE turn_light_switch (
    p_switch_id IN NUMBER,
    p_switch_status IN VARCHAR2
)
AS
BEGIN
    UPDATE light_switches
    SET switch_status = p_switch_status
    WHERE switch_id = p_switch_id;
    COMMIT;
    DBMS_OUTPUT.PUT_LINE('Switch ' || p_switch_id || ' is turned ' || p_switch_status);
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Switch ' || p_switch_id || ' not found in light_switches table');
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Error turning switch ' || p_switch_id || ' ' || p_switch_status || ': ' || SQLERRM);
END;

-- Procedure to dim a light switch
CREATE OR REPLACE PROCEDURE dim_light_switch (
    p_switch_id IN NUMBER,
    p_brightness_level IN NUMBER
)
AS
BEGIN
    -- Code to dim light switch goes here
    UPDATE light_switches
    SET switch_status = 'DIMMED'
    WHERE switch_id = p_switch_id;
    COMMIT;
    DBMS_OUTPUT.PUT_LINE('Switch ' || p_switch_id || ' is dimmed to level ' || p_brightness_level);
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Switch ' || p_switch_id || ' not found in light_switches table');
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Error dimming switch ' || p_switch_id || ' to level ' || p_brightness_level || ': ' || SQLERRM);
END;

-- Procedure to discover and configure LyFi devices
CREATE OR REPLACE PROCEDURE configure_lyfi_devices
AS
BEGIN
    -- Code to discover and configure LyFi devices goes here
    DBMS_OUTPUT.PUT_LINE('LyFi devices discovered and configured');
EXCEPTION
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Error configuring LyFi devices: ' || SQLERRM);
END;

-- Procedure to control other appliances
CREATE OR REPLACE PROCEDURE control_appliance (
    p_appliance_name IN VARCHAR2,
    p_appliance_status IN VARCHAR2
)
AS
BEGIN
    -- Code to control other appliances goes here
    DBMS_OUTPUT.PUT_LINE(p_appliance_name || ' is turned ' || p_appliance_status);
EXCEPTION
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Error controlling ' || p_appliance_name || ': ' || SQLERRM);
END;
6. Database Interfacing for LabVIEW Robotic Control
-- Create a table to store robot control parameters
CREATE TABLE robot_parameters (
    parameter_name VARCHAR2(50),
    parameter_value NUMBER,
    CONSTRAINT robot_parameters_pk PRIMARY KEY (parameter_name)
);

-- Procedure to update a robot control parameter
CREATE OR REPLACE PROCEDURE update_robot_parameter (
    p_parameter_name IN VARCHAR2,
    p_parameter_value IN NUMBER
)
AS
BEGIN
    UPDATE robot_parameters
    SET parameter_value = p_parameter_value
    WHERE parameter_name = p_parameter_name;
    COMMIT;
    DBMS_OUTPUT.PUT_LINE('Parameter ' || p_parameter_name || ' updated to ' || p_parameter_value);
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Parameter ' || p_parameter_name || ' not found in robot_parameters table');
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Error updating parameter ' || p_parameter_name || ' to ' || p_parameter_value || ': ' || SQLERRM);
END;

-- Procedure to retrieve a robot control parameter
CREATE OR REPLACE PROCEDURE get_robot_parameter (
    p_parameter_name IN VARCHAR2,
    p_parameter_value OUT NUMBER
)
AS
BEGIN
    SELECT parameter_value
    INTO p_parameter_value
    FROM robot_parameters
    WHERE parameter_name = p_parameter_name;
    DBMS_OUTPUT.PUT_LINE('Parameter ' || p_parameter_name || ' retrieved with value ' || p_parameter_value);
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Parameter ' || p_parameter_name || ' not found in robot_parameters table');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error retrieving parameter ' || p_parameter_name || ': ' || SQLERRM);
END;
