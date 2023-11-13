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

> kubectl create deploy jonas-deploy --image=nginx --dry-run=client -o yaml > jonas-deploy.yaml
- jonas-deploy adinda nginx imajindan olusmasini istedigim deployment'un yaml dosyasini jonas-deploy.yaml dosyasina yazdirdim.

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

- code persistent-volume.yaml

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

> kubectl create secret generic dev-db-secret --from-literal=username=devuser --from-literal=password='S!B\*d$zDsb=' 

- literal metodu ile password olusturma. Direkt atama yapiyorsun.

> kubectl create secret generic my-secret --from-literal=username=devuser --from-literal=password='S!B\*d$zDsb=' --dry-run=client -o yaml > mysecret.yaml

- Bu yontem ile mysecret.yamldosyasi olusturabilirsin.

- Yukaridaki uc komut farkli. Uc farkli sekilde secret olusturuluyor. Detay icin video 49:00 dakikasi bknz.

## Special characters such as `$`, `\`, `*`, `=`, and `!` will be interpreted by your shell and require escaping. In most shells, the easiest way to escape the password is to surround it with single quotes (`'`). For example, if your actual password is S!B\*d$zDsb=, you should execute the command this way:

> kubectl get secrets

> kubectl describe secrets/db-user-pass

- The commands kubectl get and kubectl describe avoid showing the contents of a secret by default. This is to protect the secret from being exposed accidentally to an onlooker, or from being stored in a terminal log.

## Creating a Secret manually (dorduncu yontem=)

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

- This time we get the environment variables from secret objects. <modify> the mysecret-pod.yaml as below.

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

1. Use the contents of an entire directory with 
> kubectl create configmap my-config --from-file=./my/dir/path/
   
2. Use the contents of a file or specific set of files with 
> kubectl create configmap my-config --from-file=./my/file.txt
   
3. Use literal key-value pairs defined on the command line with 
> kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2

### Literal key-value pairs

We will start with the third option. We have just one parameter. Greet with "Halo" in Spanish.

> kubectl create configmap demo-config --from-literal=greeting=Hola

> kubectl get cm

> kubectl get configmap/demo-config -o yaml

> kubectl delete cm demo-config

- code configmap.yaml

- code deployment-demo-modified-config.yaml

> kubectl apply -f deployment-demo-modified-config.yaml,configmap.yaml,service-demo.yaml

> kubectl delete -f deployment-demo-modified-config.yaml,configmap.yaml,service-demo.yaml

## Configure all key-value pairs in a ConfigMap as container environment variables in POSIX format

- In case if you are using envFrom  instead of env  to create environmental variables in the container, the environmental names will be created from the ConfigMap's keys. If a ConfigMap  key has invalid environment variable name, it will be skipped but the pod will be allowed to start. 

Modify configmap.yaml file:

- code configmap-modified.yaml

- code deployment-demo-modifie-config-modified.yaml

Note the change as follows:

```yaml
...
          envFrom:
          - configMapRef:
              name: demo-config
```
### From a config file

- We will write the greeting key-value pair in a file in Norwegian and create the ConfigMap from this file.

> echo "greeting: Hei" > config  # Bu komut hatali olabilir. 

> kubectl create configmap demo-config --from-file=./config

- Check the content of the `configmap/demo-config`.
> kubectl get configmap/demo-config -o json

- Delete the ConfigMap.

## Using ConfigMaps as files from a Pod 

- code configmapPod.yaml

- code depolyment-with-configmapPod.yaml

- Volume and volume mounting are common ways to place config files inside a container. We are selecting `config` key from `demo-config` ConfigMap and put it inside the container at path `/config/` with the name `demo.yaml`.

> kubectl apply -f deployment-with-configmapPod.yaml,service-demo.yaml,configmapPod.yaml

> kubectl exec -it demo-5bf545f76-qppwf -- sh
ls
- config ismindeki dizini gor!
cd config | ls
- demo.yaml dosyasini gor!
cat demo.yaml
 

<MICROSERVICE> and <HPA>

1. We will deploy the `to-do` app first and look at some key points.
2. And then deploy the `php-apache` app and highligts some important points.
3. The Autoscaling in Kubernetes will be  demonstrated as a last step.

- code db-pv.yaml

-code db-pvc.yaml

