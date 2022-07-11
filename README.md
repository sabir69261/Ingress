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
1- Change the directory using below command  
   `cd ssl-ingress-on-gke-with-clouddns`

2- Create cert-manager 
   `kubectl create -f cert-manager.yml`

3- Create a [service account]((https://cloud.google.com/iam/docs/creating-managing-service-accounts#creating) in gcp with Clouddns access,, And download the json key in your workspace.

4- Create a secret with service account key in cert-manager namespace 
   `kubectl create secret <secret_name>`
