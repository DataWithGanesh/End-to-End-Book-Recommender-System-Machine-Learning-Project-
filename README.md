# End-to-End-Book-Recommender-System-Machine-Learning-Project-

(Machine Learning • Collaborative Filtering • Streamlit • Docker • AWS EC2 • MLOps Pipeline)

## Overview

This is a fully production-ready End-to-End Book Recommendation System designed using Collaborative Filtering, built with a modular ML pipeline architecture, and deployed using Docker on an AWS EC2 instance.
The project includes:

- A complete config-driven MLOps pipeline
- Stage-wise processing (ingestion → validation → transformation → model training)
- KNN-based collaborative filtering recommender
- Real-time Streamlit UI
- Containerized deployment using Docker
- Cloud hosting using AWS EC2

This project demonstrates production-level ML Engineering and MLOps skills required for AI/ML Engineer, MLE, Data Scientist, and Generative AI Engineer roles.

## Recommender System Technique (Used in This Project)

Collaborative Filtering (Model-Based)

This project uses cosine similarity + KNN to identify similar books based on user–item interactions.

Why Collaborative Filtering?

- Doesn't require book metadata (author, genre, etc.)
- Learns patterns from user behavior
- Best for E-Commerce apps where users rate/purchase books
- Scales well with thousands of users & books

## Project Architecture

books_recommender/
│
├── components/
│ ├── stage_00_data_ingestion.py
│ ├── stage_01_data_validation.py
│ ├── stage_02_data_transformation.py
│ └── stage_03_model_trainer.py
│
├── config/
│ ├── configuration.py
│ └── config.yaml
│
├── entity/
│ └── config_entity.py
│
├── constant/
│
├── exception/
│ └── exception_handler.py
│
├── logger/
│ └── log.py
│
├── pipeline/
│ └── training_pipeline.py
│
├── utils/
│ └── util.py
│
├── app.py
├── Dockerfile
├── setup.py
├── requirements.txt
└── .dockerignore

This structure follows best practices used in MLOps-based ML systems.

## Tech Stack

### Machine Learning

- Collaborative Filtering
- Cosine Similarity
- KNN (k-Nearest Neighbors)
- Pivot matrix transformation

### Frameworks & Tools

- Python
- Streamlit
- Scikit-learn
- Pandas, NumPy, SciPy

### MLOps & Engineering

- YAML-based configuration
- Modular components
- Logging and Exception Handling
- Docker
- AWS EC2 (Ubuntu 22.04)

## Features

- End-to-End ML Pipeline
- Automated data ingestion, validation & transformation
- Cosine similarity-based recommendation engine
- Real-time book recommendations
- Dockerized application for production
- Cloud deployment on AWS EC2
- Scalable MLOps-friendly architecture
- Streamlit interactive UI

## How the Recommendation Works

1. Generate a user–item interaction matrix using ratings
2. Convert matrix into a pivot table with books as rows
3. Apply K-Nearest Neighbors on this matrix
4. For any input book → return top N similar books
5. Streamlit app displays recommendations

## Workflow

- config.yaml
- entity
- config/configuration.py
- components
- pipeline
- main.py
- app.py

## Local Development Setup?

#### STEP 01 - Clone the Repository

Clone the repository

```bash
https://github.com/entbappy/End-to-end-Book-Recommender-System-live
```

#### STEP 01- Create a conda environment after opening the repository

```bash
conda create -n books python=3.7.10 -y
conda activate books
```

#### STEP 02 - Install Install Dependencies

```bash
pip install -r requirements.txt
```

#### STEP 03 - Git Commandas

```bash
git add .
git commit -m "readme update"
git push origin main
```

#### STEP - 04 - run

```bash
streamlit run app.py
```

#### STEP - 05 Docker Setup (Local)

##### Build Docker Image

```bash
docker build -t book-recommender .
```

##### Run Docker Container

```bash
docker run -d -p 8501:8501 book-recommender
```

##### Access Application

```bash
http://localhost:8501
```

## Deploy Streamlit App on AWS EC2 using Docker

### Step 1: Launch EC2 Instance

1. Log in to your AWS Management Console
2. Go to ➝ EC2 → Launch Instance
3. Choose OS:

- Ubuntu 20.04 or Ubuntu 22.04 (recommended)

4. Instance type:

- t2.micro (free tier) or t3.micro

5. Key pair:

- Create or choose one (for SSH login)

6. Security Group (VERY IMPORTANT):

- Add Inbound Rule

  - Type: Custom TCP
  - Port: 8501
  - Source: 0.0.0.0/0

- Add SSH rule for port 22

Why port 8501?
Streamlit runs on port 8501 by default.

### Step 2: Connect to EC2 Instance

From your local machine:

```bash
ssh -i yourkey.pem ubuntu@<EC2-PUBLIC-IP>
```

### Step 3: Update and Upgrade Packages

```bash
sudo apt-get update -y
sudo apt-get upgrade -y
```

Explanation:
Updates package list & installs latest security patches.

### Step 4: Install Docker

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

Explanation:
This installs the latest docker engine using the official Docker script.

### Step 5: Add current user to Docker group (optional but recommended)

```bash
sudo usermod -aG docker ubuntu
```

Logout & login again:

```bash
exit
```

Then SSH again.

Explanation:
Allows running Docker commands without sudo.

### Step 6: Copy Your Project Files to EC2

##### Option 1 — Using Git Clone (recommended)

If your app is on GitHub:

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
```

##### Option 2 — Upload files using SCP

From your local:

```bash
scp -i yourkey.pem -r ./your-folder ubuntu@<EC2-IP>:/home/ubuntu/
```

### Step 7: Create a Dockerfile for Streamlit

Inside your app directory:

```bash
FROM python:3.9-slim

WORKDIR /app

COPY . /app

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 8501

CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

### Step 8: Build Docker Image

```bash
docker build -t my-streamlit-app .
```

Explanation:
Creates a Docker image named my-streamlit-app.

### Step 9: Run Docker Container

```bash
docker run -d -p 8501:8501 my-streamlit-app
```

- -p 8501:8501 → maps EC2 port to container port
- -d → run container in detached mode

### Step 10: Access Streamlit App in Browser

```bash
http://<EC2-PUBLIC-IP>:8501
```

### Step 11: Check Running Containers

```bash
docker ps
```

Stop container:

```bash
docker stop <container-id>
```
