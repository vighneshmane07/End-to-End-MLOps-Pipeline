# MLOps Pipeline with FastAPI, MLflow and Docker

**Professional portfolio summary:** This project demonstrates a clean end-to-end MLOps workflow: model training, experiment tracking, artifact management, API inference, testing, and containerized deployment. It is designed as a GitHub-ready portfolio project that shows practical machine learning engineering practices using a lightweight Iris classification example.

## Project Description

This repository contains a production-style machine learning pipeline that trains a classifier, tracks experiments with MLflow, serves predictions through FastAPI, and runs locally or with Docker Compose.

The example model uses the Iris dataset so the full workflow is easy to run, inspect, and extend without requiring private data or large binaries in the repository.

## Problem Statement

Machine learning projects often fail to move cleanly from notebooks to deployable services. This project solves that gap by organizing the workflow into reproducible training code, tracked experiments, a versionable API layer, tests, and containerized services.

The prediction task is multiclass flower classification using four numeric input features:

- Sepal length
- Sepal width
- Petal length
- Petal width

## Architecture Overview

```text
Dataset -> Training Pipeline -> MLflow Tracking -> Model Artifact -> FastAPI Service -> Prediction Response
```

The training script loads data, trains a model, logs parameters and metrics to MLflow, and writes the trained artifact locally. The FastAPI service loads the trained model when available and exposes a `/predict` endpoint for inference.

## Tech Stack

- Python 3.11
- FastAPI
- Uvicorn
- scikit-learn
- MLflow
- pandas and NumPy
- pytest
- Docker and Docker Compose
- Makefile automation

## Folder Structure

```text
.
├── app/
│   ├── main.py
│   ├── predict.py
│   └── schemas.py
├── data/
├── docs/
│   ├── api.md
│   └── architecture.md
├── models/
├── screenshots/
├── tests/
│   ├── test_api.py
│   └── test_prediction.py
├── training/
│   └── train.py
├── .dockerignore
├── .env.example
├── .gitignore
├── Dockerfile
├── docker-compose.yml
├── LICENSE
├── Makefile
├── README.md
└── requirements.txt
```

## Installation

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

On Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

You can also use:

```bash
make install
```

## How to Run Locally

Start the FastAPI service:

```bash
make api
```

Then open:

- API docs: `http://localhost:8000/docs`
- Health check: `http://localhost:8000/health`

## How to Run with Docker Compose

```bash
make docker-up
```

Services:

- FastAPI: `http://localhost:8000`
- MLflow UI: `http://localhost:5000`

Stop the services:

```bash
make docker-down
```

## How to Start MLflow UI

```bash
make mlflow
```

Then open:

```text
http://localhost:5000
```

## How to Train the Model

```bash
make train
```

The training command:

- Loads the Iris dataset
- Trains a Random Forest classifier
- Logs metrics and parameters to MLflow
- Saves the local model artifact to `models/model.joblib`

Model artifacts are intentionally ignored by Git to keep the repository lightweight.

## How to Serve Predictions with FastAPI

Run the API:

```bash
make api
```

Send a request to:

```text
POST http://localhost:8000/predict
```

## Example API Request

```bash
curl -X POST "http://localhost:8000/predict" \
  -H "Content-Type: application/json" \
  -d '{
    "sepal_length": 5.1,
    "sepal_width": 3.5,
    "petal_length": 1.4,
    "petal_width": 0.2
  }'
```

## Example API Response

```json
{
  "prediction": 0,
  "label": "setosa",
  "model_source": "models/model.joblib"
}
```

If no trained model exists locally yet, the API uses a small deterministic fallback rule so the service remains testable.

## Screenshots

Add screenshots here after running the project locally:

- FastAPI Swagger UI
- MLflow experiment page
- Docker Compose services
- Example prediction response

## What I Learned

- How to structure a machine learning project for reproducibility
- How to separate training logic from inference logic
- How to track experiments, metrics, and artifacts with MLflow
- How to expose a trained model through FastAPI
- How to keep generated files, local databases, logs, and model binaries out of Git
- How to package an ML service with Docker Compose
- How to add simple automated tests for API and prediction behavior

## Future Improvements

- Add CI/CD with GitHub Actions
- Add model validation before registration
- Add a real external dataset ingestion step
- Add batch inference support
- Add monitoring for latency, errors, and model drift
- Add authentication and rate limiting for production deployments
- Publish a container image to a registry

