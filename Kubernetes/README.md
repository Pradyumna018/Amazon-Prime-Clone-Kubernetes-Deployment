## Create Deployment and Service using Imperative method
### Deployment
```
apt install bat -y
```
```
kubectl create deploy amazon-prime-deploy --image==pradyumnam/amazon-prime-clone:v1 --port=80 --dry-run=client -o yaml > amazon-prime-deploy.yaml

```
```
kubectl apply -f amazon-prime-deploy.yaml
```
```
kubectl get deploy
```
### Expose Service
```
kubectl expose deploy amazon-prime-deploy --type=NodePort --port=80 --target-port=80 --name=amazon-prime-svc --dry-run=client -o yaml > amazon-prime-svc.yaml
```
```
kubectl apply -f amazon-prime-svc.yaml
```
```
kubectl get svc
```
### Port Forwarding
```
kubectl port-forward svc/amazon-prime-svc.yaml 80:80
```
---
### Installation Instruction For EKS
- Install AWS CLI
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```
- Install Kubectl
```
sudo snap install kubectl --classic
kubectl version --client
```

- Install EKS CTL
```
# for ARM systems, set ARCH to: `arm64`, `armv6` or `armv7`
ARCH=amd64
PLATFORM=$(uname -s)_$ARCH

curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"

# (Optional) Verify checksum
curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_checksums.txt" | grep $PLATFORM | sha256sum --check

tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz

sudo mv /tmp/eksctl /usr/local/bin

```

- Create EKS Cluster
```
eksctl create cluster \
  --name sen-devops \
  --region ap-south-1 \
  --nodegroup-name prime-1 \
  --nodes 1 \
  --node-type t3.medium \
  --managed

```
- Update kubeconfig to Access the EKS Cluster
```
aws eks --region ap-south-1 update-kubeconfig --name sen-devops
```
- Verify the Connection to the EKS Cluster
```
kubectl get nodes
```

-  Delete the EKS Cluster
```
eksctl delete cluster \
  --name sen-devops \
  --region ap-south-1
```
- Verify Deletion
```
eksctl get clusters --region ap-south-1
```