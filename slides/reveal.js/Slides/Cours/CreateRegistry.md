# Azure registry


--------


### You will learn about
<br/>
* With the Azure Portal
* With the Azure CLI


--------


### <p>Create a container registry</p> <p> using the Azure portal</p>


--------


![Logo_k8s](Slides/Img/registry/qs-portal-01.png)


--------


![Logo_k8s](Slides/Img/registry/qs-portal-03.png)


--------


#### Types of SKU

Three types (Basic, Standard and Premium) but all have:
* Azure Active Directory authentication integration
* image deletion
* web hooks


--------


#### SKU Description

* **Basic**
  * cost-optimized
  * for developers
  * same programmatic capabilities as Standard and Premium
* Standard
* Premium


--------


#### SKU Description

* Basic  
* **Standard**
  * increased storage limits and image throughput
  * the needs of most production scenarios
* Premium


--------


#### SKU Description

* Basic  
* Standard
* **Premium**
  * geo-replication for managing a single registry across multiple regions
  * maintaining a network-close registry to each deployment


--------


### AZURE PORTAL


--------


#### ACCESS keys


![Logo_k8s](Slides/Img/registry/qs-portal-05.png)


--------


#### ACCESS keys


![Logo_k8s](Slides/Img/registry/qs-portal-06.png)


--------


### TEST WITH DOCKER Client


--------


```
docker login --username <username> --password <password> <login server>
docker pull microsoft/aci-helloworld
docker tag microsoft/aci-helloworld <login server>/aci-helloworld:v1

docker push <login server>/aci-helloworld:v1
```

```
The push refers to a repository [uniqueregistryname.azurecr.io/aci-helloworld]
7c701b1aeecd: Pushed
c4332f071aa2: Pushed
0607e25cc175: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:f2867748615cc327d31c68b1172cc03c0544432717c4d2ba2c1c2d34b18c62ba size: 1577
```


--------


#### List container images


![Logo_k8s](Slides/Img/registry/qs-portal-09.png)


--------


#### Deploy image to ACI


![Logo_k8s](Slides/Img/registry/qs-portal-10.png)


--------


#### Deploy image to ACI


![Logo_k8s](Slides/Img/registry/qs-portal-11.png)


--------


#### Deploy image to ACI


![Logo_k8s](Slides/Img/registry/qs-portal-12.png)


--------


#### Deploy image to ACI


![Logo_k8s](Slides/Img/registry/qs-portal-13.png)


--------


#### Deploy image to ACI


![Logo_k8s](Slides/Img/registry/qs-portal-14.png)


--------


#### View the application


![Logo_k8s](Slides/Img/registry/qs-portal-15.png)


--------


#### Clean up resources


![Logo_k8s](Slides/Img/registry/qs-portal-08.png)


--------


## EXERCICE



--------


### <p>Create a container registry</p> <p> using the Azure CLI</p>


--------


Before to continue, you need to create a resource group


```
az account list-locations
az group create --name myResourceGroup
                --location <yourlocation>
```


--------


### Create a registry



--------

```
az acr create --resource-group myResourceGroup
              --name <acrName>
              --sku Basic
```

The Basic SKU is a cost-optimized entry point for development purposes that provides a balance of storage and throughput.


--------


To use the ACR instance, you must first log in.

```
az acr login --name <acrName>
```

The command returns a Login Succeeded message once completed.


--------


### Example of commands


Get the address server of the registry
```
az acr list --resource-group myResourceGroup
            --query "[].{acrLoginServer:loginServer}"
            --output table
```

Push an image
```
docker push <acrLoginServer>/myImage
```

List images
```
az acr repository list  --name <acrName>
                        --output table

```

Show the specific tags for an image
```
az acr repository show-tags --name <acrName>
                            --repository myImage
                            --output table
```


--------


## Configure ACR authentication
<br/>

To access images stored in ACR, you must grant <br/>the AKS service principal the correct rights to pull images from ACR.

<br/>
Get the ACR Resource ID
```
az acr show --resource-group myResourceGroup
            --name <acrName>
            --query "id"
            --output tsv
```


--------


<br/>
Assign the role **reader** to the AKS Service principal.
<br/>
Replace <appId> and <acrId> with the values gathered in the previous two steps.
```
az role assignment create --assignee <appId>
                          --scope <acrId>
                          --role Reader
```
