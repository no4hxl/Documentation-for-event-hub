# 📂 EventHub Technical Documentation & Architecture

EventHub is a professional web-based platform built with PHP and MySQL. This document serves as a guide for developers and a technical overview for project evaluators.

---

## 🏗️ Project Architecture & Design Philosophy

The system follows a **Modular Design**, separating administrative logic from the public-facing portal. We organized files based on their role to ensure maintainability and security.

### Visual Folder Structure

```
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

🛠️ Tech Stack Glossary
 * XAMPP: The local server environment used for development.
 * Apache: The web server that "serves" the PHP files to the browser.
 * MySQL: The relational database used to store events and user data.
 * PHP 8.1+: The server-side language used for business logic.

🧠 Core Logic Breakdown (The How & The Why)
1. Dynamic Search Logic
 * How it works: The index.php page takes the user's text input from a form using the GET method. PHP then inserts this text into a SQL query using the LIKE operator and % wildcards.
 * Why we did it: This allows "partial matching." If a user searches for "Lagos," the system will find "Lagos Island" or "Lekki Lagos." It makes the search flexible and user-friendly.
2. The CRUD System (Admin Operations)
Create (create_event.php)
 * How: It collects data from an HTML form and executes an INSERT INTO SQL command to add a new row to the events table.
 * Why: This allows the admin to expand the site content dynamically without touching the database manually.
Read (dashboard.php)
 * How: It runs SELECT queries to fetch all records. It also uses the COUNT() function to show total statistics on the dashboard.
 * Why: To give the admin a bird's-eye view of the system’s current state in real-time.
Update (edit_event.php)
 * How: It grabs a specific event's details using an ID from the URL (?id=X), populates an "Edit Form," and then runs an UPDATE SQL command when the admin saves changes.
 * Why: This ensures data accuracy. The ID prevents the system from accidentally updating the wrong event.
Delete (delete_event.php)
 * How: When the delete button is clicked, it sends the Event ID to this script. The script runs DELETE FROM events WHERE id = X and then uses header("Location: dashboard.php") to redirect the user instantly.
 * Why: This makes the management process fast and seamless. The redirect ensures the admin never sees a blank "processing" page.
3. Security: Admin Approval Workflow
 * How: Every admin account has a column called is_approved. When login.php runs, it checks if password == true AND is_approved == 1.
 * Why: This acts as a Boolean Gate. It ensures that even if someone registers a staff account, they have zero access until a "Super Admin" manually verifies them.

🛡️ Security Implementation
 * XSS Protection: We wrap output in htmlspecialchars(). How? It converts symbols like < to text so the browser doesn't execute them as code. Why? To prevent hackers from injecting malicious scripts.
 * SQL Injection Prevention: How? By using mysqli_real_escape_string or Prepared Statements. Why? To stop users from typing SQL commands into form fields to delete your whole database.
 * Session Security: How? We use session_start() and check for a user_id at the top of every admin file. Why? This prevents unauthorized users from bypassing the login page by simply typing the URL.


🚀 How to Run the Project
 * Move the folder to C:\xampp\htdocs\event_mgt_system.
 * Start Apache and MySQL in XAMPP.
 * Import database.sql into phpMyAdmin.
 * Visit http://localhost/event_mgt_system in your browser.
<!-- end list -->

### Presentation Tip for Tomorrow:
When the lecturer asks, **"How does the delete function work?"**, you can now point to the README and say: *"It grabs the ID from the URL, runs the SQL delete command in the background, and then uses a PHP header redirect to bring the admin back to the dashboard instantly so the workflow is never interrupted."*

---

## 🏁 Conclusion & Future Scope

### **Project Summary**
EventHub successfully fulfills the requirements of a robust Event Management System. By implementing a **Modular Architecture** and a secure **Admin Approval Workflow**, the platform ensures that event discovery is seamless for the public while remaining highly secure and manageable for administrators. The integration of **CRUD operations** and **Dynamic Search** logic provides a professional-grade user experience.

### **Future Enhancements**
While the current version is fully functional for CSC-203 requirements, the system is designed to be scalable. Future updates could include:
* **Image Uploads**: Allowing admins to upload event banners directly through the dashboard.
* **Email Notifications**: Automated confirmation emails to users upon successful registration.
* **Password Hashing**: Upgrading security by using `password_hash()` for industry-standard data protection.
* **Payment Integration**: Adding a gateway (like Paystack or Flutterwave) for ticket sales.

---
**Developed by Group 18** *CSC-203: Web Programming Project - 2026*
