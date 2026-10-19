CivicConnect — Public Service Request System
A Flask + SQLite web application for citizens to report community issues and track progress, while administrators manage reports and view basic service analytics.
Features
Citizen registration and login with hashed passwords
Role-based citizen and administrator access
Submit reports with category, location, description, and optional image
Automatically generated reference numbers
Citizen request list and individual request tracking
Admin dashboard with total, pending, in-progress, and resolved counts
Search and filter requests
Status history/timeline
Responsive layout for desktop and mobile
Input validation and image extension/size limits
Requirements
Python 3.10 or later
pip
Run locally (Windows)
Extract the ZIP.
Open Command Prompt or PowerShell in the extracted public_service_request_system folder.
Create a virtual environment:
py -m venv .venv
.\.venv\Scripts\Activate.ps1
If PowerShell blocks activation, use Command Prompt and run: .\.venv\Scripts\activate.bat
Install dependencies:
python -m pip install -r requirements.txt
Set a secret key for local development (PowerShell):
$env:SECRET_KEY = "replace-this-with-a-long-random-secret"
Start the app:
python app.py
Open http://127.0.0.1:5000 in your browser.
The SQLite database is created automatically on first run.
Create an administrator account
For safety, public registration only creates citizen accounts. To promote your own account:
Register a normal account through the website.
Stop the app with Ctrl+C.
Run the following command from the project folder, replacing your-email@example.com with your registered email:
python -c "import sqlite3; c=sqlite3.connect('service_requests.db'); c.execute(\"UPDATE users SET role='admin' WHERE email=?\", ('your-email@example.com',)); c.commit(); c.close(); print('Admin role updated')"
Restart the app and log in again. The Admin dashboard link will appear.
Main routes
Route
/
/register
/login
/requests/new
/my-requests
/requests/<id>
/admin
Purpose
Public home and aggregate statistics
Citizen registration
Login
Submit a request
View your requests
Request details and status timeline
Admin dashboard and filters
Project structure
public_service_request_system/
├── app.py
├── requirements.txt
├── README.md
├── .gitignore
├── static/
│   ├── style.css
│   └── uploads/
└── templates/
    ├── base.html
    ├── index.html
    ├── register.html
    ├── login.html
    ├── new_request.html
    ├── my_requests.html
    ├── request_detail.html
    ├── admin.html
    └── error.html
Database tables
users: accounts, password hashes, roles
requests: request details and current status
status_history: chronological status updates and notes
Notes before production deployment
This is a learning/demo project. Before public production use, add CSRF protection, rate limiting, stronger upload content validation, a production WSGI server, HTTPS, a persistent upload store, backups, and a managed database. Set SECRET_KEY using the deployment platform's environment variables. Do not use Flask's debug server in production.
Suggested deployment
A Python-compatible hosting platform can run this Flask app. Configure the start command as:
gunicorn app:app
Add gunicorn to requirements.txt before using that command. Make sure the host provides persistent storage for SQLite and uploads; otherwise use a managed database and object storage. Follow the selected host's current setup guide.
