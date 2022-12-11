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
# Set Project 
gcloud config set project qwiklabs-gcp-00-6b56e0acc40f

# Create a bucket
gsutil mb gs://qwiklabs-gcp-04-a4acda5a7ffc-bucket-2

# Copy to bucket
gsutil cp 'GCP interacting .png' gs://qwiklabs-gcp-04-a4acda5a7ffc-bucket-2/

# List available regions
gcloud compute regions list

# Get project
gcloud config list  | grep project 

# Connect to a VM
gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap
```
## Deploiement manager
* Deploy a ready solution on VM with some click (Exp: Jenkins)
  * Marketplace
  * Choose a product
  * Launch deploy + Customize ressource
  * We can then connect via WEB IHM or ssh connection to the VM

# VPC
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
## Network
* create network custom + subnet for one specific region

```shell
gcloud compute networks create managementnet --project=qwiklabs-gcp-04-5b2d3880edf9 --subnet-mode=custom --mtu=1460 --bgp-routing-mode=regional

gcloud compute networks subnets create managementsubnet-us --project=qwiklabs-gcp-04-5b2d3880edf9 --range=10.240.0.0/20 --stack-type=IPV4_ONLY --network=managementnet --region=us-east5

gcloud compute networks list
```

## Subnet
![](img/GCP%20subnet%20zone.png)

* Project X: has 5 networks
  * VM A & B in the network 1 : then can directly communicate even between diff regions
  * VM C & D in the same region but in a different network : they can communicate only throw "internet" (throw Google Edge Routers , not the public internet )

![](img/GCP%20subnet.png)
* Exand subnet network use case : subnet with max 4 IP and we try to create a 5th VM
  * VPC => Choose network => update mask 
  * Retry to create VM
* [Private Google Access](https://cloud.google.com/vpc/docs/private-google-access) enable connexion to External IP Addresses of Google APIs to VM instance withour an IP External
  * [Eligible API](https://cloud.google.com/vpc/docs/private-access-options#pga-supported) with PGA
![Alt text](img/GCP%20Private%20Google%20Access.jpg)

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
  * Egress: outcomming connections (to any source External or internal subnet)
* Create FW rules:

```shell
# Create FW Rules allow TCP 22 and ICMP on network managmentnet
gcloud compute --project=qwiklabs-gcp-04-5b2d3880edf9 firewall-rules create managementnet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=managementnet --action=ALLOW --rules=tcp:22,tcp:3389,icmp --source-ranges=0.0.0.0/0

# List FW rules
gcloud compute firewall-rules list --sort-by=NETWORK
```

## [Cloud NAT](https://cloud.google.com/nat/docs/overview)
* Clound NAT is a regional resource, it lets certain resources without external IP addresses create outbound connections to the internet.
* Cloud NAT use a cloud Router (mandatory) ; se below the exact architecture of VM with /Without External IP and how to connect to interne ([Explanation here](https://youtu.be/Y47yGMY2-UU?t=172))
![Alt text](img/GCP%20Cloud%20NAT%20Cloud%20Router.jpg)

# VM Instance 

## Machine type structure
* Family (Genaral, Compute, ...)  ==> Series (E2, N1, N2, ...) ==> Type 
![Alt text](img/GCP%20VM%20Family.jpg)
![Alt text](img/GCP%20VM%20Family%202.jpg)
![Alt text](img/GCP%20VM%20Family%203.jpg)
![Alt text](img/GCP%20VM%20Family%204.jpg)
## VM Lifecycle
![Alt text](img/GCP%20VM%20Lifecycle.jpg)
* When VM is running  :
  * we can not Update Availibility policies 
  * we can not Update Machine Type
  * we can add network
  * we can add disk

## SPOT VM (Preemptible)
* Spot VMs are available at much lower prices—60-91% discounts
* **Compute Engine might preemptively stop or delete (preempt) Spot VMs when it needs to reclaim that capacity.**

## VM Windows
* In order to enable remote connection :
  * Enable HTTP/HTTPS access in firewall section during creation
  * Set Login/Password here : **VM Instance ==> Your-Windows-VM ==> Details**

## Storage:
* Local SSD: a physicaly attatched disk to VM instance 
  * Terminated / Stopped VM ==> Loss Data
  * Cannot be attached to a different VM
* [More storage type](https://cloud.google.com/compute/docs/disks)
* Persistent disks
  * Zonal or Regional
  * Can be resized

![Alt text](img/GCP%20VM%20Storage.jpg)

## VM Commun actions
* Startup & Shutdown scripts : they can hae access to VM's metadata 
*  Move VM (and all subprocess)
   *  Automated : within region 
   *  Manual : between region
*  Snapshot disk to "*Cloud storage*"
   *  to simplify move disk 
   *  to simplify upgrade disk type

## Images
* Public base images  
* Custom images

## NETWORK VM without External IP
### EGRESS 
* VM without External IP can reach external network only via VPN Gateway or [Cloud IAP](https://cloud.google.com/blog/products/identity-security/cloud-iap-enables-context-aware-access-to-vms-via-ssh-and-rdp-without-bastion-hosts)
* VM without External IP can reach external IP Addresses of Google APIs only via [Private Google Access](https://cloud.google.com/vpc/docs/private-google-access)
* Enable access to internet using [Cloud NAT Gateway](https://cloud.google.com/nat/docs/overview)

### INGRESS
* we can enable external access to our machine using [IAP](https://cloud.google.com/architecture/building-internet-connectivity-for-private-vms#grant_access_to_additional_users)
![Alt text](img/GCP%20IAP.jpg)

```shell 
# Create VM without External IP
gcloud compute instances create vm-internal --project=qwiklabs-gcp-00-6b56e0acc40f --zone=us-central1-c --machine-type=n1-standard-1 --network-interface=subnet=privatenet-us,no-address --metadata=enable-oslogin=true --maintenance-policy=MIGRATE --provisioning-model=STANDARD --service-account=716570319608-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --create-disk=auto-delete=yes,boot=yes,device-name=vm-internal,image=projects/debian-cloud/global/images/debian-11-bullseye-v20221102,mode=rw,size=10,type=projects/qwiklabs-gcp-00-6b56e0acc40f/zones/us-central1-a/diskTypes/pd-balanced --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --reservation-affinity=any
```