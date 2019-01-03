# Containers Orchestrator hands-on lab with Kubernetes

Azure provides 2 main ways of creating and deploying a Kubernetes cluster.

* Azure Kubernetes Service (AKS)
* Azure Container Service Engine (ACS-Engine)

This walkthrough will focus on creating an AKS cluster.

## Outline

1. Download and install Azure CLI
1. Logging in to your Azure subscription
1. Create the Kubernetes cluster
1. Install Kubectl command line
1. Download the K8s cluster configuration
1. Test your Kubernetes installation

## Details

* You can create a cluster through CLI v2 or the Azure Portal. If you use the portal you will need to have created and provide a SSH key and Service Principal. If you use the command line you can have the SSH key and Service Principal created for you. For this tutorial, we will use the command line.
* For production deployments you would want to create and managed keys and Service Principals for specific purposes. To quickly create a cluster for demo/development purposes you can use the command line which will auto create:
  * SSH keys - in your home/.ssh directory
  * Service Principal - in your home/.azure directory
> Note: If you already have ssh keys in your home directory, then you should use those keys on the command line rather then allowing the CLI to create new keys which will overwrite any existing keys in your home/.ssh directory.
> Note: It is recommended to perform these steps in Bash (Users have experienced issues in PowerShell)

The following steps will create the Kubernetes cluster using command line commands:

### Download and install Azure CLI

If you don’t have it installed locally, follow the guide [here](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) to install Azure CLI v2 on your local machine.

### Login to your Azure subscription and create a new resource group

*NOTE: For simplicity's sake, we will use the same name for the Resource Group and the DNS Prefix.  To prevent DNS name collisions, make sure to suffix RESOURCE_GROUP with your username.*  e.g. `RESOURCE_GROUP=my-k8s-cluster-thfalgou`

```shell
RESOURCE_GROUP=my-k8s-cluster-$USER
LOCATION=westus2
az group create --name $RESOURCE_GROUP --location $LOCATION
```

### Create the Kubernetes cluster

```shell
az aks create --resource-group $RESOURCE_GROUP --name $RESOURCE_GROUP --node-count 3 --generate-ssh-keys --kubernetes-version "1.11.5"
```

The above command will use ACS to create a new Kubernetes cluster named "my-k8-cluster" within the newly created resource group. The orchestrator-type parameter indicates to ACS that you are creating a kubernetes cluster with a dns parameter and to generate new ssh keys and service principals.

### Install Kubectl command line

If not already installed, you can use the cli to install the k8 command line utility (kubectl).
> Note: On Windows you need to have opened the command windows with Administrator rights as the installation tries write the program to "C:\Program Files\kubectl.exe". You may also have to add “C:\Program Files” to your PATH

```shell
az aks install-cli
```

### Download the k8s cluster configuration (including credentials)

The kubectl application requires configuration data which includes the cluster endpoint and credentails. The credentails are created on the cluster admin server during installation and can be downloaded to your machine to the `$HOME/.kube/config` directory using the get-credential subcommand.

```shell
az aks get-credentials --resource-group=$RESOURCE_GROUP --name=$RESOURCE_GROUP
```

### Test your Kubernetes Installation

After downloading the cluster configuration you should be able to connect to the cluster using kubectl. For example the cluster-info command will show details about your cluster.

```shell
kubectl cluster-info
```
