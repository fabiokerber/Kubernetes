# AKS LAB

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

https://quay.io/repository/ansible/awx-operator?tab=tags&tag=latest
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