
# Kubernetes Utilities Commands

## General Kubernetes Commands

### Get Kubernetes command help:
```bash
kubectl help
```

### See pods:
```bash
kubectl get pods
```

### Get inside of a pod:
```bash
kubectl exec -it POD_NAME -- /bin/sh
```

## Minikube Commands

### Start Minikube and open dashboard:
```bash
minikube start
minikube status
minikube dashboard
```

## Docker Commands

### Build Docker image & push image to DockerHub:

#### Build Docker image locally:
```bash
docker build -t kub-first-app .
```

#### Manually create repository:
- Create a Docker repository on [DockerHub](https://hub.docker.com/)

#### Tag local build:
```bash
docker tag kub-first-app emtiaj2011/kub-first-app
```

#### Push local tag to DockerHub:
```bash
docker push emtiaj2011/kub-first-app
```

## Kubernetes Deployment

### Create a deployment in Kubernetes cluster:
```bash
kubectl create deployment first-app --image=emtiaj2011/kub-first-app
```

### Check deployments:
```bash
kubectl get deployments
```

### Check pods:
```bash
kubectl get pods
```

## Expose Deployment

### Expose deployment by creating a service:
```bash
kubectl expose deployment first-app --type=LoadBalancer --port=8080
```

### Check newly created service:
```bash
kubectl get services
```

### Get the exposed URL:
```bash
minikube service first-app
```

## Load Balancing and Scaling

### Scale to 3 replicas:
```bash
kubectl scale deployment/first-app --replicas=3
```

## Updating the Deployment

1. Change any code from the project.

2. Build again with a new tag:
```bash
docker build -t emtiaj2011/kub-first-app:2 .
```

3. Push latest build to DockerHub:
```bash
docker push emtiaj2011/kub-first-app:2
```

4. Update local Minikube deployment with the latest deployment from DockerHub:
```bash
kubectl set image deployment/first-app kub-first-app=emtiaj2011/kub-first-app:2
```

### Check Rollout Status:
```bash
kubectl rollout status deployment/first-app
```

### Rollback (Undo Rollout) if any problem occurs:
```bash
kubectl rollout undo deployment/first-app
```

### See Rollout History:
```bash
kubectl rollout history deployment/first-app
```

### See details of a rollout history (for debugging):
```bash
kubectl rollout history deployment/first-app --revision=1
```

### Go back to a particular revision:
```bash
kubectl rollout undo deployment/first-app --to-revision=1
```

## Deleting Services and Deployments

### Delete a Kubernetes service:
```bash
kubectl delete service first-app
```

### Delete a Kubernetes deployment:
```bash
kubectl delete deployment first-app
```

---

## Declarative Approach

Instead of using commands (Imperative Approach), we can use YAML files to define Kubernetes objects and apply them.

### Example Deployment YAML:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: second-app-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: second-app
      tier: backend
  template:
    metadata:
      labels:
        app: second-app
        tier: backend
    spec:
      containers:
        - name: second-node
          image: emtiaj2011/kub-first-app:2
```

To apply the deployment, run:
```bash
kubectl apply -f deployment.yaml
```

### Example Service YAML:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend
spec:
  selector:
    app: second-app
  ports:
    - protocol: 'TCP'
      port: 80
      targetPort: 8080
  type: LoadBalancer
```

To apply the service, run:
```bash
kubectl apply -f service.yaml
minikube service backend
```

### Update

To update anything, just modify the config file and reapply using the apply command:
```bash
kubectl apply -f deployment.yaml
```

## Deletion Using Config Files

### Delete a Deployment:
```bash
kubectl delete -f deployment.yaml
```

### Delete a Service:
```bash
kubectl delete -f service.yaml
```

### Delete both at once:
```bash
kubectl delete -f deployment.yaml,service.yaml
```

---

## Persistent Volumes

### Get persistent volumes:
```bash
kubectl get pv
```

### Get all persistent volume claims:
```bash
kubectl get pvc
```
