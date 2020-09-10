---
### Lab Name
# Creating a GKE Cluster Via Cloud

***
# 1. Deploy GKE clusters
-- set the environment variable for the zone and cluster name.
#
    export my_zone=us-central1-a export my_cluster=standard-cluster-1
-- Create a Kubernetes cluster.
#
    gcloud container clusters create $my_cluster --num-nodes 3 --zone $my_zone --enable-ip-alias
# 2. Modify GKE Clusters
-- To modify standard-cluster-1 to have four nodes
#
    gcloud container clusters resize $my_cluster --zone $my_zone --size=4
> press y to confirm.
# Connect to a GKE Clusters
-- to allow communicating with that cluster through the kubectl command-line tool
#
    gcloud container clusters get-credentials $my_cluster --zone $my_zone
-- This command creates a .kube directory in your home directory if it doesn't already exist. 
# Use kubectl to inspect a GKE cluster
#
    kubectl config view
> You can get the cluster information for the active context by running the following command
#
    kubectl cluster-info
-- command to print out the active context
#
    kubectl config current-context
-- command to print out some details for all the cluster contexts in the kubeconfig file.
#
    kubectl config get-contexts 
-- command to change the active context
#
    kubectl config use-context gke_${GOOGLE_CLOUD_PROJECT}_us-central1-a_standard-cluster-1
-- Execute the following command to view the resource usage across the nodes of the cluster:
#
    kubectl top nodes
-- In Cloud Shell, execute the following command to enable bash autocompletion for kubectl:
#
    source <(kubectl completion bash)
-- In Cloud Shell, type kubectl and press the Tab key twice.
>   The shell outputs all the possible commands:
-- In Cloud Shell, type kubectl co and press the Tab key twice.
> The shell outputs all commands starting with "co" (or any other text you type).
#  5. Deploy Pods to GKE clusters
-- Use kubectl to deploy Pods to GKE
* In Cloud Shell, execute the following command to deploy the latest version of nginx as a Pod named nginx-1
#
    kubectl run nginx-1 --image nginx:latest
* In Cloud Shell, execute the following command to view all the deployed Pods in the active context cluster:
#
    kubectl get pods    
* Store pod name in a variable using 
#
    export my_nginx_pod=[your_pod_name]
* To confirm that use
#
    echo $my_nginx_pod
* execute the following command to view the complete details of the Pod you just created.
#
    kubectl describe pod $my_nginx_pod
-- the output of this command will like this 
> Name:           nginx-1-74c7bbdb84-nvwsc
Namespace:      default
Node:           gke-standard-cluster-1-default-pool-bc4ec334-0hmk/10.128.0.5
Start Time:     Sun, 16 Dec 2018 14:29:38 -0500
Labels:         pod-template-hash=3073668640
                run=nginx-1
Annotations:    kubernetes.io/limit-ranger=LimitRanger plugin set: cpu ...
Status:         Running
IP:             10.8.3.3
Controlled By:  ReplicaSet/nginx-1-74c7bbdb84
Containers:
  nginx-1:
    Container ID:   docker://dce87d274e6d25300b07ec244c265d42806579fee...
    Image:          nginx:latest
    Image ID:       docker-pullable://nginx@sha256:87e9b6904b4286b8d41...
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Sun, 16 Dec 2018 14:29:44 -0500
    Ready:          True
    Restart Count:  0
    Requests:
      cpu:        100m
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-tok...
Conditions:
  Type           Status
  Initialized    True
  Ready          True
  PodScheduled   True
Volumes:
  default-token-nphcg:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-nphcg
    Optional:    false
QoS Class:       Burstable
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason  Age   From                         Message
>  ----    ------  ----  ----                         -------
 > Normal  Sche... 1m    default-scheduler            Successf...
  Normal  Succ... 1m    kubelet, gke-standard-cl...  MountVol...
  Normal  Pull... 1m    kubelet, gke-standard-cl...  pulling ...
  Normal  Pull... 1m    kubelet, gke-standard-cl...  Successf...
  Normal  Crea... 1m    kubelet, gke-standard-cl...  Created ...
  Normal  Star... 1m    kubelet, gke-standard-cl...  Started ...

