# Monitoring


--------


### You will learn about
<br/>
* Accessing the Dashboard UI
*
* Install the Kubernetes CLI (kubectl)
* Configure kubectl to connect to your AKS cluster


--------


## Accessing The Dashboard UI


--------


The generic command is
```
az aks browse -n $CLUSTER_NAME -g $RESOURCE_GROUP_NAME
```

In our usecase

```
az aks browse -n myAKSCluster -g myResourceGroup
```

Open the Dashboard UI in a web browser

```
Merged "myAKSCluster" as current context in /var/folders/w9/nl8xvhfn2459040hvb83xtph0000gn/T/tmph9ngwmh0
Proxy running on http://127.0.0.1:8001/
Press CTRL+C to close the tunnel...
Forwarding from 127.0.0.1:8001 -> 9090
```


--------


### If RBAC are not enabled



`kubectl create -f kube-dashboard-access.yaml`


```
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: kubernetes-dashboard
  labels:
    k8s-app: kubernetes-dashboard
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: kubernetes-dashboard
  namespace: kube-system
```


--------


## Add Monitoring to <br/>an Azure Kubernetes Service Cluster


--------


There are a number of monitoring solutions available today.<br/>
Here is a quick, but not exhaustive list for reference purposes:
* Datadog
* Sysdig
* Elastic Stack
* Splunk
* Operations Management Suite
* **Prometheus / Grafana**


--------


### Install HELM


Create a Service Account for **tiller**

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system    
```

`kubectl create -f helm-rbac.yaml`


--------


### Initialize Helm

```
helm init --service-account tiller
```


--------


### Validate Helm and <br/>Tiller were installed successfully


```
helm version

Client: &version.Version{SemVer:"v2.8.2", GitCommit:"a80231648a1473929271764b920a8e346f6de844", GitTreeState:"clean"}
Server: &version.Version{SemVer:"v2.8.2", GitCommit:"a80231648a1473929271764b920a8e346f6de844", GitTreeState:"clean"}
```


--------


### Install Prometheus using Helm


```
helm install --name myPrometheus stable/prometheus --version 4.6.13 -f prometheus-configforhelm.yaml
```


--------


### Validate that Prometheus was installed

```
kubectl get pods | grep prometheus
kubectl get svc | grep prometheus
```


--------


###  Install Grafana


```
helm install --name myGrafana stable/grafana
             --version 0.5.1
             --set server.service.type=LoadBalancer,server.adminUser=admin,server.adminPassword=admin,server.image=grafana/grafana:latest,server.persistentVolume.enabled=false
```


--------


### Validate that Grafana was installed


```
kubectl get pods | grep grafana
kubectl get svc | grep grafana
```


--------


![Logo_k8s](Slides/Img/toolkit/grafana.png)


--------

#### If RBAC are not enabled

```
 kubectl create clusterrolebinding permissive-binding
                --clusterrole=cluster-admin
                --user=admin
                --user=kubelet
                --group=system:serviceaccounts
```
