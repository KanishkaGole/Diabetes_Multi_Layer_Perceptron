# Diabetes Multi Layer Perceptron Inference UI

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.16-FF6F00?logo=tensorflow&logoColor=white)](https://www.tensorflow.org/)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![Jenkins](https://img.shields.io/badge/Jenkins-Pipeline-D24939?logo=jenkins&logoColor=white)](https://www.jenkins.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## Overview

<!-- helloo -->

Web-based application for diabetes risk prediction using a Keras MLP model. Provides a FastAPI backend with a simple UI for patient input, supports runtime model uploads, and enables inference through containerized deployment using Docker and Jenkins.

The application supports:

- Uploading model artifacts at runtime
- Entering patient features through a structured UI
- Running inference via FastAPI
- Deploying through Docker and Jenkins

## System Architecture

- Backend: FastAPI service for model upload and prediction endpoints
- Frontend: Static HTML, CSS, and JavaScript served by FastAPI
- Model runtime: TensorFlow Keras model loaded from uploaded artifact
- Deployment: Containerized with Docker, automated with Jenkins pipeline

## Project Structure

```text
.
├── backend/
│   ├── main.py
│   └── requirements.txt
├── frontend/
│   ├── index.html
│   ├── script.js
│   └── styles.css
├── Dockerfile
├── Jenkinsfile
└── README.md
```

## Prerequisites

- Python 3.10 or newer
- pip
- Docker (optional, for containerized run)
- Jenkins with Docker access (optional, for CI/CD)

## Local Development

1. Create and activate a virtual environment.

```bash
python -m venv .venv
source .venv/bin/activate
```

2. Install dependencies.

```bash
pip install -r backend/requirements.txt
```

3. Start the application.

```bash
python backend/main.py
```

4. Open the UI.

```text
http://localhost:8020/
```

## Docker

Build the image:

```bash
docker build -t diabetes_multi_layer_perceptron:latest .
```

Run the container:

```bash
docker run --rm -p 8020:8020 diabetes_multi_layer_perceptron:latest
```

Access the app at http://localhost:8020/.

## Jenkins Pipeline

The repository includes a declarative pipeline in Jenkinsfile.

Pipeline stages:

1. Checkout source
2. Build Docker image
3. Deploy container
4. Run health check

Jenkins requirements:

- Docker CLI and daemon access on the Jenkins agent
- curl installed on the Jenkins agent

To use it, create a Jenkins Pipeline job connected to this repository. Jenkins will automatically detect and run Jenkinsfile.

## API Endpoints

### POST /upload

Uploads the model and metadata files used for inference.

Required multipart files:

- model: .keras file
- meta: .json file
- classes: .json file
- scaler: .json file

### POST /predict

Runs diabetes prediction on the uploaded model using patient input features.

Request body fields:

- age
- gender (M or F)
- bmi
- chol
- tg
- hdl
- ldl
- creatinine
- bun

## Model Artifact Guidelines

For best prediction consistency, metadata should include:

- feature_columns: ordered feature names used during training
- classes_: class labels for output mapping
- scaler_mean_ and scaler_scale_: values from training-time standardization

Fallback behavior:

- Missing scaler values: identity scaling is used
- Missing class labels: sensible defaults are applied
- 
## Contributors

- **Kanishka Gole** - [GitHub Profile](https://github.com/KanishkaGole)
- **Tanisha Ahuja** 
- **Arjun Ghone**
  
## License

This project is licensed under the MIT License. See LICENSE for details.


