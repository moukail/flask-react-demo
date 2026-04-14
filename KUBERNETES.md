Kubernetes
==========
#### install minikube
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

minikube config set rootless true
minikube start --driver=podman
minikube config view driver
minikube config set memory 4096
minikube config set cpus 2
minikube status

kubectl cluster-info
minikube dashboard

```
#### install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

echo "alias k=kubectl" >> ~/.bashrc
echo "source <(kubectl completion bash)" >> ~/.bashrc
source ~/.bashrc
```

#### install helm
```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4 | bash
```

#### Create pod
```bash
kubectl run nginx --image=docker.io/library/nginx:1.29.5-alpine3.23
kubectl expose pod nginx --port=8080 --type=NodePort
kubectl get pods
kubectl get svc
```

#### Create deployment
```bash
kubectl create deployment python-test-backend-deployment --image=docker.io/library/nginx:1.29.5-alpine3.23 --replicas=3 --dry-run=client -o yaml > ./kubeernetes/deployment_backend.yml

kubectl apply -f ./kubeernetes/deployment_backend.yml
kubectl get deployment
kubectl get replicaset

kubectl expose deployment python-test-backend-deployment --port=8080 --type=NodePort
```

#### Create service
```bash
kubectl apply -f ./kubeernetes/service.yml
kubectl get svc
```

#### Rolling update
```bash
kubectl set image deployment/python-test-backend-deployment python-test-backend-pod=docker.io/library/nginx:1.29.5-alpine3.23

kubectl annotate deployment python-test-backend-deployment kubernetes.io/change-cause="nginx version updated"
kubectl rollout status deployment/python-test-backend-deployment
kubectl rollout history deployment/python-test-backend-deployment
kubectl rollout undo deployment/python-test-backend-deployment
```

#### Autoscaling
```bash
kubectl autoscale deployment python-test-backend-deployment --cpu-percent=80 --min=3 --max=5
kubectl get hpa
```

#### Troubleshooting
```bash
kubectl logs python-test-backend-deployment
kubectl exec -it python-test-backend-deployment -- sh
kubectl describe pod python-test-backend-deployment
kubectl get events
```

#### Create secret
```bash
kubectl create secret generic db-password --from-literal=passwoed=supersecret
```

#### Create configmap
```bash
kubectl create configmap db-config --from-literal=POSTGRES_USER=admin --from-literal=POSTGRES_DB=mytv_db
```

#### Load Testing
```bash
curl -LO https://github.com/rakyll/hey/releases/download/v0.0.8/hey_linux_amd64
sudo install hey_linux_amd64 /usr/local/bin/hey

hey -z 120s -c 1000 http://python-test.apps.cluster-54754.example.com
```

Nginx Ingress Controller
========================
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.1/deploy/static/provider/cloud/deploy.yaml

helm install nginx-ingress oci://ghcr.io/nginx/charts/nginx-ingress --version 2.5.1

kubectl get ingressclass
kubectl apply -f ./kubeernetes/ingress.yml
kubectl get ingress
kubectl get svc ingress-nginx-controller -n ingress-nginx
kubectl delete ingress python-test
```
