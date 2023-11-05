> kubectl cluster-info
> kubectl get nodes
- Check if Kubernetes is running and nodes are ready

> kubectl api-resources | grep pods
- Show the names and short names of the supported API resources as shown in the example.

> kubectl explain nodes
- Get the documentation of `Nodes` and its fields.

> kubectl explain pods
- Get the documentation of `Pods` and its fields.

> kubectl get pods -n kube-system      
- Show the list of the pods created for Kubernetes service itself. Note that pods of Kubernetes service are running on the master node.

> kubectl get pods -n kube-system -o wide   
- Show the details of pods in `kube-system` namespace. Note that pods of Kubernetes service are running on the master node.

> kubectl get pods -o wide --all-namespaces    
- Get the details of pods in all namespaces on master. Note that pods of Kubernetes service are running on the master node and also additional pods are running on the worker nodes to provide communication and management for Kubernetes service.

> kubectl run nginx-server --image=nginx  --port=80  
- Create and run a simple `Nginx` Server image.

> kubectl expose pod nginx-server --port=80 --type=NodePort 
- Expose the nginx-server pod as a new Kubernetes service on master.

> kubectl get service -o wide 
- Get the list of services and show the newly created service of `nginx-server`

> kubectl delete service nginx-server
> kubectl delete pods nginx-server

<POD 

> code mypod.yaml
- yaml dosyasi repoda yaml-files dosyasinda.

> kubectl create -f mypod.yaml 
- Create a pod with `kubectl create` command.

> kubectl get pods
- List the pods.

> kubectl get pods -o wide
- List pods in `ps output format` with more information (such as node name).

> kubectl describe pods/nginx-pod
- Show details of pod.

> kubectl get pods/nginx-pod -o yaml
- Show details of pod in `yaml format`.

> kubectl delete -f mypod.yaml veya - kubectl delete pod nginx-pod
- Delete the pod.

<REPLICASET 

> code myreplicaset.yaml 
- yaml dosyasi repoda yaml-files dosyasinda.

> kubectl explain replicaset
- Get the documentation of `replicasets` and its fields.

> kubectl apply -f myreplicaset.yaml
- Create the replicaset with `kubectl apply` command.

<PODSELECTOR

The .spec.selector field is a label selector. 

The .spec.selector field and .spec.template.metadata field must be same. 
There are additional issues related this subject like louse coupling, but we discuss this on the service object. 

<DEPLOYMENTS

# code myreplicaset.yaml 
- yaml dosyasi repoda yaml-files dosyasinda.

> kubectl apply -f mydeployment.yaml
- Create the deployment with `kubectl apply` command.

> kubectl get pods -w
- Canli olarak podlari ekranda calisip calismadigini goruyorsun. Bir podu sildigin zaman osutugunu canli olarak izleyebilirisin.

> kubectl logs <pod-name>
- Print the logs for a container in a pod.

> kubectl logs <pod-name> -c <container-name>
- If there is a multi-container pod, we can print logs of one container.

> kubectl exec <pod-name> -- date
> kubectl exec <pod-name> -- cat /usr/share/nginx/html/index.html
> kubectl exec <pod-name> -- ls
- Execute a command in a container.

> kubectl exec -it <pod-name> -- bash
- Open a bash shell in a container.

> kubectl scale deploy nginx-deployment --replicas=5
- Scale the deployment up to five replicas. 

> kubectl get deploy,rs,po -l app=container-info
- List the `Deployment`, `ReplicaSet` and `Pods` of `clarus-deploy` deployment using a label and note the name of ReplicaSet.

> kubectl set image deploy <deployment-name> <podsname>=<new-image-name>
# kubectl set image deploy clarus-deploy container-info=clarusway/container-info:2.0
- Upgrade image.

> kubectl rollout history deploy <deployment-name> 
# kubectl rollout history deploy clarus-deploy
- View previous rollout revisions.

> kubectl rollout history deploy clarus-deploy --revision=1
> kubectl rollout history deploy clarus-deploy --revision=2
- Display details about the revisions.

> kubectl edit deploy/clarus-deploy
- Upgrade image with kubectl edit commands.

> kubectl rollout undo deploy clarus-deploy --to-revision=1
- Rollback to `revision 1`.

> kubectl rollout history deploy clarus-deploy
> kubectl rollout history deploy clarus-deploy --revision=2
> kubectl rollout history deploy clarus-deploy --revision=2
> kubectl rollout history deploy clarus-deploy --revision=4
-revision 1'e geri dondugumuzde artik revision 4 olur. revision1 gider.

<NAMESPACE

> kubectl get namespace
- List the current namespaces in a cluster using and explain them. *Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called `namespaces`.*

- Kubernetes cluster icerisinde sanal cluster olusturmak icin kullanilan bir object. Bir nevi VPC gibi dusunebiliriz.

### default
# The default namespace for objects with no other namespace

### kube-system
# The namespace for objects created by the Kubernetes system

