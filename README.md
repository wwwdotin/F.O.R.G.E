


# F.O.R.G.E

### Forest Observation & Regulation Governance Engine

F.O.R.G.E is a **real-time forest monitoring and governance platform** designed to detect, validate, and regulate deforestation activities using satellite intelligence, machine learning, and structured compliance workflows.

It transforms forest monitoring from a **reactive system** into a **proactive, accountable, and verifiable governance pipeline**.

---

# 📌 Table of Contents

* Problem Statement
* Solution Overview
* Key Features
* System Architecture
* Workflow
* Tech Stack
* Machine Learning Approach
* Data Pipeline
* Project Structure
* Setup Instructions
* API Overview
* Future Improvements
* Contributors

---

# 🚨 Problem Statement

Illegal deforestation is typically identified **after damage is complete**.

### Current Challenges:

* Reliance on **optical satellite imagery** (fails under cloud cover)
* Lack of **real-time detection**
* No system for **contractor accountability**
* No linkage between:

  * detection
  * verification
  * enforcement
  * restoration

### Consequences:

* Delayed enforcement
* High false positives
* Poor contractor tracking
* Disconnected restoration efforts

---

# 💡 Solution Overview

F.O.R.G.E introduces a **multi-layer governance system** that integrates:

* Satellite-based disturbance detection (SAR + Optical)
* Permit-based validation system
* Contractor compliance tracking
* Risk-based alert prioritization
* Evidence-backed verification
* Reforestation coordination

---

# 🔑 Key Features

## 1. Early Disturbance Detection

* Uses **Sentinel-1 (SAR)** → works in clouds
* Uses **Sentinel-2** → vegetation analysis
* Detects:

  * vegetation loss
  * soil exposure
  * road expansion

---

## 2. Permit-Based Governance

* Contractors must submit:

  * Geo-boundaries (polygons)
  * Time windows
  * Expected logging volume
* System validates **all detected activity** against permits

---

## 3. Real-Time Compliance Checking

Automatically verifies:

* Location inside approved boundary
* Activity within time window
* Logging within allowed limits

---

## 4. Evidence-Based Alerts

Each alert includes:

* Before (T1) satellite image
* After (T2) satellite image
* Highlighted disturbance region

➡️ Enables **visual verification**, not blind trust in ML

---

## 5. Contractor Accountability

* Geo-tagged uploads (images/logs)
* Timestamp verification
* Logging volume checks
* Traceable batch IDs for timber

---

## 6. Risk Scoring Engine

Prioritizes alerts using:

* ML predictions
* historical patterns
* anomaly detection

---

## 7. Restoration Integration

* NGOs & volunteers:

  * view cleared areas
  * upload plantation proof
* Tracks reforestation progress

---

# 🏗️ System Architecture

## 1. Data Ingestion Layer

* Fetch satellite data from Google Earth Engine
* Inputs:

  * Sentinel-1 (SAR)
  * Sentinel-2 (Optical)

### Processing:

* normalization
* tiling
* temporal stacking

---

## 2. Monitoring & Detection Layer

### A. Disturbance Detection

* Detects changes in:

  * vegetation
  * terrain
  * surface roughness

### B. Route Compliance

* Contractor routes → buffered zones
* Disturbance outside buffer → anomaly

---

## 3. Compliance & Verification Layer

### Permit System

* Submit → Review → Approve → Store (PostGIS)

### Validation Rules

* Boundary check
* Time check
* Threshold check

### Logging Verification

* Geo-tag match
* Volume consistency

---

## 4. Intelligence Layer (ML)

* Segmentation (U-Net / DeepLab)
* Temporal detection (ConvLSTM)
* Risk prediction (XGBoost)
* Anomaly detection (Isolation Forest)

---

## 5. Action Layer

Outputs:

* Structured alerts
* Incident reports
* Evidence snapshots

---

## 6. Restoration Layer

* Cleared land → available for NGOs
* Plantation tracking
* Proof submission

---

# 🔄 End-to-End Workflow

