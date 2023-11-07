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

> kubectl get pods <pods-name> -o yaml 

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

- Volume type'lari var. Burada hostPath ve emptyDir tipleri gorulecek. 

- hosthPath nod'un icinde olusur. emptyDir pod'a baglaniyor. Her ikisi de best practice kullanilan tipler degil. 

- configMap: The configMap resource provides a way to inject configuration data, or shell commands and arguments into a Pod.

- PerisitentVolume: is a piece of storage in the cluster that has been provided by an administrator o dynamically provisioned using Storage Classes.

- Acces Modes
- ReadWriteOnce: read-write by a single node
- ReadOnlyMany: read-only by many nodes
- ReadWriteMany: read-write by many nodes
- ReadWriteOncePod: read-write only one pod in the cluster

- PersistentVolumeClaims: is a request for storage by a user. It is similar to a Pod.Pods consume node resources and PVCs consume PV resources. Pods can request specific levls of resources (CPU and Memory). Claims can request specific size and acces modes. 
once a suitable PV is found, it is bound to a PVC.

- PV, PVC ve StorageClass birbir ile baglantili hususlar.

- StorageClass objesi ile ilgili bili icin dokumentasyona bakiniz.

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

> k get pv -o wide
NAME           CAPACITY  ACCESS MODES  RECLAIM POLICY  STATUS     CLAIM  STORAGECLASS  REASON   AGE   VOLUMEMODE
clarus-pv-vol  5Gi       RWO           Retain          Available         manual                 78s   Filesystem

- STATUS available demek hazir ancak bagli degil. Bu daha PVC'ye baglanacak.

- RECLAIM POLICY Retain: Bir PVC yi sildigin zaman bu PV'yi silme anlamina geliyor. Ancak data sikinir. Data'nin silinmememsi icin baska bir policy uygulanmali.

- Create a `PersistentVolumeClaim.yaml` file using the following content to create a `PersistentVolumeClaim` and explain fields.

- code PersistentVolumeClaim.yaml

## After we create the PersistentVolumeClaim, the Kubernetes control plane looks for a PersistentVolume that satisfies the claim's requirements. If the control plane finds a suitable `PersistentVolume` with the same `StorageClass`, it <binds> the claim to the volume. Look for details at [Persistent Volumes and Claims](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#introduction)

> kubectl get pvc clarus-pv-claim

> k get pv,pvc

-PV ve PVC baglandi ancak hehangi bir pod'a atanmadi.

- code PodWithPersisitentVolume.yaml

> kubectl exec -it clarus-pod -- /bin/bash 
- Open a shell to the container running in your Pod. POD'a baglanip bagladigimizvolume icindeki dosyayi kontrol ediyoruz.

> curl http://localhost/
- Verify that `nginx` is serving the `index.html` file from the `hostPath` volume.

> cd pv-data
> echo "Kubernetes Rocks!!!!" > index.html
- Log into the `kube-worker-1` node, change the `index.html`.

> kubectl exec -it clarus-pod -- /bin/bash
> curl http://localhost/
- Log into the `kube-master` node, check if the change is in effect.

> kubectl expose pod clarus-pod --port=80 --type=NodePort
- Expose the clarus-pod pod as a new Kubernetes service on master. bu sekilde pod'a servis atayip dis dunyadan volume icindeki yazan kodu gorebilirsin.

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

- Her ne kadar 2gb lik volume talep etmis olsan da sana 3 gblik tahsis edilir.

- Create another PersistentVolumeClaim file and name it `pv-claim-7g.yaml`.

- code pv-claim-7g.yaml

> kubectl apply -f pv-claim-7g.yaml

- Bu talep karsilanmaz. Cunku elimzide 6 GB volume var. 

## EmptyDir

- An `emptyDir volume` is first created when a Pod is assigned to a node, and exists as long as that Pod is running on that node.

- As the name says, the emptyDir volume is initially empty.

- When a Pod is removed from a node for any reason, the data in the emptyDir is deleted permanently.

_ Note : A container crashing does not remove a Pod from a node. The data in an emptyDir volume is safe across container crashes.

- Create an `nginx-volume.yaml` file for creating an nginx pod.

-code nginx-volume.yaml

> kubectl apply -f nginx-volume.yaml 

> kubectl exec -it nginx-pod -- bash
> root@nginx-pod:/# ls
bin   dev                  docker-entrypoint.sh  home  lib64  mnt  proc  run   srv  test  usr
 boot  docker-entrypoint.d  etc                   lib   media  opt  root  sbin  sys  tmp   var

> root@nginx-pod:/# cd test
> root@nginx-pod:/test# echo "Hello World" > hello.txt 
> root@nginx-pod:/test# cat hello.txt 
Hello World

- Log in the `kube-worker-1 ec2-instance` and remove the `nginx container`. Note that container is changed.

> sudo ctr --namespace k8s.io containers ls
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

<SECRETS> 

- Secrets (an object in Kubernetes) can contain user credentials required by Pods to access a database. For example, a database connection string consists of a username and password. You can store the username in a file ./username.txt and the password in a file ./password.txt on your local machine.

