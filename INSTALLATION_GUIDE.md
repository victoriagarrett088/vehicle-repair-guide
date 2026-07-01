# Complete Installation Guide

## System Requirements

- **OS**: Windows, macOS, or Linux
- **Python**: 3.7 or higher
- **pip**: Package manager for Python
- **Disk Space**: ~500MB (including virtual environment)

## Step-by-Step Installation

### 1. Install Python (if not already installed)

**Windows:**
- Download from https://www.python.org/downloads/
- Run installer and check "Add Python to PATH"
- Verify: `python --version`

**macOS:**
```bash
brew install python3
python3 --version
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt-get update
sudo apt-get install python3 python3-venv python3-pip
python3 --version
```

### 2. Clone the Repository

```bash
git clone https://github.com/victoriagarrett088/vehicle-repair-guide.git
cd vehicle-repair-guide
```

### 3. Create Virtual Environment

**Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

**macOS/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

You should see `(venv)` in your terminal prompt when activated.

### 4. Install Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

This installs:
- Flask 2.3.3
- Flask-SQLAlchemy 3.0.5
- Flask-Login 0.6.2
- pdfplumber 0.9.0
- And other required packages

### 5. Configure Environment

```bash
cp .env.example .env
```

Edit `.env` file:
```env
FLASK_APP=app.py
FLASK_ENV=development
SECRET_KEY=your-random-secret-key-here
DATABASE_URL=sqlite:///vehicle_repair_guide.db
UPLOAD_FOLDER=uploads
```

### 6. Create Uploads Directory

```bash
mkdir -p uploads
```

### 7. Initialize Database

```bash
python -c "from app import app, init_db; init_db()"
```

You should see: `Database initialized with sample data!`

This creates:
- `vehicle_repair_guide.db` (SQLite database)
- All tables with sample vehicles and repairs

### 8. Run the Application

```bash
python app.py
```

Output should show:
```
 * Running on http://127.0.0.1:5000
```

### 9. Access the Application

Open your browser and visit: `http://localhost:5000`

## First Steps

1. Click "Register" to create a new account
2. Enter username, email, and password
3. Click "Create Account"
4. Login with your credentials
5. Explore the Dashboard

## Testing the Features

### Test Repair Guide
1. Click "Repair Guide" in navigation
2. See 5 vehicles with repairs
3. Click "View Details" on any repair
4. Read installation instructions

### Test Repair Logging
1. Click "New Repair Log"
2. Fill in vehicle info (2023 Toyota Camry)
3. Repair Type: "Oil Change"
4. Parts Cost: 25
5. Actual Hours: 0.5
6. Labor Rate: 50
7. Click "Create Repair Log"
8. See calculated total on dashboard

### Test Finance Tracker
1. Click "Finance Tracker"
2. Click "Upload Invoice PDF"
3. Select any PDF file
4. Click "Upload & Extract"
5. View extracted invoice details

## Troubleshooting

### Python not found
```bash
# Try python3 instead
python3 -m venv venv
```

### Permission denied on Linux/Mac
```bash
chmod +x venv/bin/activate
source venv/bin/activate
```

### Port 5000 already in use
```bash
# Edit app.py, change:
app.run(debug=True)
# To:
app.run(debug=True, port=5001)
```

### ModuleNotFoundError
```bash
# Ensure virtual environment is activated
# Reinstall dependencies
pip install -r requirements.txt
```

### Database errors
```bash
# Reset database
rm vehicle_repair_guide.db
python -c "from app import app, init_db; init_db()"
```

## Production Deployment

For production use:

1. Change `FLASK_ENV` to `production`
2. Generate secure `SECRET_KEY`:
   ```bash
   python -c "import secrets; print(secrets.token_hex(32))"
   ```
3. Use PostgreSQL instead of SQLite
4. Install Gunicorn: `pip install gunicorn`
5. Run: `gunicorn -w 4 -b 0.0.0.0:8000 app:app`

## Deactivating Virtual Environment

When you're done:
```bash
deactivate
```

## Getting Help

- Check [README.md](README.md) for full documentation
- Review [QUICKSTART.md](QUICKSTART.md) for quick reference
- Check app.py comments for code explanations
- Visit https://github.com/victoriagarrett088/vehicle-repair-guide for issues