- code db.deployment.yaml    for MongoDB database

- code db-service.yaml

```bash
 - Mongodb icin deployment olusturuken DockerHub'tan MongoDb imaji ile ilgili dokumantasyona bakilacak!!!!
 - ContainerPort, environment variables, Volume mounts gibi hususlar oradan alinacak
 ```

 > kubectl apply -f . 

 > kubectl describe svc db-service veya kubectl get endpoints
- db-service ile pod eslesmi onu kontrol ederiz. Endpoint olusmus. Demekki basarili.

- code web-deployment.yaml

- code web-service.yaml

### Note that this web app is connnected to MongoDB host/service via the `DBHOST` environment variable.

> kubectl apply -f .

### Deploy the second aplication

- code php-apache.yaml

> kubectl apply -f .

### Part 4 - Autoscaling in Kubernetes
### Create Horizontal Pod Autoscaler

> kubectl autoscale deployment php-apache --cpu-percent=50 --min=2 --max=10

> kubectl autoscale deployment web-deployment --cpu-percent=50 --min=3 --max=5 

or we can use yaml files.

- code hpa-php-apache.yaml
- code hpa-web.yaml

> kubectl apply -f .

- php-apache Pod number increased to 2, minimum number. 
- web-deployment Pod number increased to 3, minimum number. 
- The HPA line under TARGETS shows `<unknown>/50%`. The `unknown` means the HPA can't idendify the current use of CPU.

> k get hpa

> watch -n3 kubectl get service,hpa,pod -o wide 

### Install Metric Server

> kubectl delete -n kube-system deployments.apps metrics-server
- First Delete the existing Metric Server if any.

