# 1.Alura

### Kubernetes (ORQUESTRADOR DE CONTAINERS)
<br />

**Anotações**<br>
    *Escalabilidade Horizontal = incremento de servidores adicionais para aumentar capacidade. Mais máquinas trabalhando em paralelo.*<br>
    *Master = Control Plane*<br>
    &nbsp;&nbsp;&nbsp;&nbsp;*Gerenciar cluster*<br>
    &nbsp;&nbsp;&nbsp;&nbsp;*Manter e atualizar o estado desejado*<br>
    &nbsp;&nbsp;&nbsp;&nbsp;*Receber e executar novos comandos*<br>
    *Node = Nodes*<br>
    &nbsp;&nbsp;&nbsp;&nbsp;*Executar as aplicações*<br>
    *kubectl = Se comunica direto com a API via ReST*<br>
    *pod = Contêm um ou mais containers*<br>
    *Services*<br>
    &nbsp;&nbsp;&nbsp;&nbsp;*Proveem IP's fixos para os pods. Independente do IP, os pods se comunicam via DNS.*<br>
    &nbsp;&nbsp;&nbsp;&nbsp;*Proveem DNS para um ou mais pods.*<br>
    &nbsp;&nbsp;&nbsp;&nbsp;*Capaz de fazer balanceamento de carga.*<br>
    *ClusterIP (Services)*<br>
    &nbsp;&nbsp;&nbsp;&nbsp;*Serve apenas para viabilizar a comunicação INTERNA entre os diferentes pods dentro de um mesmo cluster.*
<br />

**Início**<br>
*PowerShell*
---
> kubectl get nodes
---
<br />

**O primeiro pod**<br>
*PowerShell*
---
> kubectl run nginx-pod --image=nginx:latest (cria um pod de nome nginx-pod baseando-se na imagem mais recente do nginx)
> kubectl get nodes 
> kubectl get pods --watch (visualiza em tempo real)
> kubectl describe pod nginx-pod (exibe diversas informações sobre o pod nginx-pod)
> kubectl edit pod nginx-pod (edita as configurações do pod)
---
<br />

**Criando pods de maneira declarativa**<br>
*PowerShell*
---
Criar primeiro-pod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: primeiro-pod-declarativo
spec:
  containers:
    - name: nginx-container
      image: nginx:latest

> kubectl apply -f primeiro-pod.yaml
> kubectl get pods

    image: nginx:latest

> kubectl apply -f primeiro-pod.yaml
> kubectl get pods --watch
---
<br />

**Iniciando o projeto**<br>
*PowerShell*
---
> kubectl delete pod nginx-pod
> kubectl delete -f primeiro-pod.yaml (deletando de forma declarativa. se baseia no metadata)

Criar portal-noticias.yaml

apiVersion: v1
kind: Pod
metadata:
  name: portal-noticias
spec:
  containers:
    - name: portal-noticias-container
      image: aluracursos/portal-noticias:1

> kubectl get pods --watch
> kubectl describe pod portal-noticias
> kubectl exec -it portal-noticias -- bash (conecta via ssh no container dentro do pod)
---
<br />

**Conhecendo services (SVC)**<br>
*PowerShell*
---
> kubectl get pods -o wide (exibe maiores informações sobre os pods em execução)
---
<br />

**Criando um Cluster IP**<br>
*PowerShell*
---

Criar arquivo pod-1.yaml

> apiVersion: v1
kind: Pod
metadata:
  name: pod-1
spec:
  containers:
    - name: container-pod-1
      image: nginx:latest
      ports:
        - containerPort: 80 (requisição )
---
<br />