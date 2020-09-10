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

1. Use the ping command to confirm that __my-vm-2__ can reach __my-vm-1__ over the network:

-entering in to __my-vm-2__:
##
    gcloud compute ssh my-vm-2
--ping __my-vm-1__ from __my-vm-2__
##
    ping -c 6 my-vm-1
--use the ssh command to open a command prompt on __my-vm-1__ from __my-vm-2__
##
    ssh my-vm-1
    
--at the command prompt on __my-vm-1__, install the Nginx web server:
##
    sudo apt-get install nginx-light -y
-- use nano text editor to add a custom message to the home page of the web server:
##
    sudo nano /var/www/html/index.nginx-debian.html
-- at text like this, and replace YOUR_NAME with your name:
##
    Hi from Muhammed Garba
-- exit the editor and confirm that the web server is serving your new page. at the command prompt on my-vm-1, execute this command:
## 
    curl http://localhost
--result:
>the response will be the html source of the web server's home page, including your line of custom text.

--then exit the command prompt using:
## 
    exit
-- to confirm that __my-vm-2__ can reach the web server on __my-vm-1__, at the command prompt on my-vm-2, execute this command:
## 
    curl http://my-vm-1

    
2. Now get external IP of __my-vm-1__ instance from this command:
##
    gcloud compute instances list
3. Paste the copied IP address of __my-vm-1__ into a new browser tab and hit enter.
> You will see your web server's home page, including your custom text.