### Push a file into a container
-- Type the following commands to open a file named test.html in the nano text editor.
#
    nano ~/test.html
-- Add the following text (shell script) to the empty test.html file:
```html
<html> <header><title>This is title</title></header>
<body> Hello world </body>
</html>
```
> Press CTRL+X, then press Y and enter to save the file and exit the nano editor.
* execute the following command to place the file into the appropriate location within the nginx container in the nginx Pod to be served statically:
#
    kubectl cp ~/test.html $my_nginx_pod:/usr/share/nginx/html/test.html
### Expose the Pod for testing
* xecute the following command to create a service to expose our nginx Pod externally
#
    kubectl expose pod $my_nginx_pod --port 80 --type LoadBalancer
* execute the following command to view details about services in the cluster:
#
    kubectl get services
* execute the following command to verify that the nginx container is serving the static HTML file that you copied.
#
    curl http://[EXTERNAL_IP]/test.html
> replace [EXTERNAL_IP] with the external your service that you obtained from the output of the previous step
* execute the following command to view the resources being used by the nginx Pod:
#
    kubectl top pods
# 6. Introspect GKE Pods
### Prepare the environment
* enter the following command to clone the repository to the lab Cloud Shell.
#
    git clone https://github.com/GoogleCloudPlatformTraining/training-data-analyst
* Change to the directory that contains the sample files for this lab
#
    cd ~/training-data-analyst/courses/ak8s/04_GKE_Shell/

> A sample manifest YAML file for a Pod called new-nginx-pod.yaml has been provided for you:
* To deploy your manifest, execute the following command:    
#
    kubectl apply -f ./new-nginx-pod.yaml
* To see a list of Pods, execute the following command
#
    kubectl get pods
### Use shell redirection to connect to a Pod
* execute the following command to start an interactive bash shell in the nginx container:
#
    kubectl exec -it new-nginx /bin/bash
> root@new-nginx:/#
* in the nginx bash shell, execute the following commands to install the nano text editor:
#
    apt-get update
apt-get install nano
> You need to create a test.html file in the static served directory on the nginx container.
* in the nginx bash shell, execute the following commands to switch to the static files directory and create a test.html file:
#
    cd /usr/share/nginx/html
nano test.html
* in the nginx bash shell nano session, type the following text:
```html
    <html> 
        <header>
            <title>This is title</title>
        </header>
        <body> Hello world </body>
    </html>
```
> Press CTRL+X, then press Y and enter to save the file and exit the nano editor.
* in the nginx bash shell, execute the following command to exit the nginx bash shell:
#
    exit
> To connect to and test the modified nginx container (with the new static HTML file), you could create a service. An easier way is to use port forwarding to connect to the Pod directly from Cloud Shell.
* execute the following command to set up port forwarding from Cloud Shell to the nginx Pod (from port 10081 of the Cloud Shell VM to port 80 of the nginx container):
#
    kubectl port-forward new-nginx 10081:80

-- the output will like the following
> Forwarding from 127.0.0.1:10081 -> 80
Forwarding from [::1]:10081 -> 80
* In the Cloud Shell menu bar, click the plus sign (+) icon to start a new Cloud Shell session.
> A second Cloud Shell session appears in your Cloud Shell window. You can switch between sessions by clicking the titles in the menu bar.
* In the second Cloud Shell session, execute the following command to test the modified nginx container through the port forwarding:
#
    curl http://127.0.0.1:10081/test.html
> The HTML text you placed in the test.html file is displayed.
* In the Cloud Shell menu bar, click the plus sign (+) icon to start another new Cloud Shell session.
* In third Cloud Shell window, execute the following command to display the logs and to stream new logs as they arrive (and also include timestamps) for the new-nginx Pod:
#
    kubectl logs new-nginx -f --timestamps
> You will see the logs display in this new window
> Return to the second Cloud Shell window and re-run the curl command to generate some traffic on the Pod.
> Review the additional log messages as they appear in the third Cloud Shell window.