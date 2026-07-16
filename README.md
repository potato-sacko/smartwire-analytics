# SmartWire Analytics

A React + TypeScript + FastAPI web application for analyzing overhead wiring measurement data from SmartWire surveys.

## Overview

SmartWire Analytics processes single or multiple overhead wiring survey datasets to detect defects, analyze trends, and generate engineering reports. Each record represents a measurement point along a route, with integrated support for future multi-file comparison analysis.

## Features

### Data Import & Processing
- Upload `.xlsx` and `.csv` files
- Automatic worksheet selection and preview
- Intelligent engineering column detection and mapping
- Manual column mapping adjustment
- Invalid value cleaning (#NUM!, empty cells, etc.)
- Coordinate parsing (GPS and Inertial formats)

### Analysis & Visualization
- **Summary Statistics**: Data quality metrics, measurement ranges, coverage analysis
- **Plots**: CSA, height, stagger, wear, remaining thickness vs. distance
- **Correlation Analysis**: Relationships between engineering parameters
- **Rolling Window Analytics**: Configurable moving averages and trend detection
- **Defect Detection**:
  - Point defects (single measurement anomalies)
  - Prolonged defect sections (configurable 60m, 120m, 250m thresholds)
- **Interactive Mapping**: Defect visualization with coordinate overlays

### Engineering Tools
- Configurable thresholds for all engineering parameters
- Baseline vs. comparison analysis (framework for future multi-file)
- Data quality assessment

### Export
- CSV export (cleaned data)
- Excel export (data + statistics)
- PDF engineering report (summary + key findings)

## Project Structure

```
smartwire-analytics/
├── frontend/                    # React + TypeScript frontend
│   ├── src/
│   │   ├── components/         # Reusable UI components
│   │   │   ├── FileUpload.tsx
│   │   │   ├── WorksheetSelector.tsx
│   │   │   ├── ColumnMapper.tsx
│   │   │   ├── Dashboard.tsx
│   │   │   ├── StatisticsPanel.tsx
│   │   │   ├── PlotViewer.tsx
│   │   │   ├── DefectTable.tsx
│   │   │   ├── MapViewer.tsx
│   │   │   ├── CorrelationMatrix.tsx
│   │   │   ├── ThresholdConfig.tsx
│   │   │   └── ExportPanel.tsx
│   │   ├── hooks/
│   │   │   ├── useFileUpload.ts
│   │   │   ├── useAnalysis.ts
│   │   │   └── useThresholds.ts
│   │   ├── types/
│   │   │   ├── index.ts        # Frontend type definitions
│   │   │   └── api.ts          # API contract types
│   │   ├── services/
│   │   │   ├── api.ts          # API client
│   │   │   └── export.ts       # Export utilities
│   │   ├── App.tsx
│   │   ├── App.css
│   │   └── index.tsx
│   ├── public/
│   ├── package.json
│   ├── tsconfig.json
│   └── Dockerfile
├── backend/                     # FastAPI backend
│   ├── app/
│   │   ├── main.py            # FastAPI application entry point
│   │   ├── models/
│   │   │   ├── __init__.py
│   │   │   ├── data.py        # Data models
│   │   │   ├── analysis.py    # Analysis result models
│   │   │   └── comparison.py  # Future multi-file comparison models
│   │   ├── services/
│   │   │   ├── __init__.py
│   │   │   ├── file_processor.py    # File upload & parsing
│   │   │   ├── column_mapper.py     # Column detection & mapping
│   │   │   ├── data_cleaner.py      # Data validation & cleaning
│   │   │   ├── coordinate_parser.py # Coordinate parsing
│   │   │   ├── analytics_engine.py  # Main analysis engine
│   │   │   ├── defect_detector.py   # Defect detection algorithms
│   │   │   ├── correlation_analyzer.py
│   │   │   ├── rolling_window.py
│   │   │   ├── mapper.py            # Map generation
│   │   │   └── exporter.py          # CSV/Excel/PDF export
│   │   ├── api/
│   │   │   ├── __init__.py
│   │   │   ├── routes.py       # API endpoints
│   │   │   └── schemas.py      # Pydantic schemas
│   │   ├── constants.py        # Engineering constants & defaults
│   │   └── config.py           # Configuration
│   ├── tests/
│   │   ├── __init__.py
│   │   ├── conftest.py
│   │   ├── test_file_processor.py
│   │   ├── test_column_mapper.py
│   │   ├── test_data_cleaner.py
│   │   ├── test_coordinate_parser.py
│   │   ├── test_defect_detector.py
│   │   ├── test_analytics_engine.py
│   │   ├── test_correlation_analyzer.py
│   │   ├── test_exporter.py
│   │   └── fixtures/
│   │       ├── sample_data.xlsx
│   │       ├── sample_data.csv
│   │       └── test_data.py
│   ├── requirements.txt
│   ├── pytest.ini
│   └── Dockerfile
├── docker-compose.yml
├── .gitignore
└── .env.example
```

## Architecture

### Frontend (React + TypeScript)
- **Components**: Modular, reusable UI components for each analysis feature
- **Hooks**: Custom React hooks for file handling, API calls, and state management
- **Type Safety**: Full TypeScript coverage with shared types between frontend and backend

### Backend (FastAPI)
- **File Processing**: Parse xlsx/csv, detect worksheets, preview data
- **Column Mapping**: Automatic detection of engineering columns with manual override
- **Data Cleaning**: Handle invalid Excel values, null fields, data validation
- **Coordinate Parsing**: Convert GPS and Inertial coordinate strings to lat/lon
- **Analytics Engine**: Core analysis logic (statistics, trends, correlations)
- **Defect Detection**: Point defects and prolonged section detection with configurable thresholds
- **Export**: Generate CSV, Excel, and PDF reports

### Future: Multi-File Comparison
Internal models support baseline/comparison datasets with record matching and change detection, enabling future functionality without architectural changes.

## Tech Stack

**Frontend**
- React 18+
- TypeScript
- Axios (API client)
- Plotly.js (charting)
- Leaflet (mapping)
- Tailwind CSS (styling)

**Backend**
- FastAPI
- Pandas (data processing)
- NumPy (numerical analysis)
- SciPy (correlation analysis)
- Openpyxl (xlsx parsing)
- ReportLab (PDF generation)
- Shapely (geometric analysis)

**DevOps**
- Docker & Docker Compose
- pytest (testing)

## Getting Started

### Prerequisites
- Docker & Docker Compose
- Node.js 16+ (for local frontend development)
- Python 3.10+ (for local backend development)

### Quick Start with Docker

```bash
# Clone the repository
git clone https://github.com/potato-sacko/smartwire-analytics.git
cd smartwire-analytics

# Start the application
docker-compose up -d

# Application will be available at:
# Frontend: http://localhost:3000
# Backend API: http://localhost:8000
# API Docs: http://localhost:8000/docs
```

### Local Development

**Backend**
```bash
cd backend
python -m venv venv
source venv/bin/activate  # or `venv\Scripts\activate` on Windows
pip install -r requirements.txt
pytest
uvicorn app.main:app --reload
```

**Frontend**
```bash
cd frontend
npm install
npm start
```

## API Endpoints

### File Upload & Processing
- `POST /api/upload` - Upload xlsx/csv file
- `GET /api/worksheets/{upload_id}` - List available worksheets
- `GET /api/preview/{upload_id}` - Preview data from worksheet

### Column Mapping
- `POST /api/detect-columns` - Auto-detect engineering columns
- `POST /api/map-columns` - Manual column mapping

### Analysis
- `POST /api/analyze` - Run full analysis on dataset
- `GET /api/statistics/{analysis_id}` - Get summary statistics
- `GET /api/plots/{analysis_id}` - Get plot data
- `GET /api/defects/{analysis_id}` - Get detected defects
- `GET /api/map/{analysis_id}` - Get defect map data
- `GET /api/correlation/{analysis_id}` - Get correlation matrix

### Export
- `GET /api/export/csv/{analysis_id}` - Export as CSV
- `GET /api/export/excel/{analysis_id}` - Export as Excel
- `GET /api/export/pdf/{analysis_id}` - Export as PDF report

## Configuration

### Engineering Thresholds
Edit `backend/app/constants.py` to configure:
- CSA (Cross Section Area) limits
- Height limits
- Stagger limits
- Wear limits
- Remaining thickness limits
- Prolonged defect distance thresholds (60m, 120m, 250m)

### Example threshold configuration:
```python
ENGINEERING_THRESHOLDS = {
    'csa': {'min': 10, 'max': 150},
    'height': {'min': 0, 'max': 400},
    'stagger': {'min': -100, 'max': 100},
    'wear': {'max': 50},
    'remaining_thickness': {'min': 1},
}

DEFECT_DISTANCES = [60, 120, 250]  # meters
```

## Data Format

### Supported Columns (Auto-detected)
- Way [m] - Distance along route
- User-defined chainage [m] - Alternative distance measurement
- Decimal degree (GPS coordinates) - GPS coordinate string
- Decimal degree (Inertial coordinates) - Inertial coordinate string
- Cross section [mm²] - CSA measurement
- Height [cm] - Wire height
- Remaining thickness [mm] - Material thickness
- Stagger [cm] - Lateral displacement
- Wear [mm²] - Wear area

### Coordinate Format
- GPS: "42.3601, -71.0589" or "42.3601° N, 71.0589° W"
- Inertial: Standard coordinate string format

### Invalid Values
Treated as null:
- #NUM!
- #DIV/0!
- #REF!
- #NAME?
- #VALUE!
- Empty cells
- Non-numeric values in numeric columns

## Analysis Output

### Statistics
- Count, mean, std, min, 25%, 50%, 75%, max
- Missing data percentage
- Coverage analysis (% of route with data)

### Plots
- CSA vs. Distance
- Height vs. Distance
- Stagger vs. Distance
- Wear vs. Distance
- Remaining Thickness vs. Distance

### Defects
- **Point Defects**: Individual measurements outside threshold ranges
- **Prolonged Sections**: Continuous sections of defects over 60m, 120m, 250m
- Defect severity classification
- Geographic coordinates for each defect

### Correlation Analysis
- Pearson correlation matrix between all engineering parameters
- Visualization of relationships

### Rolling Window Analytics
- Configurable window size (default 10 points)
- Moving average for trend identification
- Derivative analysis for change rates

## Export Formats

### CSV
- Cleaned measurement data
- All engineering parameters
- Coordinates (latitude, longitude)
- Defect flags

### Excel
- Data sheet with all measurements
- Statistics sheet with summary
- Defects sheet with defect details
- Plots as embedded images

### PDF Report
- Executive summary
- Key statistics and findings
- Defect summary and severity breakdown
- Charts and visualizations
- Recommendations

## Testing

```bash
cd backend
pytest                          # Run all tests
pytest -v                       # Verbose output
pytest --cov                    # Coverage report
pytest -k test_defect_detector  # Run specific test
```

## Future Enhancements

1. **Multi-File Comparison**
   - Upload baseline and comparison datasets
   - Automatic record matching using coordinates
   - Change detection and trend analysis
   - Delta reports highlighting improvements/degradation

2. **Advanced Analytics**
   - Machine learning defect prediction
   - Seasonal trend analysis
   - Predictive maintenance scheduling

3. **Collaboration**
   - User accounts and datasets storage
   - Shared analysis and comments
   - Version history

4. **Integration**
   - GIS system integration
   - Real-time monitoring dashboard
   - API webhooks for external systems

## Contributing

Contributions are welcome! Please:
1. Create a feature branch
2. Add tests for new functionality
3. Ensure all tests pass
4. Submit a pull request

## License

MIT License - see LICENSE file for details

## Support

For issues, questions, or suggestions, please create a GitHub issue.
