# End-to-End-Book-Recommender-System-Machine-Learning-Project-
(Machine Learning • Collaborative Filtering • Streamlit • Docker • AWS EC2 • MLOps Pipeline)


## Workflow

- config.yaml
- entity
- config/configuration.py
- components
- pipeline
- main.py
- app.py

## How to run?

### Steps

Clone the repository

```bash
https://github.com/entbappy/End-to-end-Book-Recommender-System-live
```

#### STEP 01- Create a conda environment after opening the repository

```bash
conda create -n books python=3.7.10 -y
```

```bash
conda activate books
```

#### STEP 02 - Git Commandas

```bash
git add .
```

```bash
git commit -m "readme update"
```

```bash
git push origin main
```

#### STEP 03 - Install the requirements

```bash
pip install -r requirements.txt
```

#### STEP - 04 - run

```bash
streamlit run app.py
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
