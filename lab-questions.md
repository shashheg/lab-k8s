1. Create a deployment named `nginx-deploy` in the `ns-nginx` namespace using nginx image version 1.22 with three replicas. Check that the deployment rolled out and show running pods.\
 a. Check that the deployment has rolled out and that it is running:\ 
 b. Scale the deployment to 5 replicas and check the status again.\
 c. change the image tag of `nginx` container from `1.22` to `1.23`\
<details><summary>show</summary>
<p>

```bash
# Create the template from kubectl
kubectl -n ns-nginx create deployment nginx-deploy --replicas=3 --image=nginx:1.22 --dry-run=client -o yaml > nginx-deploy.yaml
# Create the namespace first
kubectl create ns ns-nginx
kubectl apply -f nginx-deploy.yaml
 
kubectl -n ns-nginx rollout status deployment/nginx-deploy
deployment "nginx-deploy" successfully rolled out

kubectl -n ns-nginx get deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deploy   3/3     3            3           44s
 
kubectl -n ns-nginx scale deployment nginx-deploy --replicas=5

kubectl -n ns-nginx rollout status deployment nginx-deploy
kubectl -n ns-nginx rollout history deployment nginx-deploy

kubectl -n ns-nginx edit deployment/nginx-deploy
...
    spec:
      containers:
      - image: nginx:1.23
        imagePullPolicy: IfNotPresent 
```

</p>
</details> 

 
 2. Create a namespace called `mynamespace` and a pod with image `nginx` called `nginx` on this namespace 

<details><summary>show</summary>
<p>

```bash
kubectl create namespace mynamespace
kubectl run nginx --image=nginx --restart=Never -n mynamespace
```

</p>
</details> 

 3. Create a pod named `busybox` which runs the command `date` and observe the deatils in pod logs. 

<details><summary>show</summary>
<p>

```bash
kubectl run busybox --image=busybox --restart=Never --dry-run=client -o yaml --command -- date > date-pod.yaml
kubectl logs busybox
```

</p>
</details> 

4. Create a DaemonSet with the latest busybox image and see that it runs on all nodes 

<details><summary>show</summary>
<p>

```bash
apiVersion: apps/v1
kind: DaemonSet
metadata:
  labels:
    type: daemon
  name: daemontest
spec:
  selector:
    matchLabels:
      run: daemon
  template:
    metadata:
      labels:
        run: daemon
      name: daemonpod
    spec:
      containers:
      - image: busybox:latest
        name: daemonpod
        args:
          - sleep
          - "3600"
```

</p>
</details> 

5. Label a node with `kind=special` and schedule a pod to that node.
<details><summary>show</summary>
<p>

```bash
kubectl label nodes ip-192-168-17-178.ec2.internal kind=special
kubectl create -f 05_node-label.yaml
```

</p>
</details> 

6. Create a deployment with the latest nginx image and two replicas.(declarative yaml file creation)\
  a.Expose it's port `80` through a service of type `NodePort`.\
  b.Show all elements, including the endpoints.\
  c.Get the `nginx` index page through the `NodePort`.

<details><summary>show</summary>
<p>

```bash
kubectl create deployment nginx --image=nginx:latest
kubectl scale deployment nginx --replicas=2
kubectl expose deployment nginx --port=80 --target-port=80 --type=NodePort
kubectl describe svc nginx

kubectl get pods -l app=nginx -o wide
wget -O- <IP>:80
```

</p>
</details> 

7. Launch a pod with a busybox container that launches with the sheep 3600 command (this command doesn't exist.\
Get the logs from the pod, then correct the error to make it launch sleep 3600. 
<details><summary>show</summary>
<p>

```bash
kubectl describe pods podfail
...
Warning  Failed     5s (x2 over 6s)  kubelet            Error: failed to create containerd task: OCI runtime create failed: container_linux.go:367: starting container process caused: exec: "sheep": executable file not found in $PATH: unknown
...

kubectl delete -f podfail.yaml
# Change sheep to sleep
kubectl apply -f podfail.yaml
```

</p>
</details> 

8. Create a pod with image nginx called nginx and expose its port 80\
   a. Confirm that `ClusterIP` has been created. Also check endpoints\
   b. Get service's `ClusterIP`, create a temp busybox pod and 'hit' that IP with wget\
   c. Convert the ClusterIP to `NodePort` for the same service and find the `NodePort` port. Hit service using Node's IP. Delete the service and the pod at the end.
   
<details><summary>show</summary>
<p>

```bash
kubectl run nginx --image=nginx --restart=Never --port=80 --expose|
kubectl get svc nginx
kubectl get ep

kubectl get svc nginx # get the IP (something like 10.108.93.130)
kubectl run busybox --rm --image=busybox -it --restart=Never -- sh
wget -O- IP:80
exit

kubectl patch svc nginx -p '{"spec":{"type":"NodePort"}}' 
kubectl get svc
wget -O- NODE_IP:31931 # if you're using Kubernetes with Docker for Windows/Mac, try 127.0.0.1
#if you're using minikube, try minikube ip, then get the node ip such as 192.168.99.117
```

</p>
</details> 
