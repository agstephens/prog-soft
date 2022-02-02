# Getting started with Kubernetes

In this case, running on Windows...(sorry!)...

## Enable Kubernetes in Docker Desktop

Go to Docker Desktop settings:
 * Enable Kubernetes (and it will restart).

This will have installed/enabled `kubectl` in the PowerShell.

## Getting and starting a Flask App from Docker Hub

In PowerShell:

```
kubectl.exe

# Pull the image and create a deployment (in a pod)
kubectl create deployment flask-deployment --image=jcdemo/flaskapp

# Get pods
kubectl get pods

# Expose the deployed servivce on port 5000 (internally)
kubectl expose deployment/flask-deployment --type="NodePort" --port 5000

# Find out port that it is being exposed on
kubectl get service flask-deployment -o jsonpath='{.spec.ports[].nodePort}'
30711
```

# Look in browser
http://localhost:30711

## Looking at info about a pod

```
kubectl describe pods
```
