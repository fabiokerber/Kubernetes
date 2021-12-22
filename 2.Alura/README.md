# 1.Alura

### Kubernetes (ORQUESTRADOR DE CONTAINERS)
<br />

**Anotações**<br>
    *ReplicaSet = Mantém o número de pods Ready equivalente ao Desired. Caso um pod dê erro o mesmo é substituído automaticamente.*<br>
    *Master = Control Plane*<br>
    &nbsp;&nbsp;&nbsp;&nbsp;*Gerenciar cluster*<br>
<br />

**Conhecendo ReplicaSets**<br>
*PowerShell*
```
https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
---
Criar portal-noticias-replicaset.yml

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: portal-noticias-replicaset
spec:
  template:
    metadata:
      name: portal-noticias
      labels:
        app: portal-noticias
    spec:
      containers:
        - name: portal-noticias-container
          image: aluracursos/portal-noticias:1
          ports:
            - containerPort: 80
          envFrom:
            - configMapRef:
                name: portal-configmap
  replicas: 3
  selector:
    matchLabels:
      app: portal-noticias (aqui é informado qual pod deve ser aplicado este ReplicaSet. deve ser igual ao labels mais acima)
---

> kubectl delete pods portal-noticias
> kubectl apply -f .\portal-configmap.yaml
> kubectl apply -f .\sistema-configmap.yaml
> kubectl apply -f .\sistema-noticias.yaml
> kubectl apply -f .\db-noticias.yaml
> kubectl apply -f .\portal-noticias-replicaset.yml
> kubectl get replicasets
> kubectl delete pods <NAME_pod> (irá deletar o pod, e o ReplicaSet irá recriá-lo)

```
<br />

**O primeiro pod**<br>
*PowerShell*
```

```
<br />


*http://localhost:30001* (admin, admin) - Insere notícia<br>
*http://localhost:30000* - Visualiza notícia

***Resultado final:***<br> <img src="https://github.com/fabiokerber/lab/blob/main/images/211220211544.PNG">
<br />