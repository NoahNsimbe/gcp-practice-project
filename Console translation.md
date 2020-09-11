# LAB: Google Cloud Fundamentals: Getting Started with Compute Engine

## Objectives

In this lab, you will learn how to perform the following tasks:
    
    1. Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.
    
    2. Create a Compute Engine virtual machine using the gcloud command-line interface.
    
    3. Connect between the two instances.
## Steps
1. Create a virtual machine using the GCP Console

- Create a virtual machine called my-vm-1 a nd assign it a tag "allowHttp"

        gcloud compute instances create my-vm-1 --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default" --tags allowHttp

- Allow http on my-vm-1

        gcloud compute firewall-rules create allow-http --action=ALLOW --destination=INGRESS --rules=http:80 --target-tags=allowHttp 

2. Create a virtual machine using the gcloud command line

- Set compute zone

        gcloud config set compute/zone us-central1-b

- Create a virtual machine called my-vm-2

        gcloud compute instances create my-vm-2 --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default"
        
3. Connect between VM instances
    - Connect to my-vm-2:
    
            gcloud compute ssh my-vm-2
    
    - ping my-vm-1 from my-vm-2:
    
            ping -c 4 my-vm-1
    
    - Use the ssh comand to open a comand prompt on my-vm-1 from my-vm-2:
    
            ssh my-vm-1
    
    - At the command prompt on my-vm-1, install the Nginx web server:  
    
            sudo apt-get install nginx-light -y
            
    - Use the nano text editor to add a custom message to the home page of the web server:
    
            sudo nano /var/www/html/index.nginx-debian.html
            
    - Add text like this, and replace YOUR_NAME with your name:

            Hi from YOUR_NAME

    - Save your edited file using the comand below then press Enter.
    
            Ctrl + O
            
    - Confirm that the web server is serving your new page. At the command prompt on my-vm-1, you will get your web server's homegage with your custom text
            
            curl http://localhost/
            
    - Get the external IP address of my-vm-1 instance
    
            gcloud compute instances list
            
    - Paste the copied IP address of my-vm-1 into a new web browser tab and hit enter, you will get your web server's homegage with your custom text
    

<br/>
<br/>
<hr/>
<br/>
<br/>

# LAB: AK8S-03 Creating a GKE Cluster via GCP Console

## Objectives

In this lab, you learn how to perform the following tasks:

    1. Use the GCP Console to build and manipulate GKE clusters
    
    2. Use the GCP Console to deploy a Pod
    
    3. Use the GCP Console to examine the cluster and Pods
    
### Steps

1.  Deploy GKE clusters
- Create variables to store your compute zone "us-central1-a" and cluster name "standard-cluster-1".

        export my_zone=us-central1-a
        export my_cluster=standard-cluster-1

- create a cluster with 3 nodes.

        gcloud container clusters create $my_cluster --num-nodes 3 --zone $my_zone --enable-ip-alias

2. Modify GKE clusters

- Resize the cluster from 3 nodes to 4 nodes.

        gcloud container clusters resize $my_cluster --zone $my_zone --size=4

3. Deploy a sample workload

- Deloy an ngnix workload called "ngnix-1"

        kubectl create deployment nginx-1 --image nginx:latest

- List pods
        
        kubectl get pods
        
- Store the pod name in a variable

        export my_pod=[your_pod_name]

- Display details about the pod

        kubectl describe pod $my_pod




 