# Moja-Box (v2)

Moja-Box aims to provide an easy way to get up and running with Mojaloop in an opinionated environment. It allows you to run a single node with a k3s cluster, helm, mojaloop, and postman already installed


>This project supercedes [moja-box-legacy](github.com/vessels-tech/moja-box), and tries to fix some of the issues with further dependence on terraform


## Getting Started


## Configuration

With Moja-Box, 


## References

https://medium.com/@marcovillarreal_40011/cheap-and-local-kubernetes-playground-with-k3s-helm-5a0e2a110de9


## Notes:

### Attempt 2: using multipass on mac os
```bash
brew cask install multipass
multipass launch --name k3s --mem 8G --disk 5G
multipass shell k3s

# inside k3s virtual machine
export INSTALL_K3S_VERSION=v1.0.1
curl -sfL https://get.k3s.io | sh -
sudo chmod 644 /etc/rancher/k3s/k3s.yaml

## verify install
kubectl get nodes
kubectl get po -n kube-system

#download helm
curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > install-helm.sh

#Make instalation script executable
chmod u+x install-helm.sh

#Install helm
./install-helm.sh
kubectl -n kube-system create serviceaccount tiller
kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller

# install mojaloop etc.
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml #this is needed otherwise helm commands will fail
helm init --service-account tiller
helm repo add mojaloop http://mojaloop.io/helm/repo/
# helm install dev -f ./ingress.values.yml --debug --namespace=mojaloop --repo=http://mojaloop.io/helm/repo mojaloop #v3
# helm install dev --debug --namespace=mojaloop --repo=http://mojaloop.io/helm/repo mojaloop #v3
# helm install -f ./ingress.values.yml --debug --namespace=mojaloop --name=dev --repo=http://mojaloop.io/helm/repo mojaloop
helm install --debug --namespace=mojaloop --name=dev --repo=http://mojaloop.io/helm/repo mojaloop

## this is broken! As the mojaloop helm charts fail on kubernetes 1.17!
```

### From plain ubuntu (didn't work, needs systemd):
```bash
# in dockerfile
apt-get update
apt-get install curl -y

# manual
apt-get install systemd -y
curl -sfL https://get.k3s.io | sh -
```

