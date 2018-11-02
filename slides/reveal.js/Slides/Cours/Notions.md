# Notions


--------


### You will learn about
<br/>
* Create a service principal for resource interactions


--------


### Rappel
<br/>

#### Objets application <br/>et <br/>principal du service dans Azure Active Directory


--------


Le terme « **Application** » est souvent utilisé comme concept :
* faisant référence non seulement au programme d’application,
* mais également à son inscription **Azure AD** et à son rôle lors des « conversations »
 d’authentification/autorisation au moment de l’exécution.


--------


Par définition, une application peut fonctionner dans les rôles suivants :
* Rôle Client (qui utilise une ressource)
* Rôle Serveur de ressources (qui expose les API aux clients)
* Rôle Client et rôle Serveur de ressources


--------


#### Inscription de l’application


Lorsque vous inscrivez une application Azure AD dans le portail Azure,<br/>
deux objets sont créés dans votre client Azure AD :
* un objet application
* un objet principal du service.


--------


#### Objet Application


Une application Azure AD est définie par son seul et unique objet application, qui réside dans le client Azure AD dans lequel l’application a été inscrite, appelé client « de base » de l’application.


--------


#### Objet principal du service
<br/>

Le principal définit la stratégie d’accès et les autorisations <br/>pour l’utilisateur ou l’application du locataire Azure AD.

<br/>Cela rend possibles les fonctionnalités de base, telles que :
* l’authentification de l’application ou de l’utilisateur lors de la connexion,
* l’autorisation lors de l’accès aux ressources.


--------


### Creation d'un service principal


To allow an AKS cluster to interact with other Azure resources, an Azure Active Directory service principal is used


--------


This service principal can be automatically created by the Azure CLI or portal, or you can pre-create one and assign additional permissions


--------


https://docs.microsoft.com/fr-fr/learn/modules/intro-to-security-in-azure/3-identity-and-access

https://docs.microsoft.com/fr-fr/learn/modules/secure-azure-resources-with-rbac/2-rbac-overview
