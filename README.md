# Kubeshark playground
Playing with [kubeshark](https://kubeshark.co/)

# Prerequisites

## Create a resource group

```shell
az group create --name <resource-group-name> --location northeurope
```

## Create an SSH key pair using Azure CLI
    
```shell
az sshkey create --name "aksSSHKey" --resource-group "<resource-group-name>"
```

## Create an SSH key pair using ssh-keygen
```shell
ssh-keygen -t rsa -b 4096
```

# Create an AKS cluster

* DNS prefix: Enter a unique DNS prefix for your cluster, such as myakscluster.
* Linux Admin Username: Enter a username to connect using SSH, such as azureuser.
* SSH RSA Public Key: Copy and paste the public part of your SSH key pair (by default, the contents of ~/.ssh/id_rsa.pub).

```shell
az deployment group create --resource-group rg-aks-basic --template-file aks.bicep --parameters clusterName='<aks-cluster-name>' dnsPrefix=<dns-prefix> linuxAdminUsername=<linux-admin-username> sshRSAPublicKey='<ssh-key>'
```

## Connect to the cluster

```shell
az aks get-credentials --resource-group rg-aks-basic --name <aks-cluster-name>
```

# Deploy a sample application
The manifest includes two Kubernetes deployments:

1. The sample Azure Vote Python applications.
2. A Redis instance.

Two Kubernetes Services are also created:

1. An internal service for the Redis instance.
2. An external service to access the Azure Vote application from the internet.

Create namespace
```shell
kubectl create namespace av
```

```shell
kubectl apply -f manifest/azure-vote.yaml
```

## Test the application

```shell
kubectl get service azure-vote-front --watch
```

# Kubeshark
[kubeshark](https://docs.kubeshark.co/en/introduction)

[tap command](https://docs.kubeshark.co/en/network_sniffing#the-tap-command)

* Install
```shell
.\kubeshark.exe tap -n av
```

* Open dashboard
```shell
.\kubeshark.exe proxy
```

* Clean up
```shell
.\kubeshark.exe clean
```


# Clean up resources

```shell
az group delete --name rg-aks-basic --yes --no-wait
```