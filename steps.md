
# Setup Istio

[istio at GitHub](https://github.com/istio/istio/tree/master/install/kubernetes/helm/istio)   
[istio helm install](https://istio.io/docs/setup/kubernetes/helm-install/)  

## Steps:
### Download latest Istio version:

[istio releaes](https://github.com/istio/istio/releases)  

1. extract into folder  
2. cd to the folder  

### CMD
If a service account has not already been installed for Tiller, install one:  
*kubectl apply -f install/kubernetes/helm/helm-service-account.yaml*  

### Helm repo
Add istio.io chart repository and point to the daily release:  
*helm repo add istio.io https://storage.googleapis.com/istio-prerelease/daily-build/master-latest-daily/charts*  

Build the Helm dependencies: (don't work right from the download)  
*helm dep update install/kubernetes/helm/istio*  

### install istio 
helm install install/kubernetes/helm/istio-init --name istio-init --namespace istio-system  

### Grafana
Grafana (https://istio.io/docs/tasks/telemetry/using-istio-dashboard/):  
kubectl -n istio-system get svc prometheus  
kubectl -n istio-system get svc grafana  
helm upgrade --recreate-pods --namespace istio-system --set grafana.enabled=true istio install/kubernetes/helm/istio  

### Service Graph
Service Graph (https://istio.io/docs/tasks/telemetry/servicegraph/)  
kubectl -n istio-system get svc servicegraph  
helm upgrade --recreate-pods --namespace istio-system --set servicegraph.enabled=true --set grafana.enabled=true istio install/kubernetes/helm/istio  

kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=servicegraph -o jsonpath='{.items[0].metadata.name}') 8088:8088  

http://localhost:8088/force/forcegraph.html  
