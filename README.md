FastAPI Docker CI/CD:

Overview - 
This project demonstrates Continuous Delivery (CD) by automating the creation and deployment of a Dockerized FastAPI application using GitHub Actions. The workflow builds and pushes the Docker image to Docker Hub upon every push to the main branch.

Features - 
a. FastAPI server with a simple JSON response.
b. Dockerfile for containerization.
c. GitHub Actions workflow for automated build & push to Docker Hub.
d. Uses GitHub Secrets for secure authentication.

1. Running the FastAPI App Locally -
Prerequisites:
Ensure you have Python 3.8+ and pip installed.

Installation:
# Clone the repository
git clone https://github.com/Panchikagupta05/Fastapi-Docker-CI-CD.git
cd Fastapi-Docker-CI-CD
# Create a virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
# Install dependencies
pip install -r requirements.txt
# Run the FastAPI app
uvicorn main:app --host 0.0.0.0 --port 8000

Access the API:
After running the server, open:
Swagger UI: http://127.0.0.1:8000/docs
API Response: http://127.0.0.1:8000

2. Running with Docker -
Build and Run the Docker Image Locally -
# Build the Docker image
docker build -t fastapi-app .
# Run the Docker container
docker run -p 8000:8000 fastapi-app
Now, access the API at http://localhost:8000.

3. GitHub Actions Workflow -
The workflow automates the Docker build and push process.

Trigger:
Runs when code is pushed to the main branch.

Workflow Steps:
Checkout Repository: Fetches the latest code.
Set Up Docker Buildx: Enables better Docker builds.
Authenticate to Docker Hub: Uses a secret token (DOCKERTOKEN).

Build and Push Docker Image:
Builds the Docker image.
Pushes it to Docker Hub.

GitHub Actions File (.github/workflows/DockerBuild.yml):
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2
      
      - name: Log in to Docker Hub
        run: echo "${{ secrets.DOCKERTOKEN }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin
      
      - name: Build Docker Image
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/fastapi-app:latest .
      
      - name: Push Docker Image to Docker Hub
        run: docker push ${{ secrets.DOCKER_USERNAME }}/fastapi-app:latest

4. Setting Up GitHub Secrets -
To securely authenticate with Docker Hub, add the following secrets in your GitHub repository:

DOCKER_USERNAME → Your Docker Hub username.
DOCKERTOKEN → Generated from Docker Hub (See Below).

Generating a Docker Hub Token -
Sign in to Docker Hub.
Go to Account Settings → Security → Access Tokens.
Click Generate Token and copy it.
Add it to GitHub Secrets as DOCKERTOKEN.

5. Docker Hub Image -
Your Docker image is available at:
🔗 Docker Hub Repository
To pull and run the latest image:
docker pull 051104/fastapi-app:latest
docker run -p 8000:8000 051104/fastapi-app:latest

6. Conclusion -
This project showcases a CI/CD pipeline for a FastAPI application using GitHub Actions and Docker Hub. Every push to the main branch automatically builds and pushes the Docker image, enabling a smooth deployment workflow.

