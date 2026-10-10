# VAULTX — Cloud-Based File Storage System

VAULTX is a file-storage dashboard project built with **HTML, CSS, and JavaScript** on the frontend and **Python FastAPI** on the backend. It uses **Azure SQL Database** to store file metadata and provides a dashboard for uploading, searching, downloading, and managing files.

> **Project note:** In the current implementation, uploaded file contents are saved in the backend's local `uploads/` directory; Azure SQL stores their metadata. This is not yet direct Azure Blob Storage. For production use, configure cloud object storage and tighten security settings.

## Features

- Upload files using the dashboard, including drag-and-drop support.
- Store file metadata in Azure SQL Database.
- Browse and search uploaded files.
- Filter files by type.
- Download and delete files.
- View storage statistics and recent activity.
- FastAPI backend with interactive API documentation.
- Dark neon-style responsive dashboard interface.
- File upload limit: **50 MB per file** in the current backend.

## Technology Stack

- **Frontend:** HTML5, CSS3, JavaScript
- **Backend:** Python, FastAPI, Uvicorn
- **Database:** Azure SQL Database
- **Database driver:** `pyodbc` with Microsoft ODBC Driver 18 for SQL Server
- **Configuration:** `python-dotenv`

## Project Structure

```text
VAULTX/
├── index.html          # Main dashboard page
├── style.css           # Dashboard styling
├── app.js              # Frontend logic and API requests
├── backend.py          # FastAPI API and database operations
├── requirements.txt    # Python dependencies (create as described below)
├── .env.example        # Example environment configuration only
├── .gitignore          # Files and secrets excluded from Git
└── uploads/            # Uploaded files (keep out of Git)
```

## Prerequisites

Install the following before running the project:

- Python 3.10 or later
- Git
- Microsoft ODBC Driver 18 for SQL Server
- An Azure SQL Database, with server firewall access configured for your development machine

## Setup and Run Locally

### 1. Clone the repository

```bash
git clone https://github.com/Anushka-code1/VAULTX.git
cd VAULTX


### 2. Create and activate a virtual environment

**Windows PowerShell**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

If PowerShell blocks activation, you can use Command Prompt instead:

```bat
.venv\Scripts\activate.bat
```

**macOS/Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

Create a `requirements.txt` file with:

```text
fastapi
uvicorn[standard]
pyodbc
python-dotenv
python-multipart
```

Then install them:

```bash
pip install -r requirements.txt
```

You must also install **Microsoft ODBC Driver 18 for SQL Server** separately; installing `pyodbc` alone does not install the ODBC driver.

### 4. Configure environment variables

Create a local `.env` file in the project root. Use `.env.example` as a template:

```env
AZURE_SQL_SERVER=your-server.database.windows.net
AZURE_SQL_DATABASE=your-database-name
AZURE_SQL_USERNAME=your-database-username
AZURE_SQL_PASSWORD=your-database-password
```

Use valid credentials for your own Azure SQL Database. Never commit `.env` or paste database credentials into source code, screenshots, issues, or public repositories.

### 5. Start the backend

From the directory containing `backend.py`, run:

```bash
uvicorn backend:app --reload --port 8000
```

The API will be available at:

- API base URL: `http://127.0.0.1:8000`
- Interactive API docs: `http://127.0.0.1:8000/docs`
- Health endpoint: `http://127.0.0.1:8000/health`

The backend attempts to create the required database tables when it starts. Ensure the Azure SQL server, database, credentials, firewall rules, and ODBC driver are configured correctly.

### 6. Open the frontend

Open `index.html` in your browser, or use the **Live Server** extension in VS Code. The frontend currently sends requests to `http://localhost:8000`, so keep the backend running at that address while using the dashboard.

## API Endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| `GET` | `/health` | Check API/database connectivity |
| `GET` | `/files` | List files; supports search and type filters |
| `POST` | `/files/upload` | Upload a file with optional description and tags |
| `GET` | `/files/{file_id}` | Retrieve a file's metadata |
| `GET` | `/files/{file_id}/download` | Download a file |
| `DELETE` | `/files/{file_id}` | Delete a file record/file according to backend logic |
| `GET` | `/stats` | Retrieve dashboard statistics |
| `GET` | `/activity` | Retrieve recent activity |

## GitHub Safety Checklist

Before pushing the project to GitHub:

1. **Remove `.env` from the repository** and use `.env.example` with placeholder values only.
2. If real credentials have ever been committed or shared, rotate the Azure SQL password and review database access.
3. Do not upload `venv/`, `.venv/`, `uploads/`, Python cache files, or local logs.
4. Review the backend before public deployment. In particular, restrict CORS, add authentication/authorization, validate file names and file types, and avoid exposing detailed database errors.
5. Use a cloud object-storage service such as Azure Blob Storage if uploaded files need durable cloud storage.

Suggested `.gitignore`:

```gitignore
.env
.venv/
venv/
__pycache__/
*.py[cod]
uploads/
*.log
.vscode/
```

## Troubleshooting

- **Database connection fails:** Check the `.env` values, Azure SQL firewall/network rules, database permissions, and installed ODBC Driver 18.
- **`pyodbc` driver error:** Install Microsoft ODBC Driver 18 for SQL Server for your operating system.
- **Frontend cannot connect:** Confirm the backend is running on port `8000` and check the browser console for errors.
- **Upload fails:** Confirm the file is under 50 MB and that the backend can write to the `uploads/` directory.
- **API docs do not open:** Visit `http://127.0.0.1:8000/docs` after starting Uvicorn.

## Future Enhancements

- Integrate Azure Blob Storage for file content.
- Add secure user registration, login, and role-based access control.
- Add encryption, audit controls, and configurable file-size/type policies.
- Add automated tests and CI/CD deployment.
- Deploy the frontend and backend using suitable hosting services.

## Author

**Your Name**  
MCA Project — VAULTX: Cloud-Based File Storage System

---

If you find this project useful, feel free to ⭐ the repository.
