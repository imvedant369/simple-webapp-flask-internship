# Simple Flask Web Application with Docker and Kubernetes

## Overview

This project is a simple Python Flask web application. It can be run locally, in Docker, and on a local Kubernetes cluster using Minikube.

## Application Routes

| Route | Response |
|---|---|
| `/` | `Welcome!` |
| `/how-are-you` | `I am good, how about you?` |

## Project Files

- `app.py` - Flask application and routes.
- `requirements.txt` - Python dependencies.
- `Dockerfile` - Instructions to build the Docker image.
- `deployment.yaml` - Kubernetes Deployment configuration.
- `service.yaml` - Kubernetes Service configuration.
- `.gitignore` - Files and folders Git should ignore.

## Run Locally

Install the dependencies and start the application:

```bash
pip install -r requirements.txt
python app.py
```

Open `http://localhost:5000` in your browser.

## Run with Docker

Build the image:

```bash
docker build -t internship-flask:v1 .
```

Run the container:

```bash
docker run --rm -p 5000:5000 internship-flask:v1
```

Test the application at:

- `http://localhost:5000/`
- `http://localhost:5000/how-are-you`

## Run with Kubernetes and Minikube

Make sure Docker, kubectl, and Minikube are installed and that Minikube is running.

If the image has not yet been built, build it:

```bash
docker build -t internship-flask:v1 .
```

Load the image into Minikube:

```bash
minikube image load internship-flask:v1
```

Create the Deployment and Service:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

Check the resources:

```bash
kubectl get deployments,pods,services
```

Access the application through the Kubernetes Service:

```bash
minikube service internship-flask --url
```

Keep this command's terminal open while using the URL it prints. Append `/how-are-you` to the URL to test the second route.

## Stop and Clean Up

To remove the Kubernetes resources created for this project:

```bash
kubectl delete -f service.yaml
kubectl delete -f deployment.yaml
```

Only run these deletion commands when you intentionally want to remove the project from Kubernetes.

## Dockerfile

The Dockerfile starts with Ubuntu, installs Flask, copies the application into the image, and starts the Flask development server.

This setup is intended for learning and local development, not production deployment.

## Kubernetes Experiments and Results

### 1. Self-Healing

I deleted a running Pod managed by the Deployment. Kubernetes automatically created a replacement Pod to maintain the desired number of replicas.

**Result:** The Deployment restored the required Pod automatically.

### 2. Scaling

I tested scaling the application from 1 replica to 2 replicas, then to 4 replicas, and finally scaled it back down to 1 replica.

**Result:** Kubernetes created additional Pods when scaling up and removed extra Pods when scaling down.

### 3. Minikube Stop and Restart

I stopped Minikube and started it again. After the cluster restarted, I checked the Deployment, Pods, and Service and tested the application again.

**Result:** The application became accessible again after the cluster restarted.

### Lessons Learned

- A Deployment maintains the desired number of application Pods.
- Kubernetes can replace a deleted Pod automatically.
- Replica counts can be increased or decreased as required.
- Minikube can be stopped and restarted without intentionally deleting the cluster.
- A Kubernetes Service provides a way to access the application.

### Evidence

Screenshots and screen recordings for the experiments are stored in the `screenshots/` and `videos/` folders, respectively, when captured and saved.

