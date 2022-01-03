# Kubernetes

|Training     |Tools|
|-------------|-----------|
|`1.Alura`| Kubernetes: Pods, Services e ConfigMaps
|`2.Alura`| Kubernetes: Deployments, Volumes e Escalabilidade

Pré requisito:


Download Docker:<br>
&nbsp;&nbsp;&nbsp;&nbsp;https://www.docker.com/products/docker-desktop<br>
&nbsp;&nbsp;&nbsp;&nbsp;Settings > Kubernetes > Enable Kubernetes > Apply & Restart

Comandos WSL:<br>
&nbsp;&nbsp;&nbsp;&nbsp;https://docs.microsoft.com/pt-br/windows/wsl/install-manual 

Instalação Docker Ubuntu:<br>
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

Instalação kubectl Ubuntu:<br>
```
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
$ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
$ kubectl version --client
```

Instalação minikube Ubuntu:<br>
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