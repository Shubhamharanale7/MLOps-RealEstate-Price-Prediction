# 🏠 MLOps Application for Real Estate Price Prediction

![Python](https://img.shields.io/badge/Python-3.8+-blue?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-API-black?style=for-the-badge&logo=flask&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Container-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-CI/CD-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![MIT License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Active-success?style=for-the-badge)

---

## 📌 Overview

A production-grade **MLOps pipeline** for real estate price prediction using
the California Housing dataset. Demonstrates end-to-end MLOps practices —
from model training and serving via **Flask API** to containerization with
**Docker** and automated deployment through **GitHub Actions CI/CD**.

---

## 🏗️ System Architecture
User Request

│

▼

┌─────────────────────────────────────────────────────┐

│              Flask Web Application                  │

│         index.html (Frontend UI)                    │

│         predict.js (API calls)                      │

└──────────────────────┬──────────────────────────────┘

│ POST /predict

▼

┌─────────────────────────────────────────────────────┐

│              Flask REST API (server.py)             │

│         Input validation + preprocessing            │

└──────────────────────┬──────────────────────────────┘

│

▼

┌─────────────────────────────────────────────────────┐

│         ML Model (california_housing_model.joblib)  │

│         Scikit-learn trained model                  │

│         Returns predicted house price               │

└─────────────────────────────────────────────────────┘

---

## 🔄 CI/CD Pipeline Flow
Developer pushes code

│

▼

GitHub Actions Triggered

│

├── main.yml

│   ├── Setup Python 3.8+

│   ├── Install dependencies

│   ├── Run tests

│   └── Build Docker image

│

└── GHCR.yml

├── Build Docker image

└── Push to GitHub Container Registry

│

▼

Docker image available at:

ghcr.io/Shubhamharanale7/mlops-realestate

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **ML Model** | Scikit-learn | California housing price prediction |
| **Model Serving** | Flask | REST API for predictions |
| **Frontend** | HTML + CSS + JavaScript | User interface |
| **Containerization** | Docker | Application packaging |
| **CI/CD** | GitHub Actions | Automated build and deploy |
| **Registry** | GitHub Container Registry | Docker image storage |
| **Model Format** | Joblib | Model serialization |

---

## 📁 Project Structure
MLOps-RealEstate-Price-Prediction/

│

├── .github/

│   └── workflows/

│       ├── main.yml              # CI pipeline

│       └── GHCR.yml              # Docker push to GHCR

│

├── app/

│   ├── server.py                 # Flask API server

│   ├── model.py                  # Model loading & prediction

│   ├── init.py               # App initialization

│   └── california_housing_model.joblib  # Trained ML model

│

├── static/

│   ├── style.css                 # Frontend styling

│   └── predict.js                # API call logic

│

├── templates/

│   └── index.html                # Frontend UI

│

├── Dockerfile                    # Docker image definition

├── requirements.txt              # Python dependencies

├── runserver.py                  # Application entry point

└── README.md

---

## 🚀 Setup & Installation

### Prerequisites
- Python 3.8+
- pip
- Docker (for containerized deployment)
- Git

### Local Development

**Step 1 — Clone Repository:**
```bash
git clone https://github.com/Shubhamharanale7/MLOps-RealEstate-Price-Prediction.git
cd MLOps-RealEstate-Price-Prediction
```

**Step 2 — Create Virtual Environment:**
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

**Step 3 — Install Dependencies:**
```bash
pip install -r requirements.txt
```

**Step 4 — Run Application:**
```bash
python app/server.py
```
Visit `http://127.0.0.1:5000` in your browser.

---

## 🐳 Docker Deployment

**Build Docker Image:**
```bash
docker build -t mlops-realestate .
```

**Run Docker Container:**
```bash
docker run -p 5000:5000 mlops-realestate
```

Access at: `http://localhost:5000`

**Pull from GitHub Container Registry:**
```bash
docker pull ghcr.io/shubhamharanale7/mlops-realestate:latest
docker run -p 5000:5000 ghcr.io/shubhamharanale7/mlops-realestate:latest
```

---

## 📡 API Reference

### Predict House Price

```http
POST /predict
Content-Type: application/json
```

**Request Body:**
```json
{
  "MedInc": 8.3252,
  "HouseAge": 41.0,
  "AveRooms": 6.984,
  "AveBedrms": 1.024,
  "Population": 322.0,
  "AveOccup": 2.555,
  "Latitude": 37.88,
  "Longitude": -122.23
}
```

**Response:**
```json
{
  "predicted_price": 452600.0,
  "currency": "USD",
  "status": "success"
}
```

---

## ⚙️ GitHub Actions Workflows

### main.yml — CI Pipeline
```yaml
name: CI/CD Pipeline
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: '3.8'
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    - name: Build Docker image
      run: docker build -t mlops-realestate .
```

### GHCR.yml — Push to Container Registry
```yaml
name: Build and Push Docker image to GHCR
on:
  push:
    branches: [ main ]
jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Login to GHCR
      uses: docker/login-action@v1
      with:
        registry: ghcr.io
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}
    - name: Build and Push
      uses: docker/build-push-action@v2
      with:
        push: true
        tags: ghcr.io/shubhamharanale7/mlops-realestate:latest
```

---

## 🧠 ML Model Details

| Property | Value |
|----------|-------|
| **Dataset** | California Housing (sklearn) |
| **Algorithm** | Scikit-learn Regression |
| **Format** | Joblib serialized model |
| **Features** | 8 housing features |
| **Target** | Median house price (USD) |
| **Serving** | Flask REST API |

### Input Features

| Feature | Description |
|---------|-------------|
| `MedInc` | Median income in block group |
| `HouseAge` | Median house age in block group |
| `AveRooms` | Average number of rooms per household |
| `AveBedrms` | Average number of bedrooms per household |
| `Population` | Block group population |
| `AveOccup` | Average number of household members |
| `Latitude` | Block group latitude |
| `Longitude` | Block group longitude |

---

## 🗺️ Future Roadmap

- [ ] Add model retraining pipeline with DVC
- [ ] Implement MLflow experiment tracking
- [ ] Add model drift monitoring
- [ ] Deploy to AWS ECS / Azure Container Apps
- [ ] Add Prometheus metrics endpoint
- [ ] Implement A/B testing for model versions
- [ ] Add Kubernetes deployment manifests

---

## 📜 License

MIT License — feel free to use, modify, and distribute.

---

## 🙌 Contact

**Shubham Haranale**

📩 [LinkedIn](https://www.linkedin.com/in/shubhamharanale7)
📧 shubhaminfosoft7@gmail.com
🐙 [GitHub](https://github.com/Shubhamharanale7)

> If you found this helpful, consider ⭐ starring the repo!
