# tbp-deployment
- install kubectl 
- install k3d: curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
- create cluster: k3d cluster create tbp-cluster \
  --api-port 192.168.86.49:6445 \
  --k3s-arg "--tls-san=192.168.86.49@server:0" \
  -p "8080:31000@loadbalancer" \
  -p "8081:31001@loadbalancer"
- apply all yml files:
  - kubectl apply -f backend.yml
  - kubectl apply -f frontend.yml
  - kubectl apply -f postgres-pvc.yaml
  - kubectl apply -f postgres-deployment.yaml
  - kubectl apply -f auth.yml

- check if pods and srv are running
  - kubectl get pods
  - kubectl get svc
 
- copy cluster config to local config: k3d kubeconfig get tbp-cluster > ~/.kube/config
  - Use this to connecto to cluster via lens or other tools: cat ~/.kube/config
 
- If everything works, connect to frontend via http://192.168.86.49:8080/

- Postgres files are saved in hostsystem here:
  - /var/lib/rancher/k3s/storage/
  
