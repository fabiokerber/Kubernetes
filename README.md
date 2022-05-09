# Kubernetes

|Folder    |Tools|
|-------------|-----------|
|`1.Alura`| Kubernetes: Pods, Services e ConfigMaps
|`2.Alura`| Kubernetes: Deployments, Volumes e Escalabilidade
|`3.Alura`| Azure: gerencie Kubernetes com AKS e ACR

## Pré requisitos<br>


Download Docker:<br>
&nbsp;&nbsp;&nbsp;&nbsp;https://www.docker.com/products/docker-desktop<br>
&nbsp;&nbsp;&nbsp;&nbsp;Settings > Kubernetes > Enable Kubernetes > Apply & Restart

Comandos WSL:<br>
&nbsp;&nbsp;&nbsp;&nbsp;https://docs.microsoft.com/pt-br/windows/wsl/install-manual 

## Instalação Docker Ubuntu<br>
```
$ sudo apt-get remove docker docker-engine docker.io
$ sudo apt-get update
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
$ sudo apt-get update
$ sudo apt-get install docker-ce
$ sudo docker version
$ sudo usermod -aG docker $(whoami)
```

## Instalação kubectl Ubuntu<br>
```
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
$ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
$ kubectl version --client
```

## Instalação minikube Ubuntu<br>
```
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
$ sudo install minikube-linux-amd64 /usr/local/bin/minikube
$ minikube start --vm-driver=virtualbox
```

|Tool    |Link|
|-------------|-----------|
|`Docker`| https://desktop.docker.com/win/stable/amd64/Docker%20Desktop%20Installer.exe
|`Git BASH`| https://github.com/git-for-windows/git/releases/download/v2.34.1.windows.1/Git-2.34.1-64-bit.exe
|`WSL`| https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
|`kubectl`| https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msihttps://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
|`minikube`| https://minikube.sigs.k8s.io/docs/start/
|`VirtualBox`| https://download.virtualbox.org/virtualbox/6.1.30/VirtualBox-6.1.30-148432-Win.exe

## Comandos<br>
```
# kubectl cheat sheet
https://kubernetes.io/docs/reference/kubectl/cheatsheet/

# Version
kubectl version

# Connect aks
az aks get-credentials --resource-group $RESOURCE_GROUP --name $AKS_NAME --admin --overwrite-existing

# Set namespace
kubectl config set-context --current --namespace=NAMESPACE

# Set replica number
kubectl scale deployment DEPLOYMENT_NAME -n NAMESPACE --replicas=0
kb scale --replicas=1 deployment/DEPLOYMENT_NAME -n NAMESPACE

# Logs
kubectl logs POD_NAME -n NAMESPACE
kubectl logs -f POD_NAME -n NAMESPACE

# Describe
kubectl describe pod/POD_NAME -n NAMESPACE

# Apply object
kubectl apply -f FILE_NAME.yaml

# Delete object
kubectl delete -f FILE_NAME.yaml
kubectl delete namespace NAMESPACE
kubectl delete CustomResourceDefinition secretproviderclasses.secrets-store.csi.x-k8s.io -n NAMESPACE
kubectl delete CustomResourceDefinition secretproviderclasspodstatuses.secrets-store.csi.x-k8s.io -n NAMESPACE
kubectl delete ClusterRole secretproviderclasses-role -n NAMESPACE
kubectl delete ClusterRoleBinding secretproviderclasses-rolebinding -n NAMESPACE
kubectl delete CSIDriver secrets-store.csi.k8s.io -n NAMESPACE
kubectl delete ClusterRole secretprovidersyncing-role -n NAMESPACE
kubectl delete ClusterRoleBinding secretprovidersyncing-rolebinding -n NAMESPACE

# List
kubectl get all -n consent-payment    # List all objects
kubectl get nodes -w    # List nodes and watch it
kubectl get pods -o wide       

# List pods with details
kubectl logs POD_NAME --namespace=NAMESPACE --follow
kubectl get services --namespace=NAMESPACE

# Top
kubectl top nodes   # Top nodes
kubectl top pods # Top pods
watch -n 1 'kubectl top pods'

# Get yaml file
kubectl get deploy DEPLOYMENT_NAME -o yaml    # Get yaml

# Restart deployment without downtime
kubectl rollout restart deployment DEPLOYMENT_NAME -n NAMESPACE

# Exec
kubectl exec -it pods/POD_NAME sh
kubectl exec --stdin --tty POD_NAME -- /bin/bash
```

## K3s & Helm<br>
https://computingforgeeks.com/install-kubernetes-on-ubuntu-using-k3s/<br>
https://helm.sh/docs/intro/using_helm/<br>
https://github.com/helm/helm/releases<br>
https://blog.elo7.dev/subindo-uma-aplicacao-no-kubernetes-usando-helm/<br>
```
$ sudo su
# export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
# helm install nginx-ingress stable/nginx-ingress --namespace kube-system --set defaultBackend.enabled=false
# kubectl get pods -n kube-system -l app=nginx-ingress -o wide
# kubectl get svc -o wide
```

## AKS LAB<br>

# Instalar kustomize local
```
curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh" | bash
```

# AKS Sequência

1. RG
2. KV
3. ACR
4. AKS

# Trabalhar com Kubernetes a partir do Az Cli<br>
```
az account set --subscription "Subs - TU"
terraform workspace select tu
az aks get-credentials --resource-group azu-rg-tu-lab --name azu-aks-tu-lab-001 --admin --overwrite-existing
kubectl get nodes -A
kubectl get namespaces
```

# Attach ACR to AKS<br>
```
az aks update -n azu-aks-tu-lab-001 -g azu-rg-tu-lab --attach-acr azuacrtulab001

https://docs.microsoft.com/en-us/azure/aks/cluster-container-registry-integration?tabs=azure-cli
```

# Importanto a imagem awx-operator para o acr<br>
```
az acr import --name azuacrtulab001 --source quay.io/ansible/awx-operator:latest --image awx-operator:latest
```

# Listar azuacrtulab001 imagens<br>
```
az acr repository show --name azuacrtulab001 --repository awx-operator
```

# Listar azuacrtulab001 manifests<br>
``` 
az acr repository show-manifests --name azuacrtulab001 --repository awx-operator
```

# vi kustomization.yaml <br>
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  # Find the latest tag here: https://github.com/ansible/awx-operator/releases
  - github.com/ansible/awx-operator/config/default?ref=0.21.0

# Set the image tags to match the git version from above
images:
  - name: quay.io/ansible/awx-operator
    newTag: 0.21.0

# Specify a custom namespace in which to install AWX
namespace: awx
```
```
~/kustomize build . | kubectl apply -f -

kubectl get namespaces
kubectl get services --namespace=awx
kubectl get all -n awx
```

# Repositorio image awx operator<br>
https://quay.io/repository/ansible/awx-operator?tab=tags&tag=latest