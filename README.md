# CloudShield — Cloud Threat Detection Platform

CloudShield is a full-stack web application that uses machine learning to classify potential security threats in cloud environments and presents model results through an interactive dashboard.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Dataset](#dataset)
- [Models](#models)
- [Getting Started](#getting-started)
- [Docker](#docker)
- [Deployment](#deployment)
- [API](#api)
- [Project Structure](#project-structure)
- [Limitations](#limitations)
- [Future Improvements](#future-improvements)

## Overview

As organizations rely more on cloud computing, analyzing large volumes of security-related data to spot suspicious activity becomes difficult. CloudShield addresses this by:

1. Training classification models on cloud security data.
2. Serving predictions through a Django application with authentication and REST APIs.
3. Visualizing model performance (accuracy, confusion matrices, model comparison) in a dashboard.

## Features

- **Threat classification** — predicts whether cloud activity is normal or a potential threat.
- **Multiple models** — four model configurations trained and compared on the same data.
- **Authentication** — login required to access the application.
- **REST API** — endpoints for submitting input and receiving predictions in JSON.
- **Analytics dashboard** — Chart.js visualizations of accuracy, confusion matrices, and model comparison.
- **Containerized** — Docker support for consistent setup across environments.
- **Deployed on Render** — accessible online.

## Tech Stack

| Area | Technology |
| --- | --- |
| Language | Python, JavaScript |
| Backend | Django |
| ML / Data | Scikit-learn, Pandas, Joblib |
| Frontend / Charts | HTML, CSS, JavaScript, Chart.js |
| DevOps | Docker, Render, CI/CD |

## Architecture

```
Dataset (CSV)
     │
     ▼
Pandas ── clean & prepare data
     │
     ▼
Scikit-learn ── LabelEncoder + StandardScaler + model training
     │
     ▼
Joblib ── save trained model and preprocessors
     │
     ▼
Django ── authentication, REST APIs, inference
     │
     ▼
Chart.js dashboard ── accuracy, confusion matrix, model comparison
```

**Key design point:** the same preprocessing (encoders and scaler fitted during training) is applied to incoming data at inference time, so predictions are consistent with how the models were trained.

## Dataset

- **File:** `cloud_security_dataset.csv`
- **Target column:** `label`
- **Classes:** `Normal`, `Intrusion`, `Insider`, `DDoS`
- **Example features:** source/destination IP addresses, protocol, bytes transferred, login attempts, failed logins, CPU usage, memory usage, network latency, anomaly score

> Add the original source of the dataset here (website, paper, or note that it is synthetic) if applicable.

## Models

Four model configurations are trained and compared. Their names reflect the architecture they are meant to represent, but they are implemented with **Scikit-learn equivalents**, not TensorFlow/Keras deep-learning networks.

| Model name | Scikit-learn implementation |
| --- | --- |
| CNN | `MLPClassifier` (128 → 64 → 32) |
| LSTM | `GradientBoostingClassifier` |
| Autoencoder | `RandomForestClassifier` |
| Hybrid | `MLPClassifier` (256 → 128 → 64 → 32) |

**Evaluation metrics:** accuracy, precision, recall, F1-score, and confusion matrices.

| Model | Accuracy |
| --- | --- |
| CNN | _add value_ |
| LSTM | _add value_ |
| Autoencoder | _add value_ |
| Hybrid | _add value_ |

> Fill in the table from your results page before publishing.

## Getting Started

### Prerequisites

- Python 3.9+
- pip

### Installation

```bash
# Clone the repository
git clone <your-repo-url>
cd cloudshield

# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Run the application

```bash
python manage.py migrate
python manage.py createsuperuser   # optional
python manage.py runserver
```

Open http://127.0.0.1:8000/ in your browser.

### Train the models

Run the training script that uses `ml_models/ml_engine.py` to load the dataset, preprocess it, train the four models, and save them with Joblib. Adjust the command below to match your repository:

```bash
python manage.py <train-command>
```

## Docker

```bash
docker build -t cloudshield .
docker run -p 8000:8000 cloudshield
```

## Deployment

The application is deployed on **Render**.

Live demo: _add URL here_

## API

Endpoints are served by Django and return JSON. Authentication is required.

| Method | Endpoint | Description |
| --- | --- | --- |
| POST | `/api/predict/` | Submit input features, receive a predicted class |

> Update this table to match your actual routes and request/response formats.

Example request:

```json
{
  "failed_logins": 5,
  "bytes_transferred": 120000,
  "cpu_usage": 87.5
}
```

Example response:

```json
{
  "prediction": "Intrusion"
}
```

## Project Structure

```
cloudshield/
├── manage.py
├── requirements.txt
├── Dockerfile
├── cloud_security_dataset.csv
├── ml_models/
│   └── ml_engine.py        # preprocessing, training, inference
├── <django_app>/           # views, URLs, authentication, APIs
├── templates/              # HTML pages
└── static/                 # JavaScript, CSS, Chart.js dashboards
```

> Adjust folder names to match your repository.

## Limitations

- The "CNN", "LSTM", and "Autoencoder" models are Scikit-learn equivalents, not true deep-learning implementations.
- Results depend on the dataset used and may not generalize to real-world cloud traffic.
- "Real-time" classification depends on the deployment environment and response time.

## Future Improvements

- Implement true CNN/LSTM/Autoencoder models with TensorFlow or PyTorch.
- Train on real-world or larger cloud security datasets.
- Add alerting and logging of detected threats.
- Expand CI/CD to include automated tests before deployment.

## License

Add your license here (for example, MIT).
