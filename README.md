# Kubernetes CheatSheet

## Kubernetes Basic

* **Create a cluster:**
    * Kubernetes coordinates a highly available cluster of computers that 
    are connected to work as a single unit.
    * Kubernetes automates the distribution and scheduling of application
     containers 
    * A Kubernetes cluster consists of two types of resources:
        * The **Master** coordinates the cluster
        * **Nodes** are the workers that run applications

* **Kubernetes Deployments**
    * The Deployment instructs Kubernetes how to create and update 
    instances of the application.
    * A Kubernetes Deployment Controller continuously monitors those instances
    * provides a self-healing mechanism to address machine failure or maintenance
    * `kubectl run name --image=username/name:tag --port=8080` deploy the app on a specific port
    * `kubectl get deployments` list deployments
    * `kubectl proxy` enables direct access to the API from terminals
    * `curl http://localhost:8001/version` check the access from terminal

* **Kubernetes Pods**    
    * A pod is created to host the application after deployment 
    * containers in a Pod share an IP Address and port space
    * `kubectl get pods` list of existing pods
    * `kubectl describe pods` see whats inside that leaning ad pulling

* **Kubernetes Node**
    * A pod always run on a **Node**
    * Every Kubernetes node runs at least:
        * Kubelet, a process responsible for communication between the
        Kubernetes Master and the node; it manages the Pods and the containers 
        running on a machine
        * A container runtime (like Docker) responsible for pulling the 
        container image from a registry, unpacking the container, and running 
        the application
* **Kubernetes Services**
    * Each Pod in a Kubernetes cluster has a unique IP address, even Pods on the same Node
    * A Service in Kubernetes is an abstraction which defines a logical set of Pods and a policy by which to access them. 
    * A Service is defined using YAML or JSON
    * Services can be exposed in different ways by specifying a **type**
        * `ClusterIP (default)` Exposes the Service on an internal IP in the cluster. This type makes the Service only reachable from within the cluster
        * `NodePort` Exposes the Service on the same port of each selected Node in the cluster using NAT. Makes a Service accessible from outside the cluster using 
        `<NodeIP>:<NodePort>`
        * `LoadBalancer` Creates an external load balancer in the current cloud (if supported) and assigns a fixed, external IP to the Service
        * `ExternalName` Exposes the Service using an arbitrary name (specified by externalName in the spec) by returning a CNAME record with the name
    * `kubectl get services` list running services 
    * `kubectl expose deploy <deploymentName> --port=8080 --type="NodePort"` create a new service and expose it to external traffic
    * `kubectl delete service <name>` delete service
    * `kubectl delete service -l run=<label>` delete service by label 
    
### minikube Command
* `minikube version`    ckeck minikube verion
* `minikube start`  start minikube
* `minikube start --kubernetes-version=v1.11.3` minikube specific version start
* `minikube delete` delete minikube 
* `minikube delete; minikube start --kubernetes-version=v1.11.3` minikube delete and start
* `minikube ip` service access ip from outside 
* `minikube service <name>` open services
-------------------------

### kubectl Command

* `kubectl version` ckeck kubeclt version
* `kubectl cluster-info` cluster information
* `kubectl get node` shows all node
* `kubectl proxy`   makes a connection between our host (the online terminal) and the Kubernetes cluster
* `kubectl run name --image=username/name:tag --port=8080` run the app on a specific port
* `kubectl get deployments` list deployments
* `kubectl logs $POD_NAME` anything that the application would normally send to STDOUT becomes logs for the container within the Pod
* `kubectl describe deployment` describe deployment 
* `kubectl describe pods` describe pods
* `kubectl describe pod $POD_NAME` describe specific pod
* `kubectl label pod $POD_NAME app=v1` label pod with app=v1
* `kubectl get pods -l app=v1` list of pods using label 
* `kubectl delete service <name>` delete service
* `kubectl delete service -l run=<label>` delete service by label

----------------------------------
### Extra

* `export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')` store the pod name to POD_NAME variable
* `echo Name of the Pod: $POD_NAME` print the pod name
* `export NODE_PORT=$(kubectl get services/<deploymentName> -o go-template='{{(index .spec.ports 0).nodePort}}')
` store Node Port to $NODE_PORT
*