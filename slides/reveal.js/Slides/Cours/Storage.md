# Storage


--------


* PV : PersistentVolume
* PVC : PersistentVolumeClaim


--------



* A PV represents a piece of storage that has been provisioned for use with Kubernetes pods
* A persistent volume can be used by one or many pods, and can be provisioned
  * dynamically
  * statically


--------


Two great families:
* Azure DISK
* Azure FILES


--------

#### notes
<br/>

An Azure disk can only be mounted <br/>with Access mode type **ReadWriteOnce**,
<br/>which makes it available to only a **single pod** in AKS.

<br/>
<br/>
If you need to share a persistent volume across multiple pods,<br/> use Azure Files.



--------


## AZURE DISK


--------


### Azure Unmanaged Disk Storage Class


```
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: slow
provisioner: kubernetes.io/azure-disk
parameters:
  skuName: Standard_LRS
  location: eastus
  storageAccount: azure_storage_account_name
```

* **skuName** : Azure storage account Sku tier. Default is empty.
* **location** : Azure storage account location. Default is empty.
* **storageAccount** : Azure storage account name.
  * If a storage account is provided, it must reside in the same resource group as the cluster, and location is ignored.
  * If a storage account is not provided, a new storage account will be created in the same resource group as the cluster.


--------


###  New Azure Disk Storage Class <br/>(starting from v1.7.2)


```
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: slow
provisioner: kubernetes.io/azure-disk
parameters:
  storageaccounttype: Standard_LRS
  kind: Shared
```


--------



* **storageaccounttype** : Azure storage account Sku tier. Default is empty.  
* **kind** : Possible values are
  * **shared** (default) : all unmanaged disks are created in a few shared storage accounts in the same resource group as the cluster.
  * **dedicated** :  a new dedicated storage account will be created for the new unmanaged disk in the same resource group as the cluster
  * **managed** : all managed disks are created in the same resource group as the cluster.


--------


## AZURE FILES


```
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  skuName: Standard_LRS
  location: eastus
  storageAccount: azure_storage_account_name
```


--------


* **skuName**: Azure storage account Sku tier. Default is empty.
* **location**: Azure storage account location. Default is empty.
* **storageAccount**: Azure storage account name. Default is empty.
  * If a storage account is not provided, all storage accounts associated with the resource group are searched to find one that matches skuName and location.
  * If a storage account is provided, it must reside in the same resource group as the cluster, and skuName and location are ignored.


--------


### WARNING


During provision, a secret is created for mounting credentials. If the cluster has enabled both RBAC and Controller Roles, add the create permission of resource secret for clusterrole system:controller:persistent-volume-binder.




## PersistentVolumeClaims
