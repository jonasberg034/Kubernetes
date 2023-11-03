# kubectl cluster-info
# kubectl get nodes
- Check if Kubernetes is running and nodes are ready

# kubectl api-resources | grep pods
- Show the names and short names of the supported API resources as shown in the example.

# kubectl explain nodes
- Get the documentation of `Nodes` and its fields.

# kubectl explain pods
- Get the documentation of `Pods` and its fields.

# kubectl get pods -n kube-system      
- Show the list of the pods created for Kubernetes service itself. Note that pods of Kubernetes service are running on the master node.

# kubectl get pods -n kube-system -o wide   
- Show the details of pods in `kube-system` namespace. Note that pods of Kubernetes service are running on the master node.

# kubectl get pods -o wide --all-namespaces    
- Get the details of pods in all namespaces on master. Note that pods of Kubernetes service are running on the master node and also additional pods are running on the worker nodes to provide communication and management for Kubernetes service.

# kubectl run nginx-server --image=nginx  --port=80  
- Create and run a simple `Nginx` Server image.

# kubectl expose pod nginx-server --port=80 --type=NodePort 
- Expose the nginx-server pod as a new Kubernetes service on master.

# kubectl get service -o wide 
- Get the list of services and show the newly created service of `nginx-server`

# kubectl delete service nginx-server
# kubectl delete pods nginx-server

> POD 

# code mypod.yaml
- yaml dosyasi repoda yaml-files dosyasinda.

# kubectl create -f mypod.yaml 
- Create a pod with `kubectl create` command.

# kubectl get pods
- List the pods.

# kubectl get pods -o wide
- List pods in `ps output format` with more information (such as node name).

# kubectl describe pods/nginx-pod
- Show details of pod.

# kubectl get pods/nginx-pod -o yaml
- Show details of pod in `yaml format`.

# kubectl delete -f mypod.yaml veya - kubectl delete pod nginx-pod
- Delete the pod.

> REPLICASET 

# code myreplicaset.yaml 
- yaml dosyasi repoda yaml-files dosyasinda.

# kubectl explain replicaset
- Get the documentation of `replicasets` and its fields.

# kubectl apply -f myreplicaset.yaml
- Create the replicaset with `kubectl apply` command.

> POD SELECTOR

The .spec.selector field is a label selector. 

The .spec.selector field and .spec.template.metadata field must be same. 
There are additional issues related this subject like louse coupling, but we discuss this on the service object. 

> DEPLOYMENTS

# code myreplicaset.yaml 
- yaml dosyasi repoda yaml-files dosyasinda.

# kubectl apply -f mydeployment.yaml
- Create the deployment with `kubectl apply` command.

# kubectl get pods -w
- Canli olarak podlari ekranda calisip calismadigini goruyorsun. Bir podu sildigin zaman osutugunu canli olarak izleyebilirisin.

# kubectl logs <pod-name>
- Print the logs for a container in a pod.

# kubectl logs <pod-name> -c <container-name>
- If there is a multi-container pod, we can print logs of one container.

# kubectl exec <pod-name> -- date
# kubectl exec <pod-name> -- cat /usr/share/nginx/html/index.html
# kubectl exec <pod-name> -- ls
- Execute a command in a container.

# kubectl exec -it <pod-name> -- bash
- Open a bash shell in a container.

# kubectl scale deploy nginx-deployment --replicas=5
- Scale the deployment up to five replicas. 

# kubectl get deploy,rs,po -l app=container-info
- List the `Deployment`, `ReplicaSet` and `Pods` of `clarus-deploy` deployment using a label and note the name of ReplicaSet.

# kubectl set image deploy clarus-deploy container-info=clarusway/container-info:2.0
- Upgrade image.

# kubectl rollout history deploy clarus-deploy
- View previous rollout revisions.

# kubectl rollout history deploy clarus-deploy --revision=1
# kubectl rollout history deploy clarus-deploy --revision=2
- Display details about the revisions.

# kubectl edit deploy/clarus-deploy
- Upgrade image with kubectl edit commands.

# kubectl rollout undo deploy clarus-deploy --to-revision=1
- Rollback to `revision 1`.

# kubectl rollout history deploy clarus-deploy
# kubectl rollout history deploy clarus-deploy --revision=2
# kubectl rollout history deploy clarus-deploy --revision=2
# kubectl rollout history deploy clarus-deploy --revision=4
-revision 1'e geri dondugumuzde artik revision 4 olur. revision1 gider.

# kubectl get namespace
- List the current namespaces in a cluster using and explain them. *Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called `namespaces`.*

>### default
>The default namespace for objects with no other namespace

>### kube-system
>The namespace for objects created by the Kubernetes system

>### kube-public
>This namespace is created automatically and is readable by all users (including those not authenticated). This >namespace is mostly reserved for cluster usage, in case that some resources should be visible and readable >publicly throughout the whole cluster. The public aspect of this namespace is only a convention, not a >requirement.

>### kube-node-lease
>This namespace for the lease objects associated with each node which improves the performance of the node  heartbeats as the cluster scales.

# code my-namespace.yaml
- yaml dosyasi repoda yaml-files dosyasinda.

# kubectl apply -f ./my-namespace.yaml

- Alternatively, you can create namespace using below command:
# kubectl create namespace <namespace-name>

# kubectl get ns

# kubectl create deployment default-ns --image=nginx  # default namespace'te olusan deployment
# kubectl create deployment clarus-ns --image=nginx -n=clarus-namespace  # clarus-namespace'te olusan deployment
- Create pods in each namespace.

# kubectl get deployment -n clarus-namespace

# kubectl get deployment -o wide --all-namespaces
- List the all deployments.