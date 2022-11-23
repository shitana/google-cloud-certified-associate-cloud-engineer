[[_TOC_]]
# Introduction
## ORG . Folder . Project
  
![Global Archi](img/umacui39.jpg)

## Access: IAM
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

## IAM
![IAM](img/Capture%20d%E2%80%99%C3%A9cran%202022-11-22%20224026.jpg)
* IAM: https://cloud.google.com/iam/docs/overview

## Managed Instance Group: 
  * Availability
  * Scalability 
  * Automated Update
https://cloud.google.com/compute/docs/instance-groups/creating-groups-of-managed-instances 

## Regions & Zones
* Region contains 3 or more zones

## GKE
https://cloud.google.com/kubernetes-engine/docs/concepts/types-of-clusters
* Autopilot : Fully-provisioned and managed Cluster
* Standard : We can define and manage Cluster
* Network Routing: 
  * VPC-native cluster
  * Route-based cluster

## Cloud run
https://cloud.google.com/run/docs/quickstarts?hl=en#build-and-deploy
* easiest deploiment methode :
  * Cloud run ==> GKE ==> Compute

## Cloud Functions:
https://cloud.google.com/functions/docs/calling 
* Serverless function execution trggered by an event

## Storage
https://cloud.google.com/storage/docs/creating-buckets
https://cloud.google.com/storage/docs/introduction
* Storage class it depends on your needs:
  * Standard is for immediate access and has no minimum storage duration
  * Nearline has a 30 day minimum duration and data retrieval charges
  * Coldline has a 90 day min duration and data retrieval charges
  * Archive has a 365 day min duration and data retrieval charges

## VPC 
https://cloud.google.com/vpc/docs/vpc
* Automode: auto-creation of network when we create a ressources in any region
* Custom: creation of ressources allowed only in the configured region

# Tuto:
* Perform Foundational Infrastructure Tasks in Google Cloud: (https://www.qwiklabs.com/quests/118)
* Set Up and Configure a Cloud Environment in Google Cloud
(https://www.qwiklabs.com/quests/119)
* Create and Manage Cloud Resources: (https://www.qwiklabs.com/quests/120)
* https://www.cloudskillsboost.google/paths/11