> wget https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.6.3/components.yaml
- Get the Metric Server form [GitHub](https://github.com/kubernetes-sigs/metrics-server/releases/tag/v0.6.3).

- Edit the file `components.yaml`. You will select the `Deployment` part in the file. Add the below line to `containers.args field under the deployment object`. Guvenlik engeline takilmamak icin asagidaki yazanlari componenets.yaml dosyasindak ilgili yere ej\kledik. 136 nci satirdan sonra.

```yaml
apiVersion: apps/v1
kind: Deployment
......
      containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=4443
        - --kubelet-insecure-tls
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        - --metric-resolution=15s
......	
```
> kubectl apply -f components.yaml

> kubectl -n kube-system get pods
- Verify the existace of `metrics-server` run by above command.

> kubectl top pods
- Verify `metrics-server` can access resources of the pods and nodes.
-  The top command allows you to see the resource consumption for nodes or pods.

> kubectl top nodes

### Increase load

> kubectl run -it --rm load-generator --image=busybox /bin/sh  

Hit enter for command prompt

> while true; do wget -q -O- http://<puplic ip>:<port number of php-apache-service>; done 
veya
>  while true; do wget -q -O- http://<puplic ip>:<port number of web-service> > /dev/null; done  # calisan dosayi dev/null dosyasina at, kaydetme.

<EKS>

## Part 1 - Creating the Kubernetes Cluster on EKS

3. Select ```Cluster``` on the left-hand menu and click on "Create cluster" button. You will be directed to the ```Configure cluster``` page:

    - Fill the ```Name``` and ```Kubernetes version``` fields. (Ex: MyCluster, 1.25)

    - On the ```Cluster Service Role``` field; give general description about why we need this role. 

    - Create EKS Cluster Role with ```EKS - Cluster``` use case and ```AmazonEKSClusterPolicy```.
                
        - EKS Cluster Role :                                                                                          
           - use case   :  ```EKS - Cluster``` 
           - permissions: ```AmazonEKSClusterPolicy```.

    - Select the recently created role, back on the ```Cluster Service Role``` field.

    - Proceed to the ```Secrets Encryption``` field. 
    
    - Activate the field, give general description about ```KMS Service``` and describe where we use those keys and give an example about a possible key.

    - Deactivate back the ```Secrets Encryption``` field and keep it as is.

    - Proceed to the next step (Specify Networking).

4. On the ```Specify Networking``` page's ```Networking field```:

    - Select the default VPC and a,b,c public subnets.

    - Select default VPC security or create one with <SSH> and <HTTPS> 

    - Proceed to ```Cluster Endpoint Access``` field.

    - Select ```Public and Private``` option on the field.

    - Proceed to the next step (Configure Logging).

5. On the ```Configure Logging``` page:

    - Proceed to the final step (Review and create).

6. On the ```Review and create``` page:

    - Create the cluster.

- Launch an AWS EC2 instance of Amazon Linux 2 AMI with security group allowing SSH.

- Connect to the instance with SSH.

- Update the installed packages and package cache on your instance.

> sudo yum update -y

> curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.25.7/2023-03-17/bin/linux/amd64/kubectl
- Download the Amazon EKS vended kubectl binary that is compatible with kubernetes cluster version. 
- Guncel download edilecek dosya burada https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html

> chmod +x ./kubectl

> mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin
- Copy the binary to a folder in your PATH. If you have already installed a version of kubectl, then we recommend creating a $HOME/bin/kubectl and ensuring that $HOME/bin comes first in your $PATH.

> echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
- (Optional) Add the $HOME/bin path to your shell initialization file so that it is configured when you open a shell.

> kubectl version --client
- After you install kubectl , you can verify its version with the command:

> aws configure
- Configure AWS credentials. Or you can attach `AWS IAM Role` to your EC2 instance.

- aws configuration

```bash
  aws configure
  AWS Access Key ID [None]: xxxxxxx
  AWS Secret Access Key [None]: xxxxxxxx
  Default region name [None]: us-east-1
  Default output format [None]: json
  ```

> aws eks list-clusters
- Verify that you can see your cluster listed, when authenticated


## Part 2 - Creating a kubeconfig file

> aws eks list-clusters
```bash
{
  "clusters": [
    "my-cluster"
  ]
}
```
> aws eks --region <us-east-1> update-kubeconfig --name <cluster_name>

- See the content of the $HOME directory including hidden files and folders. Find the ```config``` file inside ```.kube``` directory. Then see the content of the file.

> kubectl get svc

> kubectl get node

> kubectl get po -A

## Part 3 - Adding Worker Nodes to the Cluster

> aws eks describe-cluster --name <cluster-name> --query cluster.status
- Clustwer'in actie oldugunu gor.

3. On the cluster page, click on ```Compute``` tab and ```Add Node Group``` button.

4. On the ```Configure node group``` page:

- Click add node group

    - Give a unique name for the managed node group.

    - For the node's IAM Role, get to IAM console and create a new role with ```EC2 - Common``` use case having the policies of ```AmazonEKSWorkerNodePolicy, AmazonEC2ContainerRegistryReadOnly, AmazonEKS_CNI_Policy```.
    
        - ```Use case:    EC2 ```
        - ```Policies: AmazonEKSWorkerNodePolicy, AmazonEC2ContainerRegistryReadOnly, AmazonEKS_CNI_Policy```
    
    -  Proceed to the next page.

5. On the ```Set compute and scaling configuration``` page:
 
    - Choose the appropriate AMI type for Non-GPU instances. (Amazon Linux 2 (AL2_x86_64))

    - Choose ```t3.medium``` as the instance type.

    - Choose appropriate options for other fields. (3 nodes are enough for maximum, 2 nodes for minimum and desired sizes.)

    - Proceed to the next step.

6. On the ```Specify networking``` page:

    - Choose the subnets to launch your nodes.
    
    - Allow remote access to your nodes.
    
    - Select your SSH Key to for the connection to your nodes.
    
    - You can also limit the IPs for the connection.

    - Proceed to the next step. Review and create the ```Node Group```.
    
> kubectl get nodes --watch
> kubectl get node
> kubectl get pods -A

## Part 4 - Configuring Cluster Autoscaler

2. Create a policy with following content. You can name it as ClusterAutoscalerPolicy.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "autoscaling:DescribeAutoScalingGroups",
                "autoscaling:DescribeAutoScalingInstances",
                "autoscaling:DescribeLaunchConfigurations",
                "autoscaling:DescribeTags",
                "autoscaling:SetDesiredCapacity",
                "autoscaling:TerminateInstanceInAutoScalingGroup",
                "ec2:DescribeLaunchTemplateVersions"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}

3. Attach this policy to the IAM Worker Node Role which is already in use.

