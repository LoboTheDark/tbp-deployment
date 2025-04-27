# tbp-deployment
- install kubectl 
- install k3d: curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
- create cluster: 
  - k3d cluster create tbp-cluster --api-port 192.168.86.49:6445 --k3s-arg "--tls-san=192.168.86.49@server:0" -p "8080:80@loadbalancer" -p "8443:443@loadbalancer"
  - 8080 and 8443 because if it runs on my homeserver port 80 is taken by casaOS
    
- apply all yml files:
  - kubectl apply -f backend.yml
  - kubectl apply -f frontend.yml
  - kubectl apply -f postgres-pvc.yaml
  - kubectl apply -f postgres-deployment.yaml
  - kubectl apply -f postgres-service.yaml
  - kubectl apply -f auth.yml

- check if pods and srv are running
  - kubectl get pods
  - kubectl get svc
 
- copy cluster config to local config: k3d kubeconfig get tbp-cluster > ~/.kube/config
  - Use this to connecto to cluster via lens or other tools: cat ~/.kube/config
 
- If everything works, connect to frontend via http://192.168.86.49:8080/

- Postgres files are saved in hostsystem here:
  - /var/lib/rancher/k3s/storage/

  
# HELM chart for entire deployment

- install helm

- helm repo add grafana https://grafana.github.io/helm-charts
- helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

- helm repo update

tbp-cluster-stack/
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── backend-deployment.yaml
│   ├── frontend-deployment.yaml
│   ├── auth-deployment.yaml
│   ├── postgres-deployment.yaml
│   ├── postgres-pvc.yaml
│   ├── postgres-service.yaml
│   ├── ingress-grafana.yaml
│   ├── ingress-prometheus.yaml
│   ├── ingress-loki.yaml
│   ├── ingress-tempo.yaml
├── charts/
    └── (automatically via helm dependcy update)

- Some commands
  - helm dependency update ./tbp-cluster-stack
  - helm install my-stack ./tbp-cluster-stack
  - helm uninstall my-stack
  - helm upgrade tbp-stack ./tbp-cluster-stack


