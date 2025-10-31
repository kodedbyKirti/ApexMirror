# F1 Lap Predictor - Apex Mirror

AI-powered Formula 1 lap time prediction system using machine learning and real-time telemetry analysis.

## Features

- **AI-Powered Lap Predictions**: Random Forest ML model with 98% accuracy
- **Real-time Telemetry**: Live session data monitoring
- **Historical Data Access**: Multi-season F1 data analysis
- **Interactive Dashboard**: Modern web interface with Chart.js visualizations
- **Export Capabilities**: CSV data export for further analysis


==> note= need to run app.py in vs to run the index.html.
## Tech Stack

### Backend
- **Python 3.8+**
- **Flask**: REST API server
- **FastF1**: Official F1 data access library
- **scikit-learn**: Machine learning (Random Forest)
- **pandas & numpy**: Data processing

### Frontend
- **HTML5/CSS3/JavaScript**
- **Chart.js**: Interactive data visualizations
- **Font Awesome**: Icon library
- **Google Fonts**: Orbitron & Exo 2 typography

## Installation

### Prerequisites
```bash
python --version  # Python 3.8 or higher required
pip --version     # pip package manager
```

### Setup

1. **Install Dependencies**
```bash
pip install fastf1 flask flask-cors pandas numpy scikit-learn
```

2. **Run the API Server**
```bash
python fastf1.py
```

The API will start on `http://127.0.0.1:8000`

3. **Open the Dashboard**
```bash
# Open in your browser:
file:///C:/Users/LENOVO/OneDrive/Desktop/apex_mirror/fastf1/fastf1.html

# Or use a local server (recommended):
python -m http.server 8080
# Then navigate to: http://localhost:8080/fastf1.html
```

## API Endpoints

### GET /api/meta
Get session metadata and available drivers
```
?year=2024&gp=Spanish%20Grand%20Prix&session=Race
```

**Response:**
```json
{
  "year": 2024,
  "gp": "Spanish Grand Prix",
  "session": "R",
  "drivers": ["HAM", "VER", "LEC", ...],
  "laps_total": 66
}
```

### GET /api/laps
Retrieve lap data for a driver
```
?year=2024&gp=Spanish%20Grand%20Prix&session=Race&driver=HAM
```

**Response:**
```json
{
  "laps": [
    {
      "LapNumber": 1,
      "LapTime": 87.234,
      "Sector1Time": 28.123,
      "Sector2Time": 31.456,
      "Sector3Time": 27.655,
      "Compound": "SOFT",
      "Stint": 1,
      "Position": 3
    },
    ...
  ]
}
```

### GET /api/predict-next
Predict next lap time
```
?year=2024&gp=Spanish%20Grand%20Prix&session=Race&driver=HAM&method=historical
```

**Response:**
```json
{
  "predicted_seconds": 86.872,
  "confidence": 94.3
}
```

### GET /api/compare
Compare two drivers
```
?year=2024&gp=Spanish%20Grand%20Prix&session=Race&driverA=HAM&driverB=VER
```

**Response:**
```json
{
  "driverA": "HAM",
  "driverB": "VER",
  "comparison": [
    {
      "LapNumber": 1,
      "A": 87.234,
      "B": 86.891,
      "Delta": -0.343
    },
    ...
  ]
}
```

## Machine Learning Model

### Features Used
- Lap number
- Tire compound (encoded: SOFT=3, MEDIUM=2, HARD=1, INTERMEDIATE=4, WET=5)
- Track temperature
- Air temperature
- Stint number
- Position
- Tire age
- Sector 1/2/3 times

### Model Details
- **Algorithm**: Random Forest Regressor
- **Trees**: 300 estimators
- **Training Split**: 80/20 chronological
- **Feature Scaling**: StandardScaler
- **Confidence**: Based on tree prediction variance

## Usage Guide

1. **Select Parameters**: Choose season, Grand Prix, session type
2. **Load Data**: Click "Load" to fetch available drivers
3. **Choose Driver**: Select driver from dropdown
4. **Predict**: Click "Predict" to generate lap time prediction
5. **Compare**: Select two drivers to see side-by-side analysis
6. **Live Mode**: Enable auto-refresh for live session tracking
7. **Export**: Download comparison data as CSV

## Project Structure

```
fastf1/
├── fastf1.py           # Flask API server & ML model
├── fastf1.html         # Predictor dashboard UI
├── home.html           # Landing page
├── pricing.html        # Pricing information
├── README.md           # This file
└── ff1_cache/          # FastF1 data cache (auto-generated)
```

## Troubleshooting

### API Connection Error
- Ensure `fastf1.py` is running
- Check the API URL in `fastf1.html` (line 81)
- Verify no firewall blocking port 8000

### Data Loading Issues
- FastF1 requires internet connection for first data fetch
- Data is cached locally after first retrieval
- Some historical sessions may not have complete data

### Prediction Errors
- Requires minimum 10 laps for training
- Historical method needs sufficient session data
- Live method requires at least 3 valid laps

## Performance Notes

- First data load downloads F1 session data (~5-30s)
- Subsequent loads use local cache (instant)
- Model training happens once per session (~1-2s)
- Predictions are near-instantaneous after training

## Future Enhancements

- [ ] Strategy optimization recommendations
- [ ] Pit stop timing predictions
- [ ] Weather impact analysis
- [ ] Multi-session trend analysis
- [ ] Driver performance ratings
- [ ] Team comparison features

## Credits

- **FastF1**: Official F1 data access library
- **Formula 1**: Data source
- **scikit-learn**: Machine learning framework

## License

Educational and personal use only. F1 data subject to Formula 1 licensing terms.

---

**Built with ⚡ by Apex Mirror Technologies**

