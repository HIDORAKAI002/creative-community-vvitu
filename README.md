# Creative Community @ VVITU

Creative Community is a Flask and vanilla JavaScript web platform for a student-led technology, gaming, design, and creative coding club at VVITU. It provides a public landing page, member signup and login, profile management, resources, attendance tools, and an admin dashboard.

The project is designed as a full-stack website:

- Frontend: HTML templates, CSS, and vanilla JavaScript
- Backend: Python Flask API
- Database: MySQL or MariaDB
- Runtime config: Environment variables through `.env`

## Features

- Public home page for the Creative Community club
- Member registration and login
- Member profile page with profile image upload support
- Admin dashboard for managing members and attendance
- Attendance session opening, check-in, reporting, and remarks
- Resource vault for links, tools, and GitHub repositories
- GitHub repository metadata lookup for resource cards
- Server-side MySQL persistence
- Local upload folder for profile pictures
- Basic device and IP blocking workflow for admin-controlled abuse prevention

## Project Structure

```text
.
+-- app.py
+-- requirements.txt
+-- .env.example
+-- .gitignore
+-- templates/
|   +-- index.html
|   +-- login.html
|   +-- signup.html
|   +-- profile.html
|   +-- resources.html
|   +-- admin.html
+-- static/
    +-- css/
    +-- js/
    +-- data/
    +-- images/
    +-- userpfp/
```

## What Is Not Included

This public upload package intentionally excludes private or local-only files:

- `.env`
- Local database credentials
- Session files
- Member and attendance JSON state files
- Uploaded user profile pictures
- Private helper scripts
- Private cursor effect files
- Personal gallery images

Use `.env.example` as the template for creating your own private `.env` file.

## Requirements

- Python 3.10 or newer
- MySQL or MariaDB
- `pip`
- A terminal or command prompt

## Local Setup

### 1. Clone the repository

```bash
git clone https://github.com/your-username/creative-community.git
cd creative-community
```

### 2. Create a virtual environment

Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

macOS or Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Create your environment file

Copy the example file:

Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

macOS or Linux:

```bash
cp .env.example .env
```

Edit `.env` and fill in your own values:

```env
DB_HOST=your_database_host
DB_PORT=3306
DB_USER=your_database_user
DB_PASS=your_database_password
DB_NAME=your_database_name

ADMIN_ID=your_admin_id
ADMIN_NAME="Your Admin Name"
ADMIN_ROLLNO=your_admin_roll_number
ADMIN_EMAIL=your_admin_email@example.edu
ADMIN_PERSEMAIL=admin_personal@example.com
ADMIN_PHONE=0000000000
ADMIN_PASS=your_secure_admin_password

FLASK_PORT=10018
```

Do not commit `.env` to GitHub.

### 5. Create the database

Log into MySQL or MariaDB and create the database named in `DB_NAME`:

```sql
CREATE DATABASE creative_community_db;
```

If you use a different database name, update `DB_NAME` in `.env`.

The app creates and updates its required tables during startup through `init_db()`.

### 6. Run the app

```bash
python app.py
```

Open:

```text
http://127.0.0.1:10018/creative-community/
```

If you changed `FLASK_PORT`, use that port instead.

## Important Routes

Public pages:

- `/creative-community/`
- `/creative-community/login`
- `/creative-community/signup`
- `/creative-community/profile`
- `/creative-community/resources_page`

Admin page:

- `/creative-community/admin`

Main API routes:

- `GET /creative-community/api/members`
- `POST /creative-community/api/members/register`
- `POST /creative-community/api/login`
- `GET /creative-community/api/resources`
- `POST /creative-community/api/resources/add`
- `POST /creative-community/open_attendance`
- `POST /creative-community/checkin`
- `GET /creative-community/admin/attendance_report`

## Frontend Configuration

The public server status shown on the home page is configured in:

```text
static/js/main.js
```

Find:

```js
const serverIP = 'your_public_server_ip_or_domain';
```

Replace it with the public server domain or IP you want to show.

## Backend Configuration

Backend configuration is loaded from `.env` using `python-dotenv`.

Database settings:

- `DB_HOST`
- `DB_PORT`
- `DB_USER`
- `DB_PASS`
- `DB_NAME`

Admin bootstrap settings:

- `ADMIN_ID`
- `ADMIN_NAME`
- `ADMIN_ROLLNO`
- `ADMIN_EMAIL`
- `ADMIN_PERSEMAIL`
- `ADMIN_PHONE`
- `ADMIN_PASS`

App settings:

- `FLASK_PORT`

## Deployment Notes

For production, run the app behind a production WSGI server and reverse proxy.

Example with Gunicorn:

```bash
gunicorn -w 2 -b 0.0.0.0:10018 app:app
```

If deploying behind Cloudflare or another reverse proxy, confirm the proxy headers used by `get_client_ip()` match your deployment. The app checks `CF-Connecting-IP` and `X-Forwarded-For` before falling back to `request.remote_addr`.

## Security Notes

- Keep `.env` private.
- Rotate credentials immediately if they are ever committed.
- Use strong database and admin passwords.
- The client-side checks are only a deterrent. Real authorization must be enforced on the backend.
- Do not rely on browser-side admin checks as the only protection for sensitive actions.
- Review the admin and security routes before using this in a production environment.

## GitHub Upload Checklist

Before pushing:

```bash
python -m py_compile app.py
```

Check that private local files are not present:

```text
.env
local session state
local member data
local attendance data
private cursor effect files
real user profile uploads
```

Check that `.env.example` contains placeholders only.

## License

Add your preferred license before publishing if you want others to reuse or contribute to the project.