1. Contractor submits permit
2. Authority approves/rejects
3. Satellite data continuously monitored
4. ML detects disturbances
5. System validates against permit
6. Risk score assigned
7. Alerts generated (if violation)
8. Evidence stored (before/after images)
9. Authority reviews dashboard
10. Contractor uploads proof/logs
11. NGOs perform reforestation

---

# 🧠 Machine Learning Approach

## Core Models

### 1. Segmentation

* U-Net / DeepLabV3+
* Detect:

  * roads
  * cleared land

---

### 2. Temporal Detection

* ConvLSTM / Temporal CNN
* Detects **change over time**

---

### 3. Risk Prediction

* XGBoost / Random Forest
* Outputs:

  * probability of illegal activity

---

### 4. Anomaly Detection

* Isolation Forest / Autoencoder
* Detects unusual patterns

---

## Strategy

✔ Use **ensemble approach**
✔ Combine SAR + optical + ML
✔ Focus on reliability over complexity

---

# 📊 Data Pipeline

## Sources:

* Google Earth Engine
* Sentinel-1 (SAR)
* Sentinel-2

## Steps:

1. Fetch tiles
2. Normalize
3. Tile images
4. Create time pairs (T1, T2)
5. Generate training datasets

---

# 🗂️ Project Structure

```
forge/
│
├── frontend/                         # Member 1 (React)
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Map/
│   │   │   ├── Alerts/
│   │   │   ├── PermitForms/
│   │   │   ├── ImageCompare/
│   │   │   └── Layout/
│   │   │
│   │   ├── pages/
│   │   │   ├── Dashboard.jsx
│   │   │   ├── PermitPage.jsx
│   │   │   ├── AlertsPage.jsx
│   │   │   ├── ContractorPage.jsx
│   │   │   └── Login.jsx
│   │   │
│   │   ├── services/                 # API calls
│   │   │   ├── api.js
│   │   │   ├── permitService.js
│   │   │   ├── alertService.js
│   │   │   ├── evidenceService.js
│   │   │   └── authService.js
│   │   │
│   │   ├── hooks/
│   │   ├── utils/
│   │   ├── assets/
│   │   ├── App.jsx
│   │   └── main.jsx
│   │
│   ├── package.json
│   └── .env
│
├── backend/                          # Member 2
│   ├── app/
│   │   ├── main.py                   # FastAPI entry
│   │
│   │   ├── api/                      # Route layer
│   │   │   ├── routes/
│   │   │   │   ├── auth.py
│   │   │   │   ├── permit.py
│   │   │   │   ├── alerts.py
│   │   │   │   ├── evidence.py
│   │   │   │   ├── logs.py
│   │   │   │   ├── risk.py
│   │   │   │   └── anomaly.py
│   │
│   │   ├── services/                 # Business logic
│   │   │   ├── permit_service.py
│   │   │   ├── alert_service.py
│   │   │   ├── compliance_service.py
│   │   │   ├── evidence_service.py
│   │   │   ├── log_service.py
│   │   │   └── risk_service.py
│   │
│   │   ├── models/                   # ORM models
│   │   │   ├── user.py
│   │   │   ├── permit.py
│   │   │   ├── route.py
│   │   │   ├── alert.py
│   │   │   ├── log.py
│   │   │   └── evidence.py
│   │
│   │   ├── schemas/                  # Pydantic schemas
│   │   │   ├── user_schema.py
│   │   │   ├── permit_schema.py
│   │   │   ├── alert_schema.py
│   │   │   ├── log_schema.py
│   │   │   └── risk_schema.py
│   │
│   │   ├── database/
│   │   │   ├── connection.py
│   │   │   ├── init_db.py
│   │   │   └── migrations/
│   │
│   │   ├── utils/
│   │   │   ├── geo_utils.py
│   │   │   ├── file_utils.py
│   │   │   └── auth_utils.py
│   │
│   │   └── config.py
│   │
│   ├── requirements.txt
│   └── .env
│
├── ml/                               # Member 3B
│   ├── segmentation/
│   │   ├── unet.py
│   │   ├── deeplab.py
│   │   └── train_segmentation.py
│   │
│   ├── change_detection/
│   │   ├── temporal_cnn.py
│   │   ├── convlstm.py
│   │   └── train_change.py
│   │
│   ├── risk_model/
│   │   ├── xgboost_model.py
│   │   ├── random_forest.py
│   │   └── train_risk.py
│   │
│   ├── inference/
│   │   ├── run_inference.py
│   │   └── output_formatter.py
│   │
│   ├── models/                       # Saved weights
│   └── utils/
│
├── data_pipeline/                    # Member 3A
│   ├── gee/
│   │   ├── fetch_sentinel1.py
│   │   ├── fetch_sentinel2.py
│   │   └── export_tiles.py
│   │
│   ├── preprocessing/
│   │   ├── normalize.py
│   │   ├── tiling.py
│   │   ├── temporal_stack.py
│   │   └── create_pairs.py           # before/after images
│   │
│   ├── datasets/
│   │   ├── raw/
│   │   ├── processed/
│   │   └── labels/
│   │
│   └── pipeline_runner.py
│
├── integration/                      # Member 4
│   ├── orchestrator/
│   │   ├── workflow_manager.py       # main pipeline
│   │   ├── trigger_engine.py         # when to run ML
│   │
│   ├── alert_engine/
│   │   ├── alert_rules.py
│   │   ├── notifier.py               # email/Twilio
│   │
│   ├── validation/
│   │   ├── boundary_check.py
│   │   ├── time_check.py
│   │   └── threshold_check.py
│   │
│   ├── connectors/
│   │   ├── ml_connector.py
│   │   ├── backend_connector.py
│   │   └── storage_connector.py
│   │
│   └── tests/
│       ├── test_workflow.py
│       └── test_alerts.py
│
├── storage/
│   ├── satellite_images/
│   │   ├── before/
│   │   └── after/
│   │
│   ├── contractor_uploads/
│   └── logs/
│
├── configs/
│   ├── environment.yaml
│   ├── constants.py
│   └── model_config.yaml
│
├── scripts/
│   ├── setup.sh
│   ├── run_backend.sh
│   ├── run_frontend.sh
│   └── run_pipeline.sh
│
├── docker/
│   ├── backend.Dockerfile
│   ├── frontend.Dockerfile
│   ├── ml.Dockerfile
│   └── docker-compose.yml
│
├── docs/
│   ├── architecture.md
│   ├── api_docs.md
│   ├── workflow.md
│   └── setup_guide.md
│
├── .env
├── README.md
└── requirements.txt

---

# ⚙️ Tech Stack

## Frontend

* React
* Mapbox / Leaflet

## Backend

* FastAPI
* PostgreSQL + PostGIS

## ML / Data

* PyTorch / TensorFlow
* Google Earth Engine

## Storage

* Cloud storage (images/logs)

## APIs

* GEE API
* Mapbox API
* Optional: Twilio

---

# 🚀 Setup Instructions

## 1. Clone Repository

```bash
git clone https://github.com/your-repo/forge.git
cd forge
```

---

## 2. Backend Setup

```bash
cd backend
pip install -r requirements.txt
uvicorn app.main:app --reload
```

---

## 3. Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

---

## 4. Run Data Pipeline

```bash
cd data_pipeline
python pipeline_runner.py
```

---

## 5. Run ML Inference

```bash
cd ml/inference
python run_inference.py
```

---

## 6. Docker (Optional)

```bash
docker-compose up --build
```

---

# 🔌 API Overview

### Auth

* `POST /auth/login`
* `POST /auth/register`

### Permits

* `POST /permits/`
* `GET /permits/`

### Alerts

* `GET /alerts/`
* `POST /alerts/`

### Evidence

* `POST /evidence/upload`

### Risk

* `GET /risk/{area_id}`

---

# 📈 Future Improvements

* Drone-based validation
* Mobile app for field officers
* Blockchain-based timber tracking
* Real-time alert push system
* Advanced AI anomaly detection
* Multi-country scaling

---

# 🌍 Impact

* Faster detection (weeks → hours)
* Reduced illegal logging
* Improved contractor accountability
* Better enforcement decisions
* Integrated reforestation

---

# 👥 Contributors

* Frontend Developer
* Backend Developer
* ML Engineers (2)
* Integration Engineer

---

# 📜 License

MIT License


