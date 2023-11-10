# 1. Set up a pipeline (Github actions/Gitlab runner/ Jenkins or any open source tool) to build, test, create a docker image, publish and deploy to k8s. Use the application present in this public repo https://github.com/apiwizlabs/wizdesk

### Solution - [Link](https://github.com/nikki-priyaHIT/GitHub-Action-k8s-pipeline)

### Workflow.yaml

```yaml
name: CI/CD Pipeline with Docker and kind

on:
  push:
    branches: [ main ]

jobs:
  build-test-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Repository
      uses: actions/checkout@v3
      with:
        repository: apiwizlabs/wizdesk
        ref: main

    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18' 

    - name: Install Dependencies
      run: npm install

    - name: Start the Application
      run: npm start

    - name: Build Docker Image
      run: docker build -t aviralsingh2609/wizdesk:latest . 

    - name: Log in to Docker Hub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKER_HUB_USERNAME }}
        password: ${{ secrets.DOCKER_HUB_ACCESS_TOKEN }}

    - name: Push Docker Image to Docker Hub
      run: docker push aviralsingh2609/wizdesk:latest 

    - name: Install kind
      run: |
        curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64
        chmod +x ./kind
        sudo mv ./kind /usr/local/bin/kind

    - name: Create a Kubernetes cluster with kind
      run: kind create cluster

    - name: Load Docker Image to kind Cluster
      run: |
        kind load docker-image itsaviral2609/wizdesk:latest

    - name: Apply Kubernetes Manifests
      run: |
        kubectl apply -f k8s/ 

    - name: Check Deployment Status
      run: |
        kubectl rollout status deployment/my-deployment    
```
