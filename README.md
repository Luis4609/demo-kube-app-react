# Documentation for Install KubeSphere on Linux 18.04(Hyper-V)

## PASO 1: Instalar docker 
 https://docs.docker.com/engine/install/ubuntu/

### 1.1: Set up the repository:  

    Update the apt package index and install packages to allow apt to use a repository over HTTPS:

    $ sudo apt-get update

    $ sudo apt-get install \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg-agent \
        software-properties-common

 Add Docker’s official GPG key:

>$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
>$ sudo apt-key fingerprint 0EBFCD88    //Verify
>$ sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"

### 1.2: Install Docker Engine

    Update the apt package index, and install the latest version of Docker Engine and containerd, or go to the next step to install a specific version:

     $ sudo apt-get update
     $ sudo apt-get install docker-ce docker-ce-cli containerd.io
	 
	sudo docker run hello-world //Verify 
	 
## PASO 2: Instalar Microk8s   
https://ubuntu.com/tutorials/install-a-local-kubernetes-with-microk8s#1-overview
https://microk8s.io/docs

### 2.1: Install your version for Microk8s
MicroK8s will install a minimal, lightweight Kubernetes you can run and use on practically any machine. It can be installed with a snap:

>$ sudo snap install microk8s --classic --channel=1.18

### 2.2: Join the group

MicroK8s creates a group to enable seamless usage of commands which require **admin privilege**. To add your current user to the group and gain access to the .kube caching directory, run the following two commands:

>1. sudo usermod -a -G microk8s $USER
>2. sudo chown -f -R $USER ~/.kube
>3. su - $USER

### 2.3: Check the status

MicroK8s has a built-in command to display its status. During installation you can use the --wait-ready flag to wait for the Kubernetes services to initialise:

>  microk8s status --wait-ready


### 2.4: Configure the firewall

>1. sudo ufw allow in on cni0 && sudo ufw allow out on cni0
>2. sudo ufw default allow routed


### 2.5: Enable addons

>microk8s enable dns dashboard storage


### 2.6:Accessing the Kubernetes dashboard:

>1. microk8s kubectl get all --all-namespaces
>2. token=$(microk8s kubectl -n kube-system get secret | grep default-token | cut -d " " -f1)
>3. microk8s kubectl -n kube-system describe secret $token


### 2.7: Access Kubernetes

> 1. microk8s kubectl get nodes
> 2. microk8s kubectl get services
> 3. alias kubectl='microk8s kubectl'     //Importante cambiar el alias 

## PASO 3 : INSTALAR OPENSSH Y OPENSSL

>sudo apt update
>sudo apt install openssh-server

>sudo apt-get update -y
>sudo apt-get install -y openssl

## PASO 4: Instalar  KubeSphere   
https://kubesphere.io/   Documentation/V.3.0/Minimal KUBESPHERE on KUBERNETES

>kubectl apply -f https://github.com/kubesphere/ks-installer/releases/download/v3.0.0/kubesphere-installer.yaml   
>kubectl apply -f https://github.com/kubesphere/ks-installer/releases/download/v3.0.0/cluster-configuration.yaml

### 4.1: Accesses the KubeSphere Dashboard


### Documentation:

- https://kubesphere.io/docs/quick-start/enable-pluggable-components/#installing-on-kubernetes
- https://kubesphere.io/docs/quick-start/wordpress-deployment/

- https://10.152.183.167  KUBERNETES DASHBOARD
- http://192.168.53.14:30880 KUBESPHERE
- http://192.168.53.14:31151 WORDPRESS
