# Containers Orchestrator hands-on lab with Kubernetes

## Create and deploy into Kubernetes Namspaces

Kubernetes provides namespaces as a way to create isolated environments within a cluster (e.g. dev,test,prod)

<https://github.com/lastcoolnameleft/kubernetes-workshop/blob/master/yaml/k8s-create-namespaces.yaml>

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: test
  labels:
    name: test
---
apiVersion: v1
kind: Namespace
metadata:
  name: prod
  labels:
    name: prod
```

Create a test and prod namespace deployment using the kubectl create command:

```shell
kubectl create -f https://raw.githubusercontent.com/lastcoolnameleft/kubernetes-workshop/master/yaml/k8s-create-namespaces.yaml
```

Use the UI or command line to verify the available namespaces now include test and prod:

```shell
kubectl get namespaces
```

Deploy the demo application into the test namespace

- Get a list of the current contexts
  - Context is a tuple of {Cluster, User, Namespace}
- Create a context named test and bind it to the test namespace
- Set the current context to test
- Show the current context again and note that test is tagged as CURRENT
- Deploy the application into the test namespace

```shell
kubectl config get-contexts
kubectl config set-context test --cluster=$RESOURCE_GROUP --user=$USER --namespace=test
kubectl config use-context test
kubectl config get-contexts
```
