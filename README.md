# How to Setup ssl Ingress on Kubernetes?
Before Implementing, Let's have a understanding about some topics like-

### [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)
Kubernetes Ingress is an API object that provides routing rules to manage external users' access to the services in a Kubernetes cluster, typically via HTTPS/HTTP. With Ingress, you can easily set up rules for routing traffic without creating a bunch of Load Balancers or exposing each service on the node.

### [SSL](https://www.cloudflare.com/en-in/learning/ssl/what-is-ssl/)
SSL provides a secure channel between two machines or devices operating over the internet or an internal network. One common example is when SSL is used to secure communication between a web browser and a web server. This turns a website's address from HTTP to HTTPS, the ‘S’ standing for ‘secure’


### [Cert-manager](https://cert-manager.io/docs/)
Cert-manager adds certificates and certificate issuers as resource types in Kubernetes clusters, and simplifies the process of **obtaining, renewing** and using those certificates. It can issue certificates from a variety of supported sources, including **Let's Encrypt**, **HashiCorp** Vault, and **Venafi** as well as private **PKI**.

### [Service Account](https://cloud.google.com/iam/docs/understanding-service-accounts)
A service account is a special type of Google account intended to represent a non-human user that needs to authenticate and be authorized to access data in Google APIs. Typically, service accounts are used in scenarios such as: Running workloads on virtual machines (VMs).

### [Clouddns](https://cloud.google.com/dns/docs/overview)
Cloud DNS is a high-performance, resilient, global Domain Name System (DNS) service that publishes your domain names to the global DNS in a cost-effective way. DNS is a hierarchical distributed database that lets you store IP addresses and other data, and look them up by name.

### [Secret](https://kubernetes.io/docs/concepts/configuration/secret/)
A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Such information might otherwise be put in a Pod specification or in a container image. Using a Secret means that you don't need to include confidential data in your application code.

## Usage
### How to Setup SSL Ingress on GKE using Clouddns?
```
# first Change the direcrtory using
  cd ssl-ingress-on-gke-with-clouddns
  
# Create the Cert-manager in your cluster
  kubectl create -f cert-manager.yaml 
  
# Create a service account in gcp
   gcloud iam service-accounts create SERVICE_ACCOUNT_NAME \
    --description="DESCRIPTION" \
    --display-name="DISPLAY_NAME"
   
# Grant Clouddns Admin role to your service account 
  gcloud projects add-iam-policy-binding PROJECT_ID \
    --member="serviceAccount:SERVICE_ACCOUNT_NAME@PROJECT_ID.iam.gserviceaccount.com" \
    --role="roles/dns.admin"
    
# Create Service Account key 
  gcloud iam service-accounts keys create KEY_FILE \
    --iam-account=SERVICE_ACCOUNT_NAME@PROJECT_ID.iam.gserviceaccount.com
    
# Create a secret with service account key in cert-manager namespace 
  kubectl -n cert-manager create secret generic <secret_name> \
  --from-file=<service-account.json>
  
# Create stage ClusterIssuer, Update the Email id, Project Id, Secret Name & Service Account key Name in "stage-clusterissuer.yaml" then run below command
  kubectl create -f stage-clusterissuer.yaml
  
# Create stage certificate, Update Domain name in "stage-certificate.yaml" and then run below command 
  kubectl create -f stage-certificate.yaml
  
# Now keep below line in ingress manifest file and test it 
  tls:
  - secretName: stage-cert

# If this is working fine then create Prod ClusterIssuer & Prod Certificate 
 
# Create Prod ClusterIssuer, Update the Email id, Project Id, Secret Name & Service Account key Name in "prod-clusterissuer.yaml" then run below command
  kubectl create -f prod-clusterissuer.yaml
  
# Create prod certificate, Update Domain name in "prod-certificate.yaml" and then run below command 
  kubectl create -f prod-certificate.yaml
  
# Now Update your secret Name stage to prod, Apply changes and test this, it will work fine.
  tls:
  - secretName: prod-cert
  
```
For more information follow this [Blog](https://kosyfrances.com/letsencrypt-dns01/)
