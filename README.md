# Food Delivery Time Prediction System

Full-stack application that predicts food delivery time (minutes) using a **Random Forest** model trained on `archive/Food_Delivery_Times.csv`.

## Tech stack

| Layer | Technology |
|-------|------------|
| Frontend | HTML, CSS, JavaScript |
| Backend | PHP (XAMPP) |
| Database | MySQL |
| ML | Python, Scikit-learn, Pandas |

## Features

- Predict delivery ETA from distance, weather, traffic, time of day, vehicle, prep time, and courier experience
- Store predictions in MySQL
- View prediction history and model metrics in the UI
- Health check for database, trained model, and Python integration

## Setup (XAMPP)

### 1. Python dependencies

```bash
cd c:\xampp\htdocs\delivery_time_predict
pip install -r ml/requirements.txt
```

### 2. Train the model

```bash
python ml/train_model.py
```

This creates `ml/pipeline.joblib` and `ml/model_meta.json`.

### 3. MySQL database

1. Start **Apache** and **MySQL** in XAMPP.
2. Open phpMyAdmin: http://localhost/phpmyadmin
3. Import `database/schema.sql` (or run it in the SQL tab).

Default credentials in `api/config.php`: user `root`, empty password.

### 4. Python path (if needed)

If PHP cannot find Python, edit `api/config.php`:

```php
define('PYTHON_BIN', 'C:\\Python311\\python.exe');
```

### 5. Open the app

http://localhost/delivery_time_predict/

## API endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `api/predict.php` | POST | Run prediction and save to DB |
| `api/history.php` | GET | List recent predictions (`?limit=20`) |
| `api/meta.php` | GET | Model metadata and dropdown options |
| `api/health.php` | GET | System health check |

### Example predict request

```json
POST /delivery_time_predict/api/predict.php
{
  "distance_km": 9.5,
  "weather": "Rainy",
  "traffic_level": "Medium",
  "time_of_day": "Evening",
  "vehicle_type": "Scooter",
  "preparation_time_min": 18,
  "courier_experience_yrs": 4
}
```

## Dataset features

| Column | Role |
|--------|------|
| Distance_km | Numeric |
| Weather | Clear, Rainy, Foggy, Snowy, Windy |
| Traffic_Level | Low, Medium, High |
| Time_of_Day | Morning, Afternoon, Evening, Night |
| Vehicle_Type | Bike, Scooter, Car |
| Preparation_Time_min | Numeric |
| Courier_Experience_yrs | Numeric |
| **Delivery_Time_min** | **Target (predicted)** |

## Project structure

```
delivery_time_predict/
├── archive/Food_Delivery_Times.csv
├── api/                 # PHP backend
├── assets/css, js/      # Frontend
├── database/schema.sql
├── ml/                  # Training & prediction
├── index.html
└── README.md
```

## Retrain after data changes

Replace or update the CSV in `archive/`, then run `python ml/train_model.py` again.
