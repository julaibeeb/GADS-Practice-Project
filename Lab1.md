____
**Lab Name**
##Getting Started with Compute Engine
____

###Objectives

1. Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.
1. Create a Compute Engine virtual machine using the gcloud command-line interface.
1. Connect between the two instances.

____

__Steps__

### 1. Create a virtual machine using the GCP Console

    gcloud compute instance create my-vm-1 --maachine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default" --tags http

    gcloud compute firewall-rules create allow-http --action=ALLOW --destination=INGRESS --rules=http:80 --target-tags=http

### 2. Create a virtual machine using the gcloud command line
    gcloud config set compute/zone us-central1-a

    gcloud compute instances create "my-vm-2" --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default"

### 3. Connect between VM instances

-Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network:

-entering in to my-vm-2:
##
    gcloud compute ssh my-vm-2
--ping my-vm-1 from my-vm-2
##
    ping -c 6 my-vm-1
    