4. Deploy the ```Cluster Autoscaler``` with the following command.

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml
```

5. Add an annotation to the deployment with the following command.

> kubectl -n kube-system annotate deployment.apps/cluster-autoscaler cluster-autoscaler.kubernetes.io/safe-to-evict="false"

> kubectl -n kube-system edit deployment.apps/cluster-autoscaler

This command will open the yaml file for your editting. Replace <CLUSTER NAME> value with your own cluster name, and add the following command option ```--skip-nodes-with-system-pods=false``` to the command section under ```containers``` under ```spec```. Save and exit the file by pressing ```:wq```. The changes will be applied.

7. Find an appropriate version of your cluster autoscaler in the [link](https://github.com/kubernetes/autoscaler/releases). The version number should start with version number of the cluster Kubernetes version. For example, if you have selected the Kubernetes version 1.25, you should find something like ```1.25.0```.

> kubectl -n kube-system set image deployment.apps/cluster-autoscaler cluster-autoscaler=k8s.gcr.io/autoscaling/cluster-autoscaler:<YOUR-VERSION-HERE>

## Part 5 - Deploying a Sample Application

1. Create a `myapp.yml` file in your local environment with the following content.

- code myapp.yaml

> kubectl apply -f myapp.yaml

> kubectl -n my-namespace get svc

> kubectl describe service container-info-svc -n my-namespace

Show the warning: "Error creating load balancer (will retry): failed to ensure load balancer for service default/guestbook: could not find any suitable subnets for creating the ELB"

5. Go to this [link](https://aws.amazon.com/tr/premiumsupport/knowledge-center/eks-vpc-subnet-discovery/). Explain that it is necessary to tag selected subnets as follows:

- Key: kubernetes.io/cluster/<cluster-name>
- Value: shared

6. Go to the VPC service on AWS console and select "subnets". On the ```Subnets``` page select "Tags" tab and add this tag:

- Key: kubernetes.io/cluster/<cluster-name>
- Value: shared

- AWS console > Load balancer sekemsinden gir. Ilgili load balancer DNS'ini al. Sonuna:3000 ekle ve enter.la. Uygualamayi gor.

Soyle
- http://a0370133bd44847bab2edeaf01025431-64085939.us-east-1.elb.amazonaws.com:3000/

9. For scale up edit deployment. Change "replicas=30" in `myapp.yaml` file. Save the file.

> kubectl edit deploy container-info-deploy -n my-namespace

10. Watch the pods while creating. Show that some pods are pending state.

> kubectl get po -n my-namespace -w

11. Describe one of the pending pods. Show that there is no resource to run pods. So cluster-autoscaler scales out and create one more node.


> kubectl describe pod container-info-deploy-xxxxxx -n my-namespace
> kubectl get nodes

Ilk once myapp.yaml delete et. 

AWS Console > Cluster > Node-groups > <groupname> sil

AWS Console > Cluster > <clustername> sil

AWS Console > EC2 >  sil

<STORAGECLASS> <INGRESS>

# Dynamic Volume Provisionining and Ingress

## Part 1 - Installing kubectl and eksctl on Amazon Linux 2

### Install kubectl

- Launch an AWS EC2 instance of Amazon Linux 2 AMI with security group allowing SSH.

- Connect to the instance with SSH.

- Update the installed packages and package cache on your instance.

> sudo yum update -y

- Download the Amazon EKS vended kubectl binary.

> curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.25.7/2023-03-17/bin/linux/amd64/kubectl
- Guncel download edilecek dosya burada https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html

> chmod +x ./kubectl

- Copy the binary to a folder in your PATH. If you have already installed a version of kubectl, then we recommend creating a $HOME/bin/kubectl and ensuring that $HOME/bin comes first in your $PATH.

> mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin

> echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc

> kubectl version --client

### Install eksctl

- Download and extract the latest release of eksctl with the following command.

> curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

- Su siteden de 'eksctl'i inceleyebilirsin.
https://eksctl.io/getting-started/ 

- Move the extracted binary to /usr/local/bin.


> sudo mv /tmp/eksctl /usr/local/bin

> eksctl version

## Part 2 - Creating the Kubernetes Cluster on EKS

- If needed create ssh-key with command `ssh-keygen -f ~/.ssh/id_rsa`.

- Configure AWS credentials. Or you can attach `AWS IAM Role` to your EC2 instance.

> aws configure

- Create an EKS cluster via `eksctl`. It will take a while.

```bash
eksctl create cluster \
 --name cw-cluster \
 --region us-east-1 \
 --zones us-east-1a,us-east-1b,us-east-1c \
 --nodegroup-name my-nodes \
 --node-type t3a.medium \
 --nodes 2 \
 --nodes-min 2 \
 --nodes-max 3 \
 --ssh-access \
 --ssh-public-key  ~/.ssh/id_rsa.pub \  # bu komut ile key olusuyor. Podlara ulasmak icin gerekli olabilir. Burasi zorunlu degil.
 --managed