### kube-public
# This namespace is created automatically and is readable by all users (including those not authenticated). This >namespace is mostly reserved for cluster usage, in case that some resources should be visible and readable >publicly throughout the whole cluster. The public aspect of this namespace is only a convention, not a >requirement.

### kube-node-lease
# This namespace for the lease objects associated with each node which improves the performance of the node  heartbeats as the cluster scales.

> kubectl get ns
- komutun sonucundaki cikti bu sekilde olmali.
default           Active   4h37m
kube-flannel      Active   4h37m
kube-node-lease   Active   4h37m
kube-public       Active   4h37m
kube-system       Active   4h37m

> kubectl get -n pods kube-system  
- Kube-system name-space'indeki tum podlari gosterir

# code my-namespace.yaml
- yaml dosyasi repoda yaml-files dosyasinda.

# Namespace objesinin deployment.yaml file'indaki yerini gormek icin web-flask.yaml dosyasina bak. metadata --> namespace: <namespace-name>

> kubectl apply -f ./my-namespace.yaml

- Alternatively, you can create namespace using below command:
> kubectl create namespace <namespace-name>

> kubectl create deployment default-ns --image=nginx  
- default namespace'te nginx imajindan default-ns deployment'unu olusturan komut
> kubectl create deployment clarus-ns --image=nginx -n=clarus-namespace  
- clarus-namespace'te nginx imajindan clarus-ns deployment'unu olusuran komut.
- Create pods in each namespace.

> kubectl get deployment -n clarus-namespace

> kubectl get deployment -o wide --all-namespaces
- List the all deployments.

> kubectl delete ns clarus-namespace
- Namespace'i silmek tehlike olabilir. Ns'i silmek icindeki butun objeleri silmek demektir. Yaml dosayasini sildiginizde dosyadaki eger ozel belirtilen namespace varsa o da silinir. 

<NETWORK-SERVICE

https://kubernetes.io/docs/concepts/services-networking/service/

FOUR NETWORKING PROBLEMS
1. Container to container
2. Pod to pod 
3. Pod to Service
4. External to service

# Service offer a single DNS entry for a containerized application managed by Kubernetes
# Service objesi tum cluster'da olusur. Herhangi bir node'a ait degildir. Service'in node'u yoktur.
# Service giris icindir, cikis icin degildir.
# baska bir serivce altindaki pod diger service ile iletisimi direkt kurar.
# Kube-proxy service objesini yonetir. Ip table'lari tutar. Bir nevi Route table gibidir.
# Each cluster node runs a daemon called kube-proxy.
# DaemonSet ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. Workwernode'da olusur. 
https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/
# Herbir node'da kube-proxy'yi olusturan obje.

# Dort tip service type var.

1. ClusterIP: Dis dunyadan ulasilmasin isteniyorsa
2. NodePort: Dis dunyadan erisebiliyorsun.
3. Load Balancer: Cloud provider olmali. 
4. ExternalName: Cluster icinde degilde disariya ulasmak istiyorsunuz. Video  1:02:00 izle.

- code for-curl.yaml

> k apply -f for-curl.yaml
> kubectl exec -it forcurl -- sh
/ # ping <podIP>  # ping atarsin
/ # ping 10.244.1.127
/ # curl <podIP:containerPort> # container'a ulasirsin
/ # ping 10.244.1.127:5000 
/# exit

> kubectl scale deploy web-flask-deploy --replicas=0
> kubectl scale deploy web-flask-deploy --replicas=3

- Bu durumda Pod'larin Ip'leri degismis oldu. 

> kubectl explain svc
- Get the documentation of `Services` and its fields.

### ClusterIp

- code web-svc.yaml

> kubectl apply -f web-svc.yaml
> kubectl describe svc web-flask-svc

> kubectl exec -it forcurl -- sh
/ # ping web-flask-svc
/ # curl web-flask-svc:3000 
- Her seferinde girdigimizde bizi farkli podlara yonlendiriyor. Load balancer gibi ayni.

> kubectl scale deploy web-flask-deploy --replicas=0
> kubectl scale deploy web-flask-deploy --replicas=3
-Podlarin IP'leri degisti ama service objesi sayesinde bundan etkilenmiyoruz.

### NodePort

- yaml dosyasinda type: NodePort olarak degistir tekrar apply yap yaml dosyasini.

- Worker node'un IP'sini al, Service'in NodePort degerini browser'a gir karsina bir sayfa cikmali. We can visit `http://<public-node-ip>:30036` and access the application. 

- 34.204.180.128:32026 gibi

nodePort: 30001 ekleyebilirsin. Bu durumda  34.204.180.128:30001 olur.

### Endpoints

Servise bagli endpoint adreslerini tutuyor.

As Pods come-and-go (scaling up and down, failures, rolling updates etc.), the Service dynamically updates its list of Pods. It does this through a combination of the label selector and a construct called an Endpoint object.

