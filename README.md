# kubernetes notes 

at sometime you will be taken at completely different project, and after a while
you may find yourself to quickly remind a tool, but you forget it, so this is the
kind of note that will give you that quick spark! 

# install kubectl
best way to learn kubernetes, install it on ubuntu 

step-1
sudo snap install kubectl --classic    
(snap is a software packaging system developed by Canonica )

step-2
download minikube
wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 -O minikube

step-3
make the binary executable
chmod 755 minikube 

step-4
make the binary to run from anywhere
sudo mv minikube /usr/local/bin/

step-5 
verify installation
minikube version

step-6
install docker 
curl https://get.docker.com > dockerinstall && chmod 777 dockerinstall && ./dockerinstall

step-7
start minikube
minikube start --force    (why force, incase if you docker is running with root, as this for learing, it is ok, but on production, do not use it)

# our first pod 
pod: represents containers, where your application will run. It is logical grouping, 
horizontal scalling is generally discuraged. 

create a .yaml definition file and run it. In the definition fiel, `kind: Pod` this is one way
of creating a pod. Will see also other way to create pods. 

`$ kubectl create -f create_pod.yaml`