```

or 

> eksctl create cluster --region us-east-1 --zones us-east-1a,us-east-1b,us-east-1c --node-type t3a.medium --nodes 2 --nodes-min 2 --nodes-max 3 --name cw-cluster

> eksctl create cluster --help

## Part 3 - Dynamic Volume Provisionining

### The Amazon Elastic Block Store (Amazon EBS) Container Storage Interface (CSI) driver

- The Amazon Elastic Block Store (Amazon EBS) Container Storage Interface (CSI) driver allows Amazon Elastic Kubernetes Service (Amazon EKS) clusters to manage the lifecycle of Amazon EBS volumes for persistent volumes.

- The Amazon EBS CSI driver isn't installed when you first create a cluster. To use the driver, you must add it as an Amazon EKS add-on or as a self-managed add-on. 

- Install the Amazon EBS CSI driver. For instructions on how to add it as an Amazon EKS add-on, see Managing the [Amazon EBS CSI driver as an Amazon EKS add-on](https://docs.aws.amazon.com/eks/latest/userguide/managing-ebs-csi.html).

### Creating an IAM OIDC provider for your cluster

- To use AWS EBS CSI, it is required to have an AWS Identity and Access Management (IAM) OpenID Connect (OIDC) provider for your cluster. 

- Determine whether you have an existing IAM OIDC provider for your cluster. Retrieve your cluster's OIDC provider ID and store it in a variable.

> oidc_id=$(aws eks describe-cluster --name cw-cluster --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5)

- Determine whether an IAM OIDC provider with your cluster's ID is already in your account.

> aws iam list-open-id-connect-providers | grep $oidc_id

If output is returned from the previous command, then you already have a provider for your cluster and you can skip the next step. If no output is returned, then you must create an IAM OIDC provider for your cluster.

- Create an IAM OIDC identity provider for your cluster with the following command. Replace my-cluster with your own value.

> eksctl utils associate-iam-oidc-provider --region=us-east-1 --cluster=cw-cluster --approve

### Creating the Amazon EBS CSI driver IAM role for service accounts

- The Amazon EBS CSI plugin requires IAM permissions to make calls to AWS APIs on your behalf. 

- When the plugin is deployed, it creates and is configured to use a service account that's named ebs-csi-controller-sa. The service account is bound to a Kubernetes clusterrole that's assigned the required Kubernetes permissions.

#### To create your Amazon EBS CSI plugin IAM role with eksctl

- Create an IAM role and attach the required AWS managed policy with the following command. Replace cw-cluster with the name of your cluster. The command deploys an AWS CloudFormation stack that creates an IAM role, attaches the IAM policy to it, and annotates the existing ebs-csi-controller-sa service account with the Amazon Resource Name (ARN) of the IAM role.

> eksctl create iamserviceaccount \
  --name ebs-csi-controller-sa \
  --namespace kube-system \
  --cluster cw-cluster \
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
  --approve \
  --role-only \
  --role-name AmazonEKS_EBS_CSI_DriverRole

### Adding the Amazon EBS CSI add-on

#### To add the Amazon EBS CSI add-on using eksctl

- Run the following command. Replace cw-cluster with the name of your cluster, 111122223333 with your account ID, and AmazonEKS_EBS_CSI_DriverRole with the name of the IAM role created earlier.

```bash
eksctl create addon --name aws-ebs-csi-driver --cluster cw-cluster --service-account-role-arn arn:aws:iam::111122223333:role/AmazonEKS_EBS_CSI_DriverRole --force
```

- Firstly, check the StorageClass object in the cluster. 

> kubectl get sc (storageclass)

> kubectl describe sc/gp2

- code storage-class.yaml

> kubectl apply -f storage-class.yaml

> kubectl get sc

- Create a persistentvolumeclaim with the following settings and show that new volume is created on aws management console.

- code clarus-pv-claim.yaml

> kubectl apply -f clarus-pv-claim.yaml

> kubectl get pv,pvc

- Create a pod with the following settings.

- code pod-with-dynamic-storage.yaml

> kubectl apply -f pod-with-dynamic-storage.yaml

> kubectl exec -it test-aws -- bash

> kubectl delete storageclass aws-standard

> kubectl get storageclass

> k delete -f .

## Part 4 - Ingress

- Download the lesson folder from github.

The directory structure is as follows:

```text
ingress-yaml-files
├── ingress-service.yaml
├── php-apache
│   └── php-apache.yaml
└── to-do
    ├── db-deployment.yaml
    ├── db-pvc.yaml
    ├── db-service.yaml
    ├── web-deployment.yaml
    └── web-service.yaml

