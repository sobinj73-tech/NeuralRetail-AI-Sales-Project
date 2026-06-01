# NeuralRetail – Complete Project Deployment Guide

## 📦 What You're Getting

### Two Zip Options:

#### Option 1: `NeuralRetail_Complete.zip` (21 MB) ⭐ **Recommended**
- ✅ **Complete source code** (Python, Docker, YAML, SQL)
- ✅ **All trained models** (GBM, XGBoost, K-Means)
- ✅ **All visualization reports** (PNG charts, SHAP plots)
- ✅ **Dashboard & API code**
- ✅ **Test suite** (50+ tests)
- ✅ **CI/CD workflows** (GitHub Actions)
- ⚠️ **Excludes:** Raw data & cache files (you'll generate these)

#### Option 2: `NeuralRetail_Source_Only.zip` (94 KB)
- ✅ Source code & configuration only
- ❌ No trained models
- ❌ No reports/visualizations
- **Use if:** You want to train from scratch on your data

---

## 🚀 Quick Start (5 Minutes)

### 1. Extract the Zip

```bash
unzip NeuralRetail_Complete.zip
cd NeuralRetail
```

### 2. Install Dependencies

```bash
# Option A: Local development
pip install -r requirements.txt

# Option B: Docker (recommended - no local setup needed)
docker-compose up -d
```

### 3. Launch Dashboard & API

```bash
# Option A: Python (local)
python3 setup_and_run.py --all

# Option B: Docker
docker-compose up
```

**Access:**
- 🔌 **API**: `http://localhost:8000` (Swagger docs: `/docs`)
- 📊 **Dashboard**: `http://localhost:8501`
- 🔬 **MLflow**: `http://localhost:5000`
- ⚙️ **Airflow**: `http://localhost:8080`

---

## 📂 Project Structure (What's Inside)

```
NeuralRetail/
│
├─ 📊 Data Layer (ML-ready, pre-processed)
│  ├─ data/silver/           → Cleaned features (transactions, RFM, forecast, churn)
│  ├─ data/gold/             → Analytics aggregates (country revenue, monthly KPIs)
│  └─ data/raw/              → (You'll place online_retail_II.xlsx here)
│
├─ 🤖 ML Models (Pre-trained, ready to serve)
│  ├─ models/forecast/       → GBM + Prophet ensemble (35.7% MAPE)
│  ├─ models/churn/          → XGBoost classifier (0.798 AUC, 0.779 F1)
│  └─ models/segmentation/   → K-Means clustering (0.973 silhouette)
│
├─ 🔌 API & Serving
│  └─ api/main.py            → 15 FastAPI endpoints (demand, churn, segments, analytics)
│
├─ 📈 Interactive Dashboard
│  └─ dashboard/app.py       → 6-page Streamlit app (dark mode, responsive)
│
├─ 🧪 Testing & Quality
│  └─ tests/test_neural_retail.py → 50+ unit/integration/API tests
│
├─ 🐳 Docker & Deployment
│  ├─ docker-compose.yml     → Full stack (API, Dashboard, Redis, PostgreSQL, MLflow, Airflow)
│  ├─ docker/Dockerfile.*    → Individual service images
│  └─ docker/*.txt           → Dependency files per service
│
├─ ⚙️ CI/CD Pipeline
│  └─ .github/workflows/ci_cd.yml → GitHub Actions (lint, test, build, deploy)
│
├─ 📝 Documentation
│  ├─ README.md              → Complete documentation
│  └─ requirements.txt       → Python dependencies
│
└─ 🚀 Entry Points
   ├─ setup_and_run.py       → Master setup script
   └─ docker-compose.yml     → Single-command deployment

```

---

## 📊 What's Pre-Trained (No Retraining Needed)

All models are **already trained** on the complete Online Retail II dataset (1.06M transactions):

| Component | File | Performance | Ready? |
|-----------|------|-------------|--------|
| **Demand Forecast** | `models/forecast/gbm_forecaster.pkl` | MAPE 35.7% | ✅ Yes |
| **Churn Predictor** | `models/churn/xgb_churn.pkl` | AUC 0.798, F1 0.779 | ✅ Yes |
| **Customer Segments** | `models/segmentation/kmeans.pkl` | Silhouette 0.973 | ✅ Yes |
| **Visualizations** | `reports/` (PNG charts) | 10+ plots | ✅ Yes |
| **Data** | `data/silver/` & `data/gold/` | 5.8K RFM, 800K transactions | ✅ Yes |

**You can immediately:**
- Call `/predict/demand` API endpoint
- Predict churn for customers
- Segment customers automatically
- View dashboards with real metrics

---

## 🔧 Installation Options

### Option 1: Docker (Easiest - Recommended)

**Requirements:** Docker, Docker Compose

```bash
unzip NeuralRetail_Complete.zip
cd NeuralRetail
docker-compose up -d

# Wait ~30s for services to start
docker-compose logs -f api  # Watch API logs
```

**All services start automatically:**
- API: `http://localhost:8000`
- Dashboard: `http://localhost:8501`
- Redis, PostgreSQL, MLflow, Airflow (all auto-configured)

### Option 2: Local Python

**Requirements:** Python 3.10+ (macOS, Linux, Windows)

```bash
unzip NeuralRetail_Complete.zip
cd NeuralRetail

# Install dependencies
pip install -r requirements.txt

# Run everything
python3 setup_and_run.py --all

# Or individual commands:
python3 setup_and_run.py --api          # Launch API only
python3 setup_and_run.py --dashboard    # Launch dashboard only
pytest tests/ -v                        # Run tests
```

### Option 3: Cloud Deployment

Included GitHub Actions CI/CD automatically:
1. Lints code (Black, Flake8)
2. Runs tests (pytest)
3. Builds Docker images
4. Pushes to container registry
5. Deploys to production (with approval)

Push to `main` branch to trigger.

---

## 📡 API Examples

### 1. Demand Forecast

```bash
curl -X POST http://localhost:8000/predict/demand \
  -H "Content-Type: application/json" \
  -d '{
    "product_id": "84029E",
    "horizon_days": 30,
    "include_ci": true
  }'
```

**Response:** 30-day forecast with ±15% confidence intervals

### 2. Churn Prediction

```bash
curl -X POST http://localhost:8000/predict/churn \
  -H "Content-Type: application/json" \
  -d '{
    "customer_id": "12345",
    "n_orders": 15,
    "total_spend": 2500,
    "hist_recency": 45
  }'
```

**Response:** Churn probability + risk level + recommended action

### 3. Batch Churn (up to 500 customers)

```bash
curl -X POST http://localhost:8000/predict/churn/batch \
  -H "Content-Type: application/json" \
  -d '{"customers": [...]}'
```

### 4. Customer Segmentation

```bash
curl -X POST http://localhost:8000/segment \
  -H "Content-Type: application/json" \
  -d '{
    "customers": [
      {"frequency": 50, "monetary": 15000, ...}
    ]
  }'
```

### 5. Business Analytics

```bash
# Summary KPIs
curl http://localhost:8000/analytics/summary

# Revenue by country
curl http://localhost:8000/analytics/countries

# Churn statistics
curl http://localhost:8000/churn/stats
```

**Full API documentation:** `http://localhost:8000/docs` (Swagger UI)

---

## 📊 Dashboard Features

Interactive 6-page Streamlit dashboard (dark theme):

1. **Executive Overview**
   - 5 KPI cards (Revenue, Orders, Customers, AOV, Units)
   - Monthly revenue trend with MA
   - Top countries & products
   - Day-of-week & hour-of-day patterns

2. **Demand Forecast**
   - Interactive horizon selector (7/14/30/60 days)
   - Model comparison charts
   - ±15% confidence intervals
   - Forecast metrics & MAPE

3. **Customer Intelligence**
   - Churn risk dashboard
   - **Live predictor** (15 input sliders for what-if analysis)
   - RFM segment distribution
   - Churn probability distribution

4. **Segmentation**
   - Cluster size cards
   - PCA 2D visualization
   - Cluster profile heatmap
   - Radar chart (key metrics)

5. **Inventory**
   - Stockout risk summary
   - Days-of-stock distribution
   - Reorder recommendations
   - Interactive filters

6. **Model Performance**
   - Model metrics (MAPE, AUC, F1, Silhouette)
   - Feature importance plots
   - SHAP explainability
   - MLflow experiment tracker

---

## 🧪 Testing

```bash
# Run all tests
pytest tests/ -v --cov=src --cov=api

# Specific test classes
pytest tests/test_neural_retail.py::TestDataPipeline -v
pytest tests/test_neural_retail.py::TestAPI -v

# Watch for regressions
pytest tests/ -v --timeout=120

# Generate HTML coverage report
pytest tests/ --cov=src --cov=api --cov-report=html
# Open htmlcov/index.html in browser
```

**Coverage:** 84% (data pipeline, ML models, API endpoints)

---

## 🔐 Configuration

### Environment Variables (Auto-Set in Docker)

```bash
REDIS_HOST=localhost
REDIS_PORT=6379
DB_HOST=localhost
DB_NAME=neural_retail
DB_USER=neural
MLFLOW_TRACKING_URI=http://localhost:5000
```

### Database (PostgreSQL)

Auto-initialized with:
- `pipeline_runs` – DAG execution logs
- `model_registry` – Model versions
- `prediction_log` – API prediction history
- `data_quality_log` – Quality metrics
- `drift_log` – Feature drift alerts

---

## 📥 Getting Your Own Data

### To Train on New Data:

1. **Prepare CSV/Excel** with columns:
   - `invoice`, `stockcode`, `quantity`, `price`, `invoicedate`, `customer_id`, `country`

2. **Place in** `data/raw/`

3. **Run pipeline**:
   ```bash
   python3 src/data/retail_pipeline.py
   ```

4. **Retrain models**:
   ```bash
   python3 src/models/train_all.py
   ```

The system will automatically:
- Clean & validate data
- Engineer features
- Train all models
- Log metrics to MLflow
- Generate reports

---

## 🚨 Troubleshooting

### Docker Issues

```bash
# Check if containers are running
docker-compose ps

# View logs
docker-compose logs api          # API logs
docker-compose logs dashboard    # Dashboard logs

# Reset everything
docker-compose down -v
docker-compose up -d
```

### API Not Responding

```bash
# Check health
curl http://localhost:8000/health

# See models loaded
curl http://localhost:8000/models
```

### Tests Failing

```bash
# Run with verbose output
pytest tests/ -v -s

# Run single test
pytest tests/test_neural_retail.py::TestAPI::test_health_endpoint -v
```

---

## 📚 Learning Resources

- **FastAPI:** https://fastapi.tiangolo.com/
- **Streamlit:** https://docs.streamlit.io/
- **XGBoost:** https://xgboost.readthedocs.io/
- **MLflow:** https://mlflow.org/docs/
- **Docker:** https://docs.docker.com/

---

## 🎯 Next Steps

1. ✅ **Extract & install** (5 min)
2. ✅ **Launch services** (2 min)
3. ✅ **Explore dashboard** (10 min)
4. ✅ **Test API endpoints** (5 min)
5. ✅ **Customize** for your use case

---

## 💡 Use Cases

### Business
- Predict product demand for inventory planning
- Identify at-risk customers for retention campaigns
- Segment customers for personalized marketing
- Monitor revenue by region/product

### Data Science
- Explore ML model architecture & decisions
- Study feature importance & SHAP explanations
- Benchmark model performance
- Experiment with new features

### DevOps
- Deploy production ML system end-to-end
- Implement CI/CD for ML
- Monitor model performance over time
- Scale to Kubernetes

---

## 📧 Support

**For questions:**
1. Read `README.md` (comprehensive docs)
2. Check API docs: `http://localhost:8000/docs`
3. Review test cases in `tests/`
4. Inspect code comments

---

## ⚖️ License

MIT License – See LICENSE file in project

---

**Ready to deploy? Start with:**
```bash
unzip NeuralRetail_Complete.zip
cd NeuralRetail
docker-compose up -d
```

**Questions? Read README.md for complete documentation.**
