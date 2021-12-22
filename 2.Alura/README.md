# 1.Alura

### Kubernetes (ORQUESTRADOR DE CONTAINERS)
<br />

**Anotações**<br>
    *ReplicaSet = Mantém o número de pods Ready equivalente ao Desired. Caso um pod dê erro o mesmo é substituído automaticamente.*<br>
    *Deployment = Camada acima de um ReplicaSet. Quando cria um Deployment automaticamente cria-se um ReplicaSet. Permite controle de versionamento das imagens e pods.
    Mais comum é utilizado o Deployment ao invés do ReplicaSet.*<br>
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

**Conhecendo Deployments**<br>
*PowerShell*
```
---
Criar nginx-deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-eployment
spec:
  replicas: 3
  template:
    metadata:
      name: nginx-pod
      labels:
        app: nginx-pod
    spec:
      containers:
        - name: nginx-container
          image: nginx:stable
          ports:
            - containerPort: 80
  selector:
    matchLabels:
      app: nginx-pod
---

> kubectl apply -f .\nginx-deployment.yaml
> kubectl get deployments
> kubectl rollout history deployment nginx-deployment (mostra a versão atual)

---
Alterar nginx-deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-eployment
spec:
  replicas: 3
  template:
    metadata:
      name: nginx-pod
      labels:
        app: nginx-pod
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
          ports:
            - containerPort: 80
  selector:
    matchLabels:
      app: nginx-pod
---

> kubectl apply -f .\nginx-deployment.yaml --record
> kubectl rollout history deployment nginx-deployment (mostra a nova versão após ter alterado para latest)
> kubectl annotate deployment nginx-deployment kubernetes.io/change-cause="Definindo a imagem com a versão latest"
> kubectl rollout history deployment nginx-deployment
> kubectl undo deployment nginx-deployment --to-revision=1 (retorna o deployment para a versão 1 conforme o history deployment)

```
<br />


*http://localhost:30001* (admin, admin) - Insere notícia<br>
*http://localhost:30000* - Visualiza notícia

***Resultado final:***<br> <img src="https://github.com/fabiokerber/lab/blob/main/images/211220211544.PNG">
<br />