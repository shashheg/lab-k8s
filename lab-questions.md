1. Create a deployment named `nginx-deploy` in the `ns-nginx` namespace using nginx image version 1.22 with three replicas. Check that the deployment rolled out and show running pods.\
 a. Check that the deployment has rolled out and that it is running:\ 
 b. Scale the deployment to 5 replicas and check the status again.\
 c. change the image tag of `nginx` container from `1.22` to `1.23`\
<details><summary>show</summary>
<p>

```bash

```

</p>
</details> 

 
 2. Create a namespace called `mynamespace` and a pod with image `nginx` called `nginx` on this namespace 

<details><summary>show</summary>
<p>

```bash

```

</p>
</details> 

 3. Create a pod named `busybox` which runs the command `date` and observe the deatils in pod logs. 

<details><summary>show</summary>
<p>

```bash

```

</p>
</details> 

4. Create a DaemonSet with the latest busybox image and see that it runs on all nodes 

<details><summary>show</summary>
<p>

```bash

```

</p>
</details> 

5. Label a node with `kind=special` and schedule a pod to that node.
<details><summary>show</summary>
<p>

```bash

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

```

</p>
</details> 

7. Launch a pod with a busybox container that launches with the sheep 3600 command (this command doesn't exist.\
Get the logs from the pod, then correct the error to make it launch sleep 3600. 
<details><summary>show</summary>
<p>

```bash

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

```

</p>
</details> 
