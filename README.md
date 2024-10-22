
# Terminal Command History

## 1. Creating and Managing Kubernetes Cluster with Kind

- Clear terminal:
  ```bash
  clear
  ```

- Create a 3-node Kubernetes cluster using Kind:
  ```bash
  cd kind-cluster/
  ```
# Install Kind & Kubectl & Docker
- Install Docker,Kind & Kubectl 
  ```bash
  # Change to executable permit to this shell-script files
  ./install_docker
  ./install_kind.sh
  ./install_kubectl.sh
  ```
- Create a 3-node Kubernetes cluster using Kind:
  ```bash
  cd kind-cluster/
  kind create cluster --config=config.yml --name=mycluster
  ```

- Check cluster information:
  ```bash
  kubectl cluster-info --context kind-kind
  kubectl get nodes
  kind get clusters
  ```

---

## 2. Installing kubectl

- Download `kubectl` for managing Kubernetes clusters:
  ```bash
  curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
  chmod +x ./kubectl
  sudo mv ./kubectl /usr/local/bin
  kubectl version --short --client
  ```

---

## 3. Managing Docker and Kubernetes Pods

- Check Docker containers running:
  ```bash
  docker ps
  ```

- List all Kubernetes pods in all namespaces:
  ```bash
  kubectl get pods -A
  ```
---

## 4. Installing Argo CD

- Create a namespace for Argo CD:
  ```bash
  kubectl create namespace argocd
  ```

- Apply the Argo CD manifest:
  ```bash
  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
  ```

- Check services in ArgoCD namespace:
  ```bash
  kubectl get svc -n argocd
  ```

- Expose Argo CD server using NodePort, change service clasterIP to NodePort:
  ```bash
  kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
  ```

- Forward ports to access Argo CD server:
  ```bash
  kubectl port-forward -n argocd service/argocd-server 8443:443 --address=0.0.0.0 &       #change the port of service according to your nodeport 
  ```
# Argo CD Initial Admin Password

- Retrieve Argo CD admin password:
  ```bash
  kubectl get secret -n argocd argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
  ```
---
## 6. Installing Kubernetes Dashboard

- Deploy Kubernetes dashboard:
  ```bash
  kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
  ```

- Create a token for dashboard access:
  ```bash
  kubectl -n kubernetes-dashboard create token admin-user
  ```

---


## 5. Deleting Kubernetes Cluster

- Delete the Kind cluster:
  ```bash
  kind delete cluster --name=mycluster
  ```
---
