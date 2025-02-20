# kubernetes notes 

at sometime you will be taken at completely different project, and after a while
you may find yourself to quickly remind a tool, but you forget it, so this is the
kind of note that will give you that quick spark! 

# install kubectl
best way to learn kubernetes, install it on ubuntu 

step-1
`sudo snap install kubectl --classic`    
(snap is a software packaging system developed by Canonica )

step-2
download minikube
`wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 -O minikube`

step-3
make the binary executable
`chmod 755 minikube` 

step-4
make the binary to run from anywhere
`sudo mv minikube /usr/local/bin/`

step-5 
verify installation
`minikube version`

step-6
install docker 
`curl https://get.docker.com > dockerinstall && chmod 777 dockerinstall && ./dockerinstall`

step-7
start minikube
`minikube start --force`    (why force, incase if you docker is running with root, as this for learing, it is ok, but on production, do not use it)

# pod 
pod: represents containers, where your application will run. It is logical grouping, horizontal scalling is generally discuraged. 
it is the smallest deployable unit.

create a .yaml definition file and run it. In the definition fiel, `kind: Pod` this is one way
of creating a pod. Will see also other way to create pods. 

`$ kubectl create -f create_pod.yaml`

# replication controllers/replica sets 
manages the number of containers of your application run in pods. They ensure that and instance of an image is being run with the specific number of copies. 

# replicaset 
this is the updated way to run pods. 


# kubernets on digitalocean

go head and create digitalocean kubernetes.
you will be managing kubernetes from your local machine 
and for that you need to install DigitalOcean's CLI (doctl) 

# install doctl (Digitalocean CLI)
`sudo snap install doctl`

# authenticate your local doctl with DigitalOcean
before authentiating your local doctl, you need create api token in your digitalOcean
control panel. 
`doctl auth init` 

#  doctl permission
 as we are using snap version of doctl we need to set permission
 `sudo snap connect doctl:kube-config`  or `sudo chown $USER:$USER ~/.kube/config`

# install kubectl 
we are to use kubectl from our localmachine to manage cloud digitalOcean kubernets
`sudo snap install doctl`

# kubectl needs kubeconfig File 
kubectl, uses a configuration file (~/.kube/config) to connect to a remote Kubernetes API server.
before working with kubectl, locally you need kubeconfig file that comes from digitalOcean clud 
it looks by defautl at ~/.kube/config

for the kubeconfig File you have two options
1. manually download from digitialOcean control panel. After download 
`mkdir -p ~/.kube`
`mv /path/to/your-downloaded-config.yaml ~/.kube/config`

2. download the kubeconfig using doctl
`doctl kubernetes cluster kubeconfig save <your-cluster-name>`

# check kubernetes 
`kubectl get node` 

# Deployment 
Create a Deployment YAML File (deployment.yaml)

apiVersion: apps/v1
kind: Deployment
metadata:
  name: go-api
  labels:
    app: go-api
spec:
  replicas: 3
  selector:
    matchLabels:
      app: go-api
  template:
    metadata:
      labels:
        app: go-api
    spec:
      containers:
      - name: go-api
        image: awet/go-api:latest 
        ports:
        - containerPort: 8080
   
# Apply the deployment
`kubectl apply -f deployment.yaml` and this deploys it. 

# Verify pods are running 
`kuectl get pods` 

# Expose our Go API Using a LoadBalancer
Create a Service YAML File (service.yaml)

apiVersion: v1
kind: Service
metadata:
  name: go-api-service
spec:
  selector:
    app: go-api
  type: LoadBalancer
  ports:
    - protocol: TCP
      port: 80    # Exposed port on the LoadBalancer
      targetPort: 8080  # Port inside the container

# deploy the service 
`kubectl apply -f service.yaml`

# verify the service 
`kubectl get svc`  The EXTERNAL-IP will be the public IP assigned by DigitalOcean.

# Verify of logs of the Go API
`kubectl logs -f <pod-name>`

# Enter a running container
`kubectl exec -it <pod-name> -- /bin/sh`

# Scale 
`kubectl scale deployment go-api --replicas=5`