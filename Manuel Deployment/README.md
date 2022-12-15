# Couchbase-Kubernetes

This chart provide the Couchbase deployments.
Note: It is strongly recommend to use on Official Couchbase image to run this chart.

## Create Namespace
```bash
$ kubectl create ns couchbase-cluster
```
## Create secret 
```bash
$ kubectl create secret generic couchbase-administrator -n couchbase-cluster --from-literal='username=Adminisitrator' --from-literal='password=p@ssword'
```
## Edit `values.yaml` file
```bash
$ kubectl apply -f values.yaml 
```
## Edit `ingress.yaml` file
```bash
$ kubectl apply -f ingress.yaml
```
