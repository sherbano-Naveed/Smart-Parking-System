================================================================
 University Parking Management System
 Imtiaz Super Mart Peshawar
 README.txt
================================================================

----------------------------------------------------------------
1. PROJECT OVERVIEW
----------------------------------------------------------------
This is a University-level web project called:
"University Parking Management System - Imtiaz Super Mart Peshawar"

It is a simple parking management system developed for
university demonstration purposes. The system is designed
specifically for Imtiaz Super Mart Parking Area in Peshawar.

The system allows:
- Admins to manage parking lots, slots, vehicles, tickets,
  billing, payments, and view driver records.
- Drivers to view their own vehicles, tickets, and payment
  details.

----------------------------------------------------------------
2. TECHNOLOGIES USED
----------------------------------------------------------------
- HTML5      : Page structure and markup
- CSS3       : Styling (style.css)
- JavaScript : Confirm dialogs, auto-hide alerts (script.js)
- PHP        : Server-side logic (no frameworks)
- MySQL      : Database via XAMPP / phpMyAdmin

No frameworks, no Bootstrap, no Laravel, no React used.

----------------------------------------------------------------
3. HOW TO RUN ON XAMPP
----------------------------------------------------------------
Step 1: Install XAMPP from https://www.apachefriends.org/
Step 2: Start Apache and MySQL from the XAMPP Control Panel.
Step 3: Copy the entire "parking-management-system" folder into:
        C:\xampp\htdocs\
Step 4: Open your browser and go to:
        http://localhost/parking-management-system/

----------------------------------------------------------------
4. DATABASE IMPORT STEPS
----------------------------------------------------------------
Step 1: Open your browser and go to:
        http://localhost/phpmyadmin/
Step 2: Click "New" on the left sidebar.
Step 3: Create a database named: parking_management_system
Step 4: Click on that database, then click "Import".
Step 5: Browse and select the file:
        /database/parking_management_system.sql
Step 6: Click "Go" to import all tables and sample data.

DONE! The database is ready.

----------------------------------------------------------------
5. FOLDER STRUCTURE
----------------------------------------------------------------
/parking-management-system
|
|-- index.php           (redirects to login)
|-- login.php           (login page)
|-- logout.php          (session destroy)
|-- db.php              (database connection)
|-- style.css           (main CSS file)
|-- script.js           (JavaScript file)
|-- README.txt          (this file)
|
|-- /admin/
|   |-- dashboard.php       (admin dashboard with stats)
|   |-- vehicles.php        (list & delete vehicles)
|   |-- add_vehicle.php     (add new vehicle)
|   |-- edit_vehicle.php    (edit existing vehicle)
|   |-- parking_lots.php    (full CRUD for parking lots)
|   |-- parking_slots.php   (full CRUD for parking slots)
|   |-- parking_tickets.php (full CRUD for tickets)
|   |-- parking_rates.php   (full CRUD for rates)
|   |-- billing.php         (full CRUD for billing)
|   |-- payments.php        (full CRUD for payments)
|   |-- drivers.php         (view all drivers)
|
|-- /driver/
|   |-- dashboard.php       (driver dashboard with stats)
|   |-- my_vehicle.php      (view own vehicles)
|   |-- my_tickets.php      (view own tickets)
|   |-- my_payments.php     (view own billing/payments)
|
|-- /database/
    |-- parking_management_system.sql   (full SQL file)

----------------------------------------------------------------
6. LOGIN CREDENTIALS
----------------------------------------------------------------
ADMIN LOGIN:
  Email   : admin@parking.com
  Password: admin123
  Role    : Admin

DRIVER LOGIN 1:
  Email   : ali@parking.com
  Password: driver123
  Role    : Driver

DRIVER LOGIN 2:
  Email   : sara@parking.com
  Password: driver123
  Role    : Driver

----------------------------------------------------------------
7. CRUD EXPLANATION
----------------------------------------------------------------
CRUD stands for:
  C - Create  (Add new records using INSERT)
  R - Read    (View records using SELECT)
  U - Update  (Edit records using UPDATE)
  D - Delete  (Remove records using DELETE)

This project implements full CRUD for:
  1. Vehicle
  2. Parking Lot
  3. Parking Slot
  4. Parking Ticket
  5. Parking Rate
  6. Billing
  7. Payment

All CRUD operations use PHP and MySQL with prepared statements.

----------------------------------------------------------------
8. SQL INJECTION EXPLANATION
----------------------------------------------------------------

WHAT IS SQL INJECTION?
-----------------------
SQL Injection is a type of security attack where a malicious
user enters specially crafted text into a form field that gets
inserted directly into a SQL query, allowing them to:
  - Bypass login authentication
  - View all data in the database
  - Delete or modify data
  - Extract sensitive information

HOW SQL INJECTION WORKS:
-------------------------
Imagine a login query written like this (DANGEROUS WAY):

  $sql = "SELECT * FROM User WHERE Email='$email'
          AND Password='$password'";

If a hacker enters this as the email:
  admin@x.com' OR '1'='1

The query becomes:
  SELECT * FROM User WHERE Email='admin@x.com' OR '1'='1'
  AND Password='...'

Since '1'='1' is always true, the hacker bypasses the password
check and logs in as admin without knowing the password!

WHY RAW QUERIES ARE DANGEROUS:
--------------------------------
When user input is placed directly into SQL strings:
  - No filtering is applied.
  - The database cannot tell the difference between
    code (SQL) and data (user input).
  - Attackers can control the logic of the query.

HOW PREPARED STATEMENTS PREVENT SQL INJECTION:
------------------------------------------------
Prepared statements separate the SQL code from the data.
The query structure is sent first, then the data is sent
separately. The database engine treats the data as pure
data only - it can NEVER be interpreted as SQL code.

Example from this project (SAFE WAY):

  $stmt = $conn->prepare(
    "SELECT * FROM User WHERE Email = ? AND Password = MD5(?)"
  );
  $stmt->bind_param("ss", $email, $password);
  $stmt->execute();

How it works:
  1. "?" is a placeholder - it marks where data will go.
  2. bind_param() sends the actual data separately.
  3. The database treats the input as data, not SQL.
  4. Even if the hacker enters: ' OR '1'='1
     It will be searched literally as a string, not as SQL.

WHY THIS PROJECT IS SECURE:
-----------------------------
Every single SQL query in this project uses:
  - $conn->prepare() to create the query template
  - $stmt->bind_param() to attach data safely
  - $stmt->execute() to run the query

No raw SQL queries with variables are used anywhere.
All user input is handled via prepared statements only.
htmlspecialchars() is used when displaying data to prevent
XSS (Cross-Site Scripting) attacks.

----------------------------------------------------------------
9. PREPARED STATEMENTS EXPLANATION
----------------------------------------------------------------
A prepared statement is a feature of database systems that:

1. COMPILES the query first (without any user data).
2. Sends the user data separately, bound to placeholders (?).
3. The database executes the compiled query with the safe data.

Benefits:
  a) Prevents SQL Injection completely.
  b) Better performance for repeated queries.
  c) Cleaner, more organized code.

Syntax used in this project:

  // Step 1: Prepare the query
  $stmt = $conn->prepare("INSERT INTO Table (col) VALUES (?)");

  // Step 2: Bind the parameter (s = string, i = integer)
  $stmt->bind_param("s", $variable);

  // Step 3: Execute
  $stmt->execute();

  // Step 4: Close
  $stmt->close();

Type characters for bind_param:
  s = string
  i = integer
  d = double (decimal)
  b = blob (binary)

================================================================
 Project By: [Your Name] | University: [Your University]
 Course: Database Management System (DBMS)
================================================================
