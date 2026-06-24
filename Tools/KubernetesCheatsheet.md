# ☸️ Kubernetes Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Kubernetes (k8s) + kubectl quick reference.

---

## Cluster & Context

```bash
kubectl version
kubectl cluster-info
kubectl config get-contexts
kubectl config use-context <name>
kubectl get nodes
```

## Core Resource Commands

```bash
kubectl get pods                          # list pods
kubectl get pods -A                       # all namespaces
kubectl get pods -o wide                  # extra columns
kubectl get svc,deploy,pods               # multiple types
kubectl get pods -n kube-system           # specific namespace
kubectl get pods --watch                  # live updates

kubectl describe pod <name>               # detailed status/events
kubectl logs <pod>                        # container logs
kubectl logs -f <pod>                     # follow
kubectl logs <pod> -c <container>         # multi-container pod
kubectl exec -it <pod> -- bash            # shell into a pod
```

## Apply / Delete

```bash
kubectl apply -f deployment.yaml          # create/update from file
kubectl apply -f ./manifests/             # whole directory
kubectl delete -f deployment.yaml
kubectl delete pod <name>
kubectl delete deployment myapp
```

## Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp
          image: myapp:1.0
          ports:
            - containerPort: 3000
          env:
            - name: NODE_ENV
              value: production
          resources:
            requests: { cpu: "100m", memory: "128Mi" }
            limits:   { cpu: "500m", memory: "256Mi" }
```

## Service YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-svc
spec:
  type: ClusterIP        # or NodePort, LoadBalancer
  selector:
    app: myapp
  ports:
    - port: 80
      targetPort: 3000
```

## Scaling & Updates

```bash
kubectl scale deployment myapp --replicas=5
kubectl set image deployment/myapp myapp=myapp:2.0   # rolling update
kubectl rollout status deployment/myapp
kubectl rollout history deployment/myapp
kubectl rollout undo deployment/myapp                # rollback
kubectl autoscale deployment myapp --min=2 --max=10 --cpu-percent=80
```

## Config & Secrets

```bash
kubectl create configmap app-config --from-literal=KEY=value
kubectl create secret generic db-secret --from-literal=password=secret
kubectl get configmaps
kubectl get secrets
```

```yaml
# reference in a pod
env:
  - name: PASSWORD
    valueFrom:
      secretKeyRef:
        name: db-secret
        key: password
```

## Namespaces

```bash
kubectl get namespaces
kubectl create namespace dev
kubectl apply -f app.yaml -n dev
kubectl config set-context --current --namespace=dev
```

## Debugging

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl describe pod <name>               # check Events section
kubectl port-forward svc/myapp 8080:80    # access locally
kubectl top pods                          # resource usage
kubectl get pod <name> -o yaml            # full manifest
```

---

[🔝 Back to README](../README.md)