Each Service that is created automatically gets an associated Endpoint object. This Endpoint object is a dynamic list of all of the Pods that match the Service’s label selector.

Kubernetes is constantly evaluating the Service’s label selector against the current list of Pods in the cluster. Any new Pods that match the selector get added to the Endpoint object, and any Pods that disappear get removed. This ensures the Service is kept up-to-date as Pods come and go.

> kubectl get ep -o wide

<VOLUME>

> kubectl explain pv

## Log into the `kube-worker-1` node, create a `pv-data` directory under home folder, also create an `index.html` file with `Welcome to Kubernetes persistence volume lesson` text and note down path of the `pv-data` folder.

```bash 
mkdir pv-data  && cd pv-data 
echo "Welcome to Kubernetes persistence volume lesson" > index.html
ls
pwd
/home/ubuntu/pv-data
```
- Create a `persistent-volume.yaml` file using the following content with the volume type of `hostPath` to build a `PersistentVolume` and explain fields.

- code persisiten-volume.yaml

- Create a `PersistentVolumeClaim.yaml` file using the following content to create a `PersistentVolumeClaim` and explain fields.

- code PersistentVolumeClaim.yaml

## After we create the PersistentVolumeClaim, the Kubernetes control plane looks for a PersistentVolume that satisfies the claim's requirements. If the control plane finds a suitable `PersistentVolume` with the same `StorageClass`, it binds the claim to the volume. Look for details at [Persistent Volumes and Claims](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#introduction)

> kubectl get pvc clarus-pv-claim

- code PodWithPersisitentVolume.yaml

> kubectl exec -it clarus-pod -- /bin/bash 
- Open a shell to the container running in your Pod.

> curl http://localhost/
- Verify that `nginx` is serving the `index.html` file from the `hostPath` volume.

> cd pv-data
> echo "Kubernetes Rocks!!!!" > index.html
- Log into the `kube-worker-1` node, change the `index.html`.

> kubectl exec -it clarus-pod -- /bin/bash
> curl http://localhost/
- Log into the `kube-master` node, check if the change is in effect.

> kubectl expose pod clarus-pod --port=80 --type=NodePort
- Expose the clarus-pod pod as a new Kubernetes service on master.

> kubectl get svc

- Check the browser (`http://<public-workerNode-ip>:<node-port>`) that clarus-pod is running.

## Binding PV to PVC

- Create a `pv-3g.yaml` file using the following content with the volume type of `hostPath` to build a `PersistentVolume`.

- code pv-3g.yaml

> kubectl apply -f pv-3g.yaml

- Create a `pv-6g.yaml` file using the following content with the volume type of `hostPath` to build a `PersistentVolume`.

- code pv-6g.yaml

> kubectl apply -f pv-6g.yaml

- Create a `pv-claim-2g.yaml` file using the following content to create a `PersistentVolumeClaim`.

- code pv-claim-2g.yaml

> kubectl apply -f pv-claim-2g.yaml

- Create another PersistentVolumeClaim file and name it `pv-claim-7g.yaml`.

- code pv-claim-7g.yaml

> kubectl apply -f pv-claim-7g.yaml

## EmptyDir

- An `emptyDir volume` is first created when a Pod is assigned to a node, and exists as long as that Pod is running on that node. 
- As the name says, the emptyDir volume is initially empty. 
- When a Pod is removed from a node for any reason, the data in the emptyDir is deleted permanently.

_ Note : A container crashing does not remove a Pod from a node. The data in an emptyDir volume is safe across container crashes.

- Create an `nginx-volume.yaml` file for creating an nginx pod.

-code nginx-volume.yaml

> kubectl apply -f nginx.yaml 

> kubectl exec -it nginx-pod -- bash
> root@nginx-pod:/# ls
bin   dev                  docker-entrypoint.sh  home  lib64  mnt  proc  run   srv  test  usr
boot  docker-entrypoint.d  etc                   lib   media  opt  root  sbin  sys  tmp   var

> root@nginx-pod:/# cd test
> root@nginx-pod:/test# echo "Hello World" > hello.txt 
> root@nginx-pod:/test# cat hello.txt 
Hello World

> sudo ctr --namespace k8s.io containers ls

- Log in the `kube-worker-1 ec2-instance` and remove the `nginx container`. Note that container is changed.

- See the running containers

> sudo ctr --namespace k8s.io tasks rm -f <container-id> 

- Stop the running containers

> sudo ctr --namespace k8s.io containers delete <container-id> 
- Delete the running containers

- Log in the kube-master ec2-instance again and connect the nginx-pod. See that test folder and content are there.

> kubectl exec -it nginx-pod -- bash
> root@nginx-pod:/# ls
bin   dev                  docker-entrypoint.sh  home  lib64  mnt  proc  run   srv  test  usr
boot  docker-entrypoint.d  etc                   lib   media  opt  root  sbin  sys  tmp   var
> root@nginx-pod:/# cd test/
> root@nginx-pod:/test# cat hello.txt 
Hello World