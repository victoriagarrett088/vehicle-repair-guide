# Vehicle Repair Guide

A comprehensive Flask-based web application for managing vehicle repairs, tracking labor costs, calculating invoices with markups and taxes, and extracting financial data from PDF invoices using AI.

## Features

### 🔐 Authentication
- User registration and login with secure password hashing
- User session management
- Protected routes requiring authentication

### 🔧 Repair Guide Database
- Static database of repairs for all modern vehicles (2020-2024)
- Searchable by year, make, and model
- Detailed installation instructions for each repair
- Estimated labor times for accurate pricing

### 📋 Repair Log Tracker
- Create, edit, and delete repair logs
- Track vehicle information (year, make, model)
- Record actual vs. estimated labor hours
- Log parts costs with automatic markup calculation
- Automatic tax calculation (7.97%)
- Complete cost breakdown:
  - Parts Cost
  - 20% Markup on Parts
  - Labor Cost (hours × rate)
  - Subtotal
  - 7.97% Tax
  - **Total Cost**

### 💰 Finance Tracker with PDF Upload
- Upload PDF invoices (max 16MB)
- AI-powered data extraction from PDFs:
  - Vendor/supplier name
  - Invoice number and date
  - Line items with quantities and prices
  - Total amount
- Store extracted data in database
- View detailed invoice information
- Track all spending

### 📊 Dashboard
- Overview of total repairs logged
- Total spending across all repairs
- Average repair cost
- Recent repair history
- Quick access to all features

## Technology Stack

- **Backend**: Python 3.x with Flask
- **Database**: SQLite (with SQLAlchemy ORM)
- **Authentication**: Flask-Login with Werkzeug security
- **PDF Processing**: pdfplumber for text extraction
- **Frontend**: HTML5 with responsive CSS
- **Server**: Gunicorn for production

## Installation

### Prerequisites
- Python 3.7+
- pip (Python package manager)

### Setup Instructions

1. **Clone the repository**
   ```bash
   git clone https://github.com/victoriagarrett088/vehicle-repair-guide.git
   cd vehicle-repair-guide
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   
   # On Windows
   venv\Scripts\activate
   
   # On macOS/Linux
   source venv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment variables**
   ```bash
   cp .env.example .env
   ```
   
   Edit `.env` and update:
   - `SECRET_KEY`: Change to a secure random string for production
   - `DATABASE_URL`: Keep as is for SQLite, or change for PostgreSQL
   - `UPLOAD_FOLDER`: Directory for PDF uploads

5. **Initialize the database**
   ```bash
   python -c "from app import app, init_db; init_db()"
   ```
   
   This will:
   - Create the SQLite database
   - Create all tables
   - Populate with sample vehicle/repair data

6. **Run the application**
   ```bash
   python app.py
   ```
   
   The app will be available at `http://localhost:5000`

## Usage Guide

### 1. Register & Login
- Navigate to the registration page to create a new account
- Login with your credentials
- You'll be redirected to the dashboard

### 2. Browse Repair Guide
- Click "Repair Guide" in the navigation
- Search for repairs by vehicle year, make, and model
- Click "View Details" to see full installation instructions
- Click "Log Repair" to create a repair entry for that service

### 3. Create Repair Log
- Click "New Repair Log" or from the Repair Guide
- Fill in vehicle information
- Enter repair type and labor hours (estimated or actual)
- Enter parts cost
- Adjust labor rate if needed (default: $50/hr)
- Costs are calculated automatically:
  - **Markup**: 20% of parts cost
  - **Labor**: hours × hourly rate
  - **Tax**: 7.97% on subtotal
  - **Total**: All costs combined

### 4. Upload Invoices for Finance Tracking
- Click "Finance Tracker" → "Upload Invoice PDF"
- Select a PDF file from your computer
- The app will extract:
  - Vendor name
  - Invoice number and date
  - Line items with prices
  - Total amount
- View extracted data on the finance details page
- Track all invoices and total spending

### 5. View Dashboard
- See summary statistics
- View all recent repairs
- Check total spending
- Calculate average repair costs

## Database Schema

### Users
- `id`: Primary key
- `username`: Unique username
- `email`: User email
- `password_hash`: Hashed password
- `created_at`: Registration timestamp

### Vehicles
- `id`: Primary key
- `year`: Vehicle year
- `make`: Manufacturer
- `model`: Vehicle model

### RepairTypes
- `id`: Primary key
- `vehicle_id`: Foreign key to Vehicle
- `repair_name`: Name of the repair
- `estimated_hours`: Labor time estimate
- `installation_instructions`: Step-by-step guide

### RepairLogs
- `id`: Primary key
- `user_id`: Foreign key to User
- `vehicle_year`, `make`, `model`: Vehicle info
- `repair_type`: Type of repair performed
- `parts_cost`: Cost of parts
- `labor_cost`: Calculated labor cost
- `markup_amount`: 20% markup on parts
- `tax_amount`: 7.97% tax on subtotal
- `total_cost`: Final invoice total
- `created_at`: Timestamp

### FinanceUploads
- `id`: Primary key
- `user_id`: Foreign key to User
- `filename`: PDF filename
- `vendor_name`: Extracted vendor
- `invoice_number`: Extracted invoice #
- `invoice_date`: Extracted date
- `total_amount`: Extracted total
- `items_count`: Number of line items
- `raw_text`: Extracted PDF text

### PDFLineItems
- `id`: Primary key
- `finance_upload_id`: Foreign key to FinanceUpload
- `description`: Item description
- `quantity`: Item quantity
- `unit_price`: Price per unit
- `total_price`: Total item price

## Cost Calculation Formula

```
Parts Cost: [User Input]
Markup (20%): Parts Cost × 0.20
Labor Cost: Hours × Labor Rate
Subtotal: Parts + Markup + Labor
Tax (7.97%): Subtotal × 0.0797
Total: Subtotal + Tax
```

## PDF Extraction

The application uses `pdfplumber` to extract text from PDF invoices and applies pattern matching to identify:

- **Vendor Name**: First non-trivial line in document
- **Invoice Number**: Pattern matching for "Invoice #", "Inv #", etc.
- **Date**: Pattern matching for date formats (MM/DD/YYYY, MM-DD-YYYY)
- **Total Amount**: Pattern matching for "Total", "Grand Total", "Amount Due"
- **Line Items**: Patterns with descriptions and prices

Note: PDF extraction accuracy depends on invoice formatting. Review extracted data before relying on it.

## File Structure

```
vehicle-repair-guide/
├── app.py                      # Main Flask application
├── requirements.txt            # Python dependencies
├── .env.example               # Environment variables template
├── .gitignore                 # Git ignore rules
├── README.md                  # Full documentation
├── QUICKSTART.md              # Quick start guide
├── LICENSE                    # MIT License
├── templates/
│   ├── base.html              # Base template with navigation
│   ├── login.html             # Login page
│   ├── register.html          # Registration page
│   ├── dashboard.html         # Main dashboard
│   ├── repair_guide.html      # Repair database browser
│   ├── repair_details.html    # Repair details with instructions
│   ├── new_repair_log.html    # Create repair log form
│   ├── edit_repair_log.html   # Edit repair log form
│   ├── finance_tracker.html   # Finance tracker overview
│   ├── upload_pdf.html        # PDF upload interface
│   ├── finance_details.html   # Invoice details view
│   ├── 404.html               # 404 error page
│   └── 500.html               # 500 error page
├── uploads/                   # Directory for uploaded PDFs
└── vehicle_repair_guide.db    # SQLite database (auto-created)
```

## Configuration

### Environment Variables (.env)

```env
FLASK_APP=app.py
FLASK_ENV=development
SECRET_KEY=your-secret-key-here-change-in-production
DATABASE_URL=sqlite:///vehicle_repair_guide.db
UPLOAD_FOLDER=uploads
MAX_CONTENT_LENGTH=16777216
```

### For Production

1. Change `FLASK_ENV` to `production`
2. Generate a strong `SECRET_KEY`
3. Use PostgreSQL instead of SQLite:
   ```
   DATABASE_URL=postgresql://user:password@localhost/vehicle_repair_guide
   ```
4. Use Gunicorn as the server:
   ```bash
   gunicorn -w 4 -b 0.0.0.0:8000 app:app
   ```

## Sample Data

The database is automatically populated with sample data including:
- **5 Modern Vehicles** (Toyota Camry, Honda Civic, Ford F-150, Chevrolet Malibu, BMW 3 Series)
- **20+ Repair Types** with labor times and installation instructions
- Common repairs like:
  - Oil changes
  - Brake pad replacement
  - Battery replacement
  - Air filter replacement
  - And more...

## API Routes

### Authentication
- `POST /register` - Create new account
- `POST /login` - Login user
- `POST /logout` - Logout user

### Dashboard
- `GET /` - Redirect to login or dashboard
- `GET /dashboard` - View dashboard

### Repair Guide
- `GET /repair-guide` - View all vehicles and repairs
- `GET /repair-guide/search` - Search repairs by vehicle
- `GET /repair/<id>/details` - View repair details

### Repair Logs
- `GET /repair-log/new` - New repair log form
- `POST /repair-log/new` - Create repair log
- `GET /repair-log/<id>/edit` - Edit repair log form
- `POST /repair-log/<id>/edit` - Update repair log
- `POST /repair-log/<id>/delete` - Delete repair log

### Finance
- `GET /finance-tracker` - View finance tracker
- `GET /finance-tracker/upload` - Upload PDF form
- `POST /finance-tracker/upload` - Process PDF upload
- `GET /finance-tracker/<id>/details` - View invoice details
- `POST /finance-tracker/<id>/delete` - Delete invoice

## Future Enhancements

- [ ] Advanced PDF parsing with OCR
- [ ] Email notifications for repair reminders
- [ ] Export repair logs to CSV/PDF
- [ ] Multiple vehicle profiles
- [ ] Repair history and statistics
- [ ] Integration with real parts pricing APIs
- [ ] Mobile app version
- [ ] Advanced analytics and reporting
- [ ] Multi-user shop management
- [ ] Integration with payment systems

## Troubleshooting

### Database Issues
- Delete `vehicle_repair_guide.db` and run `init_db()` again
- Check database path in `.env`

### PDF Extraction Not Working
- Ensure PDF is text-based (not scanned image)
- Check file size doesn't exceed 16MB
- Review raw extracted text for accuracy

### Upload Folder Issues
- Ensure `uploads/` directory exists and is writable
- Check `UPLOAD_FOLDER` path in `.env`

## License

MIT License - See LICENSE file for details

## Support

For issues, feature requests, or contributions, please open an issue on GitHub.

---

**Created with ❤️ for vehicle repair professionals and DIY mechanics**