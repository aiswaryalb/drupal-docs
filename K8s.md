# Kubernetes Cheat Sheet (Minikube + YAML)

## Cluster Basics
```
minikube start  
kubectl get nodes  
kubectl cluster-info  
```
---

## Pod (Learning Only)
```
apiVersion: v1  
kind: Pod  
metadata:  
  name: nginx-pod  
  labels:  
    app: nginx  
spec:  
  containers:  
    - name: nginx  
      image: nginx:1.25  
      ports:  
        - containerPort: 80  
```
```
kubectl apply -f pod.yaml  
kubectl get pods  
kubectl delete pod nginx-pod  
```
---

## Deployment (Production)
```
apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: nginx-deployment  
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
          image: nginx:1.25  
          ports:  
            - containerPort: 80  
```
```
kubectl apply -f deployment.yaml  
kubectl scale deployment nginx-deployment --replicas=4  
```
---

## Service – ClusterIP
```
apiVersion: v1  
kind: Service  
metadata:  
  name: nginx-service  
spec:  
  type: ClusterIP  
  selector:  
    app: nginx  
  ports:  
    - port: 80  
      targetPort: 80  
```
```
kubectl get svc  
kubectl get endpoints nginx-service  
```
---

## Service – NodePort
```
apiVersion: v1  
kind: Service  
metadata:  
  name: nginx-nodeport  
spec:  
  type: NodePort  
  selector:  
    app: nginx  
  ports:  
    - port: 80  
      targetPort: 80  
      nodePort: 30007  
```

```
minikube service nginx-nodeport  
```
---

## ConfigMap
```
apiVersion: v1  
kind: ConfigMap  
metadata:  
  name: app-config  
data:  
  APP_MODE: dev  
```
---

## Secret
```
apiVersion: v1  
kind: Secret  
metadata:  
  name: app-secret  
type: Opaque  
data:  
  PASSWORD: cGFzcw==  
```
---

## Health Probes
```
livenessProbe:  
  httpGet:  
    path: /  
    port: 80  
  initialDelaySeconds: 10  
  periodSeconds: 10  

readinessProbe:  
  httpGet:  
    path: /  
    port: 80  
  initialDelaySeconds: 5  
  periodSeconds: 5  
```
---

## Resources
```
resources:  
  requests:  
    cpu: "100m"  
    memory: "128Mi"  
  limits:  
    cpu: "500m"  
    memory: "256Mi"  
```
---

## Ingress (Minikube)
```
minikube addons enable ingress
```

```
apiVersion: networking.k8s.io/v1  
kind: Ingress  
metadata:  
  name: nginx-ingress  
spec:  
  rules:  
    - host: nginx.local  
      http:  
        paths:  
          - path: /  
            pathType: Prefix  
            backend:  
              service:  
                name: nginx-service  
                port:  
                  number: 80  
```
---

## Namespaces
```
kubectl create namespace dev  
kubectl apply -f deployment.yaml -n dev  
```
---

## Debugging Commands
```
kubectl describe pod <pod>  
kubectl logs <pod>  
kubectl exec -it <pod> -- sh  
kubectl get events  
kubectl get pods -w  
```
---

## Mental Model
```
Ingress
  ↓
Service
  ↓
Deployment
  ↓
ReplicaSet
  ↓
Pods
```

 
