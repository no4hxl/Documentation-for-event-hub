# 📂 EventHub Technical Documentation & Architecture

EventHub is a professional web-based platform built with PHP and MySQL. This document serves as a guide for developers and a technical overview for project evaluators.

---

## 🏗️ Project Architecture & Design Philosophy
The system follows a **Modular Design**, separating administrative logic from the public-facing portal.

### Folder Structure
* **`admin/`**: Houses all CRUD operations and the restricted dashboard.
* **`includes/`**: Contains core system files like `db.php` (The Database Bridge).
* **`assets/`**: Contains styling (CSS) and static media (Images).
* **`index.php`**: The public landing page and entry point.

---

## 🛠️ Tech Stack Glossary
For those new to the stack, here is the role of each technology:
- **XAMPP**: The local server environment used for development.
- **Apache**: The web server that "serves" the PHP files to the browser.
- **MySQL**: The relational database used to store events and user data.
- **PHP 8.1+**: The server-side language used for business logic and database communication.

---

### Visual Folder Structure

Here is a diagram of the project organization:

```plaintext
event_mgt_system/
│
├── admin/                  # Backend Management Area (Restricted access)
│   ├── create_event.php    # CRUD: Create new events
│   ├── dashboard.php       # Admin Home & Statistics
│   ├── delete_event.php    # CRUD: Delete logic (hidden script)
│   ├── edit_event.php      # CRUD: Update existing events
│   ├── login.php           # Admin authentication gateway
│   ├── register_admin.php  # Sign up for new staff accounts
│   └── view_registrations.php # See who signed up for events
│
├── assets/                 # Frontend Resources
│   ├── css/
│   │   └── style.css       # Main stylesheet
│   └── images/             # Static site images
│
├── includes/               # Shared Resources
│   └── db.php              # Database Connection Bridge (Used everywhere)
│
├── database.sql            # Database Schema import file
├── index.php               # Public Homepage (The "Entry Point")
├── register.php            # Public Registration Page for users
├── README.md               # Project Documentation
└── success.php             # Registration confirmation page

```

## 🧠 Core Logic Breakdown

### 1. Dynamic Search Logic
The search functionality uses the SQL **`LIKE`** operator combined with **`%`** wildcards.
- **Implementation**: `SELECT * FROM events WHERE title LIKE '%keyword%'`
- **Why?**: This allows for "partial matching," meaning if a user searches for "Lagos," the system will find any event containing that word anywhere in its title.



### 2. The CRUD System
The Administrative Suite is built on the **CRUD** (Create, Read, Update, Delete) principle:
- **Create (`create_event.php`)**: Captures form data and uses `INSERT INTO` to add records.
- **Read (`dashboard.php`)**: Fetches statistics and event lists using `SELECT` queries.
- **Update (`edit_event.php`)**: Uses a unique **ID** passed via the URL to target and modify specific rows.
- **Delete (`delete_event.php`)**: A background script that removes records using the `DELETE` command and redirects the admin instantly.

### 3. Security: Admin Approval Workflow
To prevent unauthorized access, we implemented a **Boolean Gate**:
1.  **Status 0**: New admins are created with `is_approved = 0`. They cannot log in.
2.  **Status 1**: An existing admin must manually approve the account via the dashboard.
- **Benefit**: This ensures that even if someone finds the registration link, they cannot access sensitive data without manual verification.

---

## 📄 Key File Functions

| File | Role |
| :--- | :--- |
| `includes/db.php` | Manages the connection between PHP and MySQL. Used in every file. |
| `index.php` | Displays events to the public and handles the search filter. |
| `register.php` | Captures attendee information and links it to an Event ID. |
| `admin/login.php` | Authenticates users and starts a secure **Session**. |
| `admin/dashboard.php` | Central hub for system statistics and admin management. |



---

## 🛡️ Security Implementation
- **XSS Protection**: Every piece of data displayed to the user is wrapped in `htmlspecialchars()` to prevent malicious scripts from running.
- **SQL Injection Prevention**: We used **Prepared Statements** (or sanitization) to ensure user inputs cannot manipulate our SQL queries.
- **Session Security**: `session_start()` is used to protect the `admin/` directory. If no session exists, the user is redirected to the login page.

---

## 🚀 How to Run the Project
1.  Move the folder to `C:\xampp\htdocs\event_mgt_system`.
2.  Start **Apache** and **MySQL** in XAMPP.
3.  Import `database.sql` into **phpMyAdmin**.
4.  Visit `http://localhost/event_mgt_system` in your browser.
