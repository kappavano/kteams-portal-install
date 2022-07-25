# Install k-teams on a one node kubernetes cluster

Features:
* stand-alone
* fully-working
* needs a DNS entry

## Preparation
             
### Compute Instance

You need a compute instance (VM) from the hoster of your choice. 

* 4 CPUs 
* 8 GB of RAM 
* 40 GB of disk 
* Ubuntu 20.04
* public IP
* root access via SSH

are enough. Smaller can still work for some evaluations. Bigger is always better, but at some point you should switch to a multi-node kubernetes cluster instead.
                                  
Take note of the public IP address of the instance, we take `100.101.102.103` as an example.

### Set DNS records

You need two DNS records. At first, decide about a domain entry you want to access k-teams under. example: `kteams-demo.mycompany.com`, that's what we'll be working with now.

For this domain, please create two 'A' records, but make sure you replace IP and domain with the ones you're using:

```
A kteams-demo.mycompany.com 100.101.102.103
A *.kteams-demo.mycompany.com 100.101.102.103
```

### Install k3s

`k3s` is a lightweight kubernetes installation from Rancher.

connect to the server as root and run

` curl -sfL https://get.k3s.io | sh - `

### Access k3s from your environment                                                                    
Disconnect from the server. 
Now you can aquire the kubernetes configuration file to access k3s from your local env going forward.

```
scp root@kteams-demo.mycompany.com:/etc/rancher/k3s/k3s.yaml kubeconfig.yaml
export KUBECONFIG={pwd}/kubeconfig.yaml
```
                                                        
Adapt email in `traefik-override.yaml` to a valid contact email under your control. 
add Let's Encrypt configuration. 
```
helm -n kube-system  upgrade traefik traefik/traefik -f traefik-override.yaml
```

### Install k-teams

```
kubectl apply -f kteams-namespace.yaml
kubectl apply -f kteams-db-postgresql.yaml
kubectl apply -f kteams-portal.yaml
```
TODO ingress.


