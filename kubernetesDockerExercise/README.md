# Developing and deploying a Node.js app from Docker to Kubernetes

## Assumptions:
- It is assumed that you already have node and npm installed
- It is also assumed that **minikube** environment for kubernetes is configured for installing single node cluster. Or else you also use the **kataconda** which is a cloud based simulation for running minikube.  

## Step 1: Make A Separate Directory And Initialize The Node Application

```
mkdir nodejs
cd nodejs/
npm init
```

## Step 2: Installing Express

```
npm install express --save
```
