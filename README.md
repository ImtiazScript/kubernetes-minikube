# Kubernetes with Minikube - Learning Guide

## Setup Minikube in windows
### Install Minikube
Browse following url to download and install minikube
or run following command to install using command
```shell
New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
```

### Add the minikube.exe binary to your PATH.
Run following command in power shell as administrator
```shell
$oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
if ($oldPath.Split(';') -inotcontains 'C:\minikube'){
  [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine)
}
```

### Start Minikube
```shell
minikube start
```

## 1. Deploying a Sample Application

To get started with Kubernetes, here's how you can deploy a simple `nginx` application on your Minikube cluster:

### Deploy the Application
```bash
minikube kubectl -- create deployment nginx --image=nginx
```

### Check Pod Status
Verify if the pod is running:
```bash
minikube kubectl -- get pods
```

### Expose the Deployment
Expose the deployment to make it accessible from outside the cluster:
```bash
minikube kubectl -- expose deployment nginx --type=NodePort --port=80
```

### Access the Service
Use Minikube to get the URL to access the `nginx` service:
```bash
minikube service nginx --url
```

## 2. Scaling the Deployment
You can scale your `nginx` deployment to run multiple replicas of the `nginx` pod:
```bash
minikube kubectl -- scale deployment nginx --replicas=3
```

Verify the new replicas:
```bash
minikube kubectl -- get pods
```

## 3. Viewing Logs
You can inspect the logs from one of the `nginx` pods:
```bash
minikube kubectl -- logs <pod-name>
```

Replace `<pod-name>` with the actual pod name.

## 4. Inspecting the Service
View detailed information about your `nginx` service:
```bash
minikube kubectl -- describe service nginx
```

## 5. Using YAML for Deployment
Instead of using commands, you can create a deployment using a YAML configuration. Here's an example `nginx-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```

Apply this YAML file using:
```bash
minikube kubectl -- apply -f nginx-deployment.yaml
```

## 6. Cleaning Up
Once you're done experimenting, you can delete the resources:
```bash
minikube kubectl -- delete deployment nginx
minikube kubectl -- delete service nginx
```

## 7. Explore More
- **Persistent Storage**: Try attaching a persistent volume to a pod.
- **ConfigMaps/Secrets**: Learn how to inject configuration or sensitive data.
- **Ingress Controller**: Set up an ingress to handle external traffic to multiple services.
- **Helm**: Use Helm to manage more complex Kubernetes applications.

Happy learning!