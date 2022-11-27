[[_TOC_]]
# GCP Compute services
* Compute Engine : VM
* GKE: Managed Kubernetes Cluster
* App Engine: run code on the cloud
* Cloud Functions: run code on the cloud on response to events 
* Cloud Run: Run a stateless container via trigger event (web / pubsub events)

# Interactting with GCP
* GCP Console
* Cloud SDK
* Could Shell
* Cloud Mob App 
![](img/GCP%20interacting%20.png)

## Cloud Shell
* Persistant vol 5Go (/home/)
* For persisting var; add them in `~/.profile`
```shell
# Create a bucket
gsutil mb gs://qwiklabs-gcp-04-a4acda5a7ffc-bucket-2

# Copy to bucket
gsutil cp 'GCP interacting .png' gs://qwiklabs-gcp-04-a4acda5a7ffc-bucket-2/

# List available regions
gcloud compute regions list

# Get project
gcloud config list  | grep project 

```
## Deploiement manager
* Deploy a ready solution on VM with some click (Exp: Jenkins)
  * Marketplace
  * Choose a product
  * Launch deploy + Customize ressource
  * We can then connect via WEB IHM or ssh connection to the VM

# Virtual Networks
## VPC
![](img/GPC%20VPC%20object.png)
* Procject: contains networks (up to 5) / associate objects, services with billing
* Network: NO IP address RAnge / it contains subnetworks

* 3 VPC Network types
  * Default: 
    * for each project 
    * one subnet / region
  * Automode
    * Regional IP allocation 
    * fixed /20 subnetwork / region
  * Custom mode
    * No default subnets created
    * Full contorl of IP ranges

## Subnet
![](img/GCP%20subnet%20zone.png)

* Project X: has 5 networks
  * VM A & B in the network 1 : then can directly communicate even between diff regions
  * VM C & D in the same region but in a different network : they can communicate only throw "internet" (throw Google Edge Routers , not the public internet )

![](img/GCP%20subnet.png)
* Exand subnet network use case : subnet with max 4 IP and we try to create a 5th VM
  * VPC => Choose network => update mask 
  * Retry to create VM

## Ip address
* Each VM may have Internal IP & External IP (optionnal)
* VM don't know its External IP ; it's routed to the internal IP via VPC Network
* We can assign a rang of IP Add as alias to VM's ine. to enable communication between services inside VMs.
![](img/GCP%20Subnet%20alias.png)
### Internal IP
* FQDN: hostname.zone.c.project-id.internal

### External IP
* DNS is not published by default
* it can be published using cloud DNS

## [Routes](https://console.cloud.google.com/networking/routes/list) & [FW rules](https://console.cloud.google.com/networking/firewalls/list)
* Routes / FW Rules are defined at network level and they are applicable to all instances of the subnet
* FW Rules:
  * Ingress: incomming connections (from any source External or internal subnet)
  * Egress: outcomming connections (from any source External or internal subnet)
