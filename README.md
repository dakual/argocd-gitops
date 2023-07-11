## ARGOCD

Setup and prepare Minikube Kubernetes Cluster
```
minikube start -p gitops
eval $(minikube docker-env -p gitops)

# Enable the Ingress Addon for Minikube:
minikube addons enable ingress -p gitops
```

Argo CD Installation
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get pods -n argocd

# Patch the ArgoCD service from ClusterIP to a LoadBalancer:
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

Connecting to the Web Console
```
# Now with the minikube service list you can check the argocd service exposed:
minikube -p gitops service list | grep argocd

# Get ArgoCD password:
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

# Get ArgoCD URL:
minikube -p gitops service argocd-server -n argocd --url -p gitops
```

Apply the Application
```
kubectl apply -f app.yaml
```





