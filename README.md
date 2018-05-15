# Kubenetes_jenkins_pipielin cloudbee plugin
create 3 Ubuntu machine
install
1.Kubernetes-M
2.Kubernetes-C
2.Jenkins

run below commands Kubernetes-M

1.  ---------Deploying Docker
        sudo apt-get update

	sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo apt-key fingerprint 0EBFCD88

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt-get update

sudo apt-get install docker-ce


2.---------Deploying Kubectl
        sudo snap install kubectl --classic

3.----Deploying Kubelet and Kubeadm
        apt-get update && apt-get install -y apt-transport-https

        curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

        vi /etc/apt/sources.list.d/kubernetes.list
	
        deb http://apt.kubernetes.io/ kubernetes-xenial main
        

        apt-get update
	
        apt-get install -y kubelet kubeadm

4.----------------Using kubeadm to Create a Cluster
        kubeadm init
	
	-----------output of the above command last line-----
	
	kubeadm join 172.31.31.151:6443 --token y6otsr.d7xdmbsywzpx9yn0 --discovery-token-ca-cert-hash sha256:00e1264997a59ee4ffb73fcdca4d97b99cba60e44696f9abac5e0f56ab2ec662
	
	---execute it on the secod mahine
5.----Configuring kubelet
        
	mkdir -p $HOME/.kube
        
	sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
        
	sudo chown $(id -u):$(id -g) $HOME/.kube/config

6.  kubectl taint nodes --all node-role.kubernetes.io/master-
7.  kubectl apply -f https://git.io/weave-kube-1.6
8.  watch kubectl get pods --all-namespaces


------------------------------Kubernetes-C----------------------

1.  ---------Deploying Docker
        sudo apt-get update

	sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo apt-key fingerprint 0EBFCD88

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt-get update

sudo apt-get install docker-ce


2.---------Deploying Kubectl
        sudo snap install kubectl --classic

3.----Deploying Kubelet and Kubeadm
        apt-get update && apt-get install -y apt-transport-https

        curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

        vi /etc/apt/sources.list.d/kubernetes.list
        deb http://apt.kubernetes.io/ kubernetes-xenial main
        

        apt-get update
        apt-get install -y kubelet kubeadm

	kubeadm join 172.31.31.151:6443 --token y6otsr.d7xdmbsywzpx9yn0 --discovery-token-ca-cert-hash sha256:00e1264997a59ee4ffb73fcdca4d97b99cba60e44696f9abac5e0f56ab2ec662
	---execute it



-------------------Jenkins Machine and Jenkins configuration--------------

1.  ---------Deploying Docker
        sudo apt-get update

	sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo apt-key fingerprint 0EBFCD88

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt-get update

sudo apt-get install docker-ce


2.---------Deploying Kubectl
        sudo snap install kubectl --classic


give jenkins sudo permission in ubuntu machine

----------https://gist.github.com/hayderimran7/9246dd195f785cf4783d--------

to add jenkins As a sudo in ubuntu

after this copy Home/.kube/config file to /var/lib/jenkins/.kube/

use winSCP to copy files

in git my-calc-deployment.yml add ur repo name like sharadmagar/mycal

1. now create new jenkins job freestyl project
2. add git url to source code management
3. add docker build plugin manage jenkins->jenkins plugins->cloudbee dcoker build nd deploy 
   and install
4. add project name and provide ur credentials to it
5. now add build step Execute shell in jenkins project config
	
	kubectl apply -f my-calc-deploymnet.yaml

Save and build

u will get error while building docker


now in jenkins machine run below 2 steps
	
	usermod -a -G docker jenkins
	
	chmod 664 /var/run/docker.sock

and restart jenkins
	
	service jenkins restart

and now run ur jenkins job

.......done..................

u will see the ur image is the docker hub repo

now u can reuse the image for any of ur project
