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
```
## Deploiement manager
* Deploy a ready solution on VM with some click (Exp: Jenkins)
  * Marketplace
  * Choose a product
  * Launch deploy + Customize ressource
  * We can then connect via WEB IHM or ssh connection to the VM