- Alternatively you can clone some part of your repository as show below:

```shell
sudo yum install git -y
mkdir repo && cd repo
git init
git remote add origin <origin-url>
git config core.sparseCheckout true
echo "subdirectory/under/repo/" >> .git/info/sparse-checkout  # do not put the repository folder name in the beginning
git pull origin <branch-name>
```

### Steps of execution:

1. We will deploy the `to-do` app first and look at some key points.

2. And then deploy the `php-apache` app and highlights some important points.

> kubectl apply -f .  (her iki dosyada da calistir)

## Ingress

Briefly explain ingress and ingress controller. For additional information a few portal can be visited like;

- https://kubernetes.io/docs/concepts/services-networking/ingress/
  
- https://banzaicloud.com/blog/k8s-ingress/
  
- Open the offical [ingress-nginx]( https://kubernetes.github.io/ingress-nginx/deploy/ ) explain the `ingress-controller` installation steps for different architecture.


> kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.7.0/deploy/static/provider/cloud/deploy.yaml

- Guncellenmis halini yukaridaki siteden al.


- Now, check the contents of the `ingress-service`.

- code ingress-service.yaml

> kubectl apply -f ingress-service.yaml

> kubectl get ingress (ing)

On browser, type this  ( a26be57ce12e64883a5ad050025f2c5b-94ab4c4b033cf5fa.elb.eu-central-1.amazonaws.com ), you must see the to-do app web page. If you type `a26be57ce12e64883a5ad050025f2c5b-94ab4c4b033cf5fa.elb.eu-central-1.amazonaws.com/load`, then the apache-php page, "OK!". Notice that we don't use the exposed ports at the services.

- Delete the cluster

> eksctl get cluster --region us-east-1

> eksctl delete cluster cw-cluster --region us-east-1

- Do no forget to delete related ebs volumes.

<POD> <SCHEDULING>

## Part 2 - Scheduling Pods

- Taints node'a, toleratins pod'a eklenir.

- In Kubernetes, scheduling refers to making sure that Pods are matched to Nodes so that Kubelet can run them.

- A scheduler watches for newly created Pods that have no Node assigned. For every Pod that the scheduler discovers, the scheduler becomes responsible for finding the best Node for that Pod to run on.

- kube-scheduler is the default scheduler for Kubernetes and runs as part of the control plane.

- For every newly created pod or other unscheduled pods, kube-scheduler selects an optimal node for them to run on. However, every container in pods has different requirements for resources and every pod also has different requirements. Therefore, existing nodes need to be filtered according to the specific scheduling requirements.

- In a cluster, Nodes that meet the scheduling requirements for a Pod are called feasible nodes. If none of the nodes are suitable, the Pod remains unscheduled until the scheduler is able to place it.

- We can constrain a Pod so that it can only run on a particular set of Node(s). There are several ways to do this. Let's start with nodeName.

## Part 3 - nodeName

- For this lesson, we have two instances. The one is the controlplane and the other one is the worker node. As a practice, kube-scheduler doesn't assign a pod to a controlplane. Let's see this.

- Create yaml file named `clarus-deploy.yaml` and explain its fields.

> code clarus-deploy.yaml

> kubectl apply -f .

> kubectl get po -o wide

> kubectl get node <conrol-plane-name> (kube-master) -o yaml
- Bu komut ile master node yaml dosyasi icersinde bunu gor  
  
  ' -  taints:
      - effect: NoSchedule
        key: node-role.kubernetes.io/control-plane'

> kubectl taint nodes kube-master node-role.kubernetes.io/control-plane:NoSchedule- 
- Bu etiketi cikartmak icin komutu giriyoruz. 

Taint'i cikartmak icin diger yontem edit etmek

> kubectl edit node kube-master 

> kubectl delete -f .

> kubectl apply -f .

> kubectl get po -o wide
- Burada NODE sutunun da podlarin dengeli bir sekilde master ve worker node'lara dagildigini gor. Yukaridaki komutttan dolayi

> kubectl delete -f .

- `nodeName` is the simplest form of node selection constraint, but due to its limitations, it is typically not used. nodeName is a field of PodSpec. If it is non-empty, the scheduler ignores the pod and the kubelet running on the named node tries to run the pod. Thus, if nodeName is provided in the PodSpec, it takes precedence over the other methods for node selection.

- Some of the limitations of using nodeName to select nodes are:

  - If the named node does not exist, the pod will not be run, and in some cases may be automatically deleted.
  - If the named node does not have the resources to accommodate the pod, the pod will fail and its reason will indicate why, for example, OutOfmemory or OutOfcpu.
  - Node names in cloud environments are not always predictable or stable.

- Let's try this. First list the names of nodes.

##### Podlari istedigim node'a gonderme. 

- Bu yontem cok tercif EDILMEYEN bir yontem.

> code clarus-deploy-nodeName.yaml
- Yaml dosyaysini update ediyoruz. Son satira nodeName: kube-master ekledik.

> kubectl apply -f clarus-deploy-nodeName.yaml

> kubectl get po -o wide
- tum podlarin kube-master'a gonderilir.

## Part 4 - nodeSelector

- `nodeSelector` is the simplest recommended form of node selection constraint. nodeSelector is a field of PodSpec. It specifies a map of key-value pairs. For the pod to be eligible to run on a node, the node must have each of the indicated key-value pairs as labels (it can have additional labels as well). The most common usage is one key-value pair.

- Let's learn how to use nodeSelector.

- First, we will add a label to controlplane node with the following command. 

> kubectl label nodes <node-name> <label-key>=<label-value>
- Herhangi bir kubernetes objesine bu komutla etiket ekleyebiliriz.

> kubectl label nodes kube-master size=large

> kubectl describe kube-master
- Label'lari gor

> kubectl get nodes --show-labels

> code clarus-deploy-nodSelector

> kubectl apply -f clarus-deploynodeSelector.yaml

> kubectl get po -o wide

> kubectl delete -f .

## Part 5 - Node Affinity

- `Node affinity`  is conceptually similar to nodeSelector, but it greatly expands the types of constraints we can express. It allows us to constrain which nodes our pod is eligible to be scheduled on, based on labels on the node.

- There are currently three types of node affinity:

  - requiredDuringSchedulingIgnoredDuringExecution
  - preferredDuringSchedulingIgnoredDuringExecution

- Let's analyze this long sentence.

| Types                                           | DuringScheduling | DuringExecution |
| ------------------------------------------------| ---------------- | --------------- |
| requiredDuringSchedulingIgnoredDuringExecution  | required         | Ignored         |
| preferredDuringSchedulingIgnoredDuringExecution | preferred        | Ignored         |


- The first one (requiredDuringSchedulingIgnoredDuringExecution) specifies rules that must be met for a pod to be scheduled onto a node (similar to nodeSelector but using a more expressive syntax), while the second one (preferredDuringSchedulingIgnoredDuringExecution) specifies preferences that the scheduler will try to enforce but will not guarantee. For example, we specify labels for nodes and nodeSelectors for pods.
Later, the labels of the node are changed. If we use the first one (requiredDuringSchedulingIgnoredDuringExecution), the pod is not be scheduled. Let's see this.

- We will update clarus-deploy.yaml as below. We will add an affinity field instead of nodeSelector.

> code clarus-deploy-nodeAffinity

- This node affinity rule says the pod can only be placed on a node with a label whose key is size and whose value is large or medium.

> kubectl get nodes --show-labels
- We have already labeled the controlplane node with `size=large` key-value pair. Let's see.

> kubectl label node kube-master size-
- Delete `size=large` label from `kube-master` node.