- The kubectl create secret command packages these files into a Secret and creates the object on the API server. The name of a Secret object must be a valid DNS subdomain name. Show types of secrets with opening : (Kubetnetes Secret Types)[https://kubernetes.io/docs/concepts/configuration/secret/]

> echo -n 'admin' > ./username.txt
> echo -n '1f2d1e2e67df' > ./password.txt

- Create files needed for the rest of the example.

> kubectl create secret --help

Outputs:
- Available Commands:
  docker-registry   Create a secret for use with a Docker registry
  generic           Create a secret from a local file, directory, or literal value
  tls               Create a TLS secret

> kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt

> kubectl create secret generic db-user-pass-key --from-file=username=./username.txt --from-file=password=./password.txt

## Special characters such as `$`, `\`, `*`, `=`, and `!` will be interpreted by your shell and require escaping. In most shells, the easiest way to escape the password is to surround it with single quotes (`'`). For example, if your actual password is S!B\*d$zDsb=, you should execute the command this way:

> kubectl create secret generic dev-db-secret --from-literal=username=devuser --from-literal=password='S!B\*d$zDsb='

> kubectl get secrets

> kubectl describe secrets/db-user-pass

- The commands kubectl get and kubectl describe avoid showing the contents of a secret by default. This is to protect the secret from being exposed accidentally to an onlooker, or from being stored in a terminal log.

## Creating a Secret manually

- You can also create a Secret in a file first, in JSON or YAML format, and then create that object. The name of a Secret object must be a valid DNS subdomain name. The Secret contains two maps: data and stringData. The data field is used to store arbitrary data, encoded using base64. The stringData field is provided for convenience, and allows you to provide secret data as unencoded strings.

- For example, to store two strings in a Secret using the data field, convert the strings to base64 as follows:

> echo -n 'admin' | base64

- The output is similar to: YWRtaW4=

> echo -n '1f2d1e2e67df' | base64

- The output is similar to: MWYyZDFlMmU2N2Rm

- Write a Secret that looks like this named `secret.yaml`:

- code secret.yaml

> kubectl apply -f ./secret.yaml

### Decoding a Secret

> kubectl get secret mysecret -o yaml

> echo 'MWYyZDFlMmU2N2Rm' | base64 --decode

- Decode the password field. - The output is similar to: 1f2d1e2e67df

### Using Secrets

- Firstly we get the parameters as plain environment variable.

- code mysecret-pod.yaml

> kubectl apply -f mysecret-pod.yaml

> kubectl exec -it secret-env-pod -- bash
> root@secret-env-pod:/data# echo $SECRET_USERNAME
admin
> root@secret-env-pod:/data# echo $SECRET_PASSWORD
1f2d1e2e67df

> kubectl delete -f mysecret-pod.yaml

- This time we get the environment variables from secret objects. <Modify> the mysecret-pod.yaml as below.

- code mysecret-pod-modify.yaml

> kubectl apply -f mysecret-pod-modify.yaml

### Consuming Secret Values from environment variables

- Inside a container that consumes a secret in an environment variables, the secret keys appear as normal environment variables containing the base64 decoded values of the secret data. This is the result of commands executed inside the container from the example above:

> kubectl exec -it secret-env-pod -- bash
> root@secret-env-pod:/data# echo $SECRET_USERNAME
admin
> root@secret-env-pod:/data# echo $SECRET_PASSWORD
1f2d1e2e67df

<CONFIGMAPS>

- A ConfigMap is a dictionary of configuration settings. This dictionary consists of key-value pairs of strings. Kubernetes provides these values to your containers. Like with other dictionaries (maps, hashes, ...) the key lets you get and set the configuration value.

- A ConfigMap stores configuration settings for your code. Store connection strings, public credentials, hostnames, environment variables, container command line arguments and URLs in your ConfigMap.

- ConfigMaps bind configuration files, command-line arguments, environment variables, port numbers, and other configuration artifacts to your Pods' containers and system components at runtime.

- ConfigMaps allow you to separate your configurations from your Pods and components. 

- ConfigMap helps to makes configurations easier to change and manage, and prevents hardcoding configuration data to Pod specifications.

- ConfigMaps are useful for storing and sharing non-sensitive, unencrypted configuration information.

- For the show case we will select a simple application that displays a message like this.

```text
Hello, Clarusway!
```

- We will parametrized the "Hello" portion in some languages.

- code deployment-demo.yaml

- code service-demo.yaml

> kubectl apply -f deployment-demo.yaml,service-demo.yaml

> kubectl get svc demo-service -o wide

- Let's see the message.

```bash
kubectl get svc demo-service -o wide
NAME           TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE     SELECTOR
demo-service   LoadBalancer   10.97.39.39   <pending>     80:30001/TCP   2m20s   app=demo

curl < worker-ip >:30001
Hello, Clarusway!
```

> kubectl delete -f .

- We have modified the application to take the greeting message as a parameter (environmental variable). So we will expose configuration data into the container’s environmental variables. Firstly, let's see how to pass environment variables to pods.

- Modify the `deployment-demo.yaml` as below.

- code deployment-demo-modified.yaml

> kubectl apply -f deployment-demo-modified.yaml

Let's see the message.

```bash
kubectl get svc demo-service -o wide
NAME           TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE     SELECTOR
demo-service   LoadBalancer   10.97.39.39   <pending>     80:30001/TCP   2m20s   app=demo

curl < worker-ip >:30001
selam, Clarusway!
```

> kubectl delete -f .

- This time we will expose configuration data into the container’s environmental variables. And,  we will create `ConfigMap` and use the `greeting` key-value pair as in the `deployment.yaml` file.

## Create and use ConfigMaps with `kubectl create configmap` command

There are three ways to create ConfigMaps using the `kubectl create configmap` command. Here are the options.

1. Use the contents of an entire directory with `kubectl create configmap my-config --from-file=./my/dir/path/`
   
2. Use the contents of a file or specific set of files with `kubectl create configmap my-config --from-file=./my/file.txt`
   
3. Use literal key-value pairs defined on the command line with `kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2`

### Literal key-value pairs

We will start with the third option. We have just one parameter. Greet with "Halo" in Spanish.

> kubectl create configmap demo-config --from-literal=greeting=Hola

> kubectl get configmap/demo-config -o yaml