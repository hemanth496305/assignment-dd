=>This repository is used to build and deploy a MEAN (MongoDB, Express, Angular, Node.js) application using Jenkins. The pipeline automates the following steps:
     - Cloning source code from GitHub
     - Building backend and frontend Docker image
     - Pushing Docker images to Docker Hub
     - Deploying the application to a target VM using Docker Compose
=>Project Structure:
    crud-dd-task-mean-app/
      ├── backend/ Node.js backend
      ├── frontend/ Angular frontend
      └── Jenkinsfile
      └── README.md
      └──  docker-compose.yml
      
=>Pipeline Overview:
    - Runs on Jenkins agent labeled `slave`.
=>Environment Variables:
    - `BACKEND_IMAGE`: Docker image name for backend (`hemanth496305/be:latest`)
    - `FRONTEND_IMAGE`: Docker image name for frontend (`hemanth496305/fe:latest`)
    - `DEPLOY_DIR`: Path on VM where Docker Compose is run
=>pipeline Stages:
    1. Checkout SCM
      - Clones the source code from the `master` branch of the GitHub repository:  
        `https://github.com/hemanth496305/assignment-dd.git`
    2. List Directory
      - Lists files in the cloned `crud-dd-task-mean-app` directory for verification.
    3. Build Backend Docker Image
      - Navigates to `backend/` directory.
      - Builds Docker image:  
          `docker build -t hemanth496305/be:latest .`
    4. Build Frontend Docker Image
      - Navigates to `frontend/` directory.
      - Builds Docker image:  
          `docker build -t hemanth496305/fe:latest .`
    5. Push Images to Docker Hub
      - Uses Docker credentials stored in Jenkins (`credentialsId: dockerhub`).
      - Pushes both backend and frontend images to Docker Hub.
    6. Deploy to VM
      - Navigates to the deployment directory (`$DEPLOY_DIR`) on the target VM.
      - Executes Docker Compose to pull the latest images and start containers:
          ```sh
            docker compose pull
            docker compose up -d --remove-orphans
            
  =>Post Actions:

    On Success:
        Prints ✅ Deployment successful!

    On Failure:
        Prints ❌ Pipeline failed.

=>Prerequisites:
- Jenkins Agent labeled "slave"
- Docker and Docker Compose installed on the agent/VM
- Jenkins credentials with ID "dockerhub" for Docker Hub access
- SSH access and proper permissions for deployment directory
- ports enabled " 22 - ssh
-                 80 - internet access
-                 8080 - backend Node.js API
-                 27017 - MongoDB database "
  
