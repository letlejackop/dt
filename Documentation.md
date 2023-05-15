# Simple Hello World App using node Deployment in k8s Cluster

## Prequesets
- docker installed
- minikube installed

### Confirm by running these commands
```
docker version  
minikube version
kubectl version

```



### 1. Start the cluster with command
```
minikube start --driver docker
```
![this](run%20docker.PNG)

### Make sure that docker is running!! or else this error will pop up.


### [optional for custom images] 2. This step is required when you have an image locally
```
& minikube -p minikube docker-env --shell powershell | Invoke-Expression
```
### Now any ‘docker’ command you run in this current terminal will run against the docker inside minikube cluster.

### Now you can ‘build’ against the docker inside minikube, which is instantly accessible to kubernetes cluster.

```
docker build -t my_image .
```

### for more information about this method and more methods to add images to minikube follow the link:

https://minikube.sigs.k8s.io/docs/handbook/pushing/

### 3. Create a yaml Configruation file named hello-world.yaml where there is an image ready to use from docker hub, with these settings:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment
spec:
  selector:
    matchLabels:
      app: app
  replicas: 2 
  template:
    metadata:
      labels:
        app: app
    spec:
      containers:
      - name: app
        image: letlejackop/hello-world:latest
        ports:
        - containerPort: 3000
---
apiVersion: v1
kind: Service
metadata:
  name: app-service
spec:
  type: NodePort
  selector:
    app.kubernetes.io/name: app
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
      nodePort: 30000
```

### then run these commands to create the service, the deployment and pods

```
kubectl apply -f hello-world.yaml
```
### To check the status of the cluster and details of what is running run:

```
kubectl get all
```
![this](cluster%20info.png)

### All the pods, services and deployment details will be displayed, now you can access the service in the browser by making a tunnel using the command:

```
minikube service app-service
```

### you can see more details in:
https://kubernetes.io/docs/home/

### Also see this helpful tutorial to help you understand more:

https://www.youtube.com/watch?v=s_o8dwzRlu4
