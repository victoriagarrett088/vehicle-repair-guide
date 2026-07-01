# Vehicle Repair Guide - Quick Start

## Installation (5 minutes)

### Step 1: Clone & Setup
```bash
git clone https://github.com/victoriagarrett088/vehicle-repair-guide.git
cd vehicle-repair-guide
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### Step 2: Configure
```bash
cp .env.example .env
# Edit .env if needed (optional for development)
```

### Step 3: Initialize Database
```bash
python -c "from app import app, init_db; init_db()"
```

### Step 4: Run
```bash
python app.py
```

Visit `http://localhost:5000`

## First Use

1. **Register** - Create an account
2. **Explore** - Check "Repair Guide" for available repairs
3. **Log Repair** - Click "New Repair Log" to track a repair
4. **Upload Invoice** - Try the "Finance Tracker" to upload a PDF
5. **View Dashboard** - See your totals and history

## Cost Calculation Example

**Scenario**: Oil change with $25 parts, 0.5 hours labor, $50/hour rate

- Parts Cost: $25.00
- Markup (20%): $5.00
- Labor (0.5 hrs × $50): $25.00
- Subtotal: $55.00
- Tax (7.97%): $4.38
- **Total: $59.38**

## Key Features

✅ User authentication  
✅ 5 vehicles with 20+ repairs in database  
✅ Automatic cost calculations with 20% markup  
✅ 7.97% tax calculation  
✅ PDF invoice upload with AI extraction  
✅ Finance tracking and reporting  
✅ Responsive design  
✅ Secure password storage  

## Support

See [README.md](README.md) for full documentation.