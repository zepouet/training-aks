# AKS cluster


--------


### You will learn about
<br/>
* Deploy a Kubernetes AKS cluster
* Create a registry
* Install the Kubernetes CLI (kubectl)
* Configure kubectl to connect to your AKS cluster


--------


```
az ad sp create-for-rbac
```


```
{
  "appId": "e7596ae3-6864-4cb8-94fc-20164b1588a9",
  "displayName": "azure-cli-2018-06-29-19-14-37",
  "name": "http://azure-cli-2018-06-29-19-14-37",
  "password": "52c95f25-bd1e-4314-bd31-d8112b293521",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```


--------


## Create really the Cluster
<br/>


By default, the Azure CLI automatically <br/>enables RBAC when you create an AKS cluster.

```
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 3 \
    --service-principal <appId> \
    --client-secret <password> \
    --generate-ssh-keys
```


--------


### Install the Kubernetes Client
<br/>

If you use the Azure Cloud Shell, kubectl is already installed else <br/>

```
az aks install-cli
```


--------


### Connect to cluster using kubectl
<br/>

```
az aks get-credentials  --resource-group myResourceGroup
                        --name myAKSCluster

```

Then

```
$ kubectl get nodes

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-xxxxxxxx-0   Ready     agent     9m        v1.10.1
aks-nodepool1-xxxxxxxx-1   Ready     agent     9m        v1.10.1
aks-nodepool1-xxxxxxxx-2   Ready     agent     9m        v1.10.1
```
