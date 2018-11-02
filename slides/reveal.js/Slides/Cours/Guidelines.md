# GUIDELINES


--------


## cost optimization


--------


### Cluster Autoscaler


--------


#### Nodes scaling

* Can reduce the cost of the cluster significantly
* You define a minimum number of nodes
* Keeping the cluster from scaling its nodes to the limit of 100,
* Define a maximum number of nodes, when traffic increases
* The CA checks every 10 seconds for pending pods or empty nodes
* Nodes are removed from the cluster, if they are unneeded for more than 10 minutes


--------


### Horizontal Pod Autoscaler


--------


Autoscaling option based on metrics like
* CPU
* memory
* or network utilization`


--------


## Pods scaling


* Scale between the defined range of minimum pods
* CA and HPA are working well together
* General recommendation is to enable both autoscaler options in your AKS cluster.
* Covering short peaks via the HPA to scale out only the application
* Longer peaks in conjunction with the CA provisioning additional nodes


--------


## Azure Container <br/>Instances connector


--------


### The good part

* The ACI connector comes in as a Virtual Kubelet
* Virtual agent node with nearly unlimited capacity depending on your Azure subscription limits
* It is not a replacement for the HPA, but can be used as a replacement for the CA
* When the CA is adding nodes to the AKS cluster, it takes time until the nodes are provisioned and ready for pod deployment
* On ACI new pods can be spin up instantly.


--------


#### The bad part

* The only downside currently is that ACI does not have an integration into the VNET the AKS cluster is sitting in.
* That limits its capability only to applications/pods that do not need to communicate to other applications/pods in the cluster.


--------


## Node - VM sizes


--------


Selecting the correct VM size can have a significant impact on the operational cost of an AKS cluster


https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes


--------


### production


* The general recommendation for production clusters are the VM series Dsv3 and Esv3 that are SSD backed
* Most workloads can be covered by the VM sizes
  * Standard_D2s_v3 with 2 vCPUs and 8 GB memory
  * Standard_D4s_v3 with 4 vCPUs and 16 GB memory.
* If you need a higher vCPU to memory ratio
  * Standard_E2s_v3 with 2 vCPUs and 16 GB memory
  * Standard_E4s_v3 with 4 vCPUs and 32 GB memory



-------


### Development


AKS clusters for dev/test can be operated with VM sizes
* Standard_B2ms
* Standard_B4ms
* Standard_B8ms

Goal is to reduce runtime cost compared against the same sizes out of the Dv3-series
