# kteams-portal-install

```                
export NAMESPACE=default
export KTEAMS_RELEASE=day20230101

kubectl create namespace $NAMESPACE    
helm install -n $NAMESPACE kteams https://raw.githubusercontent.com/kappavano/kteams-portal-install/main/single-node-k3s/kteams-portal-helm-release/kteams-portal-helm-1.0.0.tgz --set kteamsReleaseTag=$KTEAMS_RELEASE
```