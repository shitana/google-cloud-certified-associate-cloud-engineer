[[_TOC_]]
# Introduction
## ORG . Folder . Project
![Global Archi](img/umacui39.jpg)

## Access: [IAM](https://cloud.google.com/iam/docs/overview)
  * Policy: Role binding between Pricipals (user, group, svc account, ...) and Roles
  * **WHO** Principals:
    * Google Account
    * Service account
    * Google group
    * Google Workspace account
    * Cloud Identity domain
    * All authenticated users
    * All users
  * **Can do what** Roles
    * Basics
    * Predefined
    * Custom 
### [Authentication](https://cloud.google.com/docs/authentication/)
* Types of authentication keys
![](img/GCP%20Authentication.png)

* [Create a custom role](https://cloud.google.com/iam/docs/creating-custom-roles) : 
![](img/GCP%20custom%20role.png)
### [Service Account](https://cloud.google.com/docs/authentication#service-accounts)
![](img/svc%20accoutn%20iam.png)

* Create service account ==> Assign permissions ==> Attach to ressource (VM)

![IAM](img/Capture%20d%E2%80%99%C3%A9cran%202022-11-22%20224026.jpg)

1. [Creating a service account](https://cloud.google.com/iam/docs/creating-managing-service-accounts#creating_a_service_account) : 
2. Using service accounts with IAM policies
3. [Assigning service accounts to resources](https://cloud.google.com/compute/docs/access/create-enable-service-accounts-for-instances#using) 


## Regions & Zones
* Region contains 3 or more zones

## GKE
* [Cluster Type](https://cloud.google.com/kubernetes-engine/docs/concepts/types-of-clusters)
* [GKE Overview](https://cloud.google.com/kubernetes-engine/docs/concepts/kubernetes-engine-overview)
* Autopilot : Fully-provisioned and managed Cluster
* Standard : We can define and manage Cluster
### Networking
* Network Routing: 
  * VPC-native cluster
  * Route-based cluster
* To Deploy Ingress internal HTTP(s) load balancer `ingress.class: “gce-internal”` we should anootate service with a NEG ref 
  * NEG: Network Endpoint Groups `cloud.google.com/neg: '{"ingress": true}'`
  * https://cloud.google.com/kubernetes-engine/docs/concepts/ingress-ilb
  * https://cloud.google.com/kubernetes-engine/docs/how-to/internal-load-balance-ingress
* To Deploy Ingress External HTTP(s) load balance `ingress.class: “gce”`
  * https://cloud.google.com/kubernetes-engine/docs/concepts/ingress-xlb
  * https://cloud.google.com/kubernetes-engine/docs/how-to/load-balance-ingress

### [Security](https://cloud.google.com/architecture/prep-kubernetes-engine-for-prod#managing_identity_and_access)
 

## Cloud run
https://cloud.google.com/run/docs/quickstarts?hl=en#build-and-deploy
* easiest deploiment methode :
  * Cloud run ==> GKE ==> Compute
* [Autoscaling](https://cloud.google.com/run/docs/about-instance-autoscaling)
  * We can set au min instances / max instances 
  * we can set concurrency to handle how many user can connect to a particular instance

## Cloud Functions: 
* [Serverless function execution trggered by an event](https://cloud.google.com/functions/docs/calling)

## [Storage](https://cloud.google.com/storage/docs/introduction)
https://cloud.google.com/storage/docs/creating-buckets
* Storage class it depends on your needs:
  * Standard is for immediate access and has no minimum storage duration
  * Nearline has a 30 day minimum duration and data retrieval charges
  * Coldline has a 90 day min duration and data retrieval charges
  * Archive has a 365 day min duration and data retrieval charges
* [Lifecycle](https://cloud.google.com/storage/docs/lifecycle): (Exple )
    * Downgrade the storage class of objects older than 365 days to Coldline storage.
    * Delete objects created before January 1, 2019.
    * Keep only the 3 most recent versions of each object in a bucket with versioning enabled.

![](img/Screenshot%202022-11-23%20at%2023-09-26%20Preparing_for_ACE_Module_4_v2.0%20-%20Reading_Preparing_for_ACE_Module_4_v2.0.pdf.png)
* [Audit Logs with Cloud Storage](https://cloud.google.com/storage/docs/audit-logging)

![](img/GCP%20sotrage%20audit%20logs.png)


## [VPC](https://cloud.google.com/vpc/docs/vpc) 
* Automode: auto-creation of network when we create a ressources in any region
* Custom: creation of ressources allowed only in the configured region
* Expand ip range
  * expand ip range within a VPC by reducing your subnet mask
  * it can be undone

## [Compute](https://cloud.google.com/compute/docs/disks/snapshots)
* Persistent disk snapshots : backup disk 
  * we can create a schedule snapshot
  * Only the first one is a fullsnapshot
  * Snapshot N+1 contains only updated block of snapshot N

### [Instance template](https://cloud.google.com/compute/docs/instance-templates)
* Used to create a MIG or VM
### [Managed Instance Group](https://cloud.google.com/compute/docs/instance-groups) 
  * Availability
  * Scalability 
  * Automated Update

## Monitoring
### [Logging](https://cloud.google.com/logging/docs/audit)
* 4 Audit logs type
  * Admin Activity
  * Data Access
  * System Event
  * Policy Denied
### [Alerting](https://cloud.google.com/monitoring/alerts)
* https://cloud.google.com/monitoring/alerts/using-alerting-ui
* WHAT => HOW => WHO

![](img/gcp%20monitoring.png)


# Tuto:
* Perform Foundational Infrastructure Tasks in Google Cloud: (https://www.qwiklabs.com/quests/118)
* Set Up and Configure a Cloud Environment in Google Cloud: (https://www.qwiklabs.com/quests/119)
* Create and Manage Cloud Resources: (https://www.qwiklabs.com/quests/120)
* https://www.cloudskillsboost.google/paths/11
