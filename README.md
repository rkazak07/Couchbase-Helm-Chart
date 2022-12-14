# Couchbase-Kubernetes

## Create Namespace
$ kubectl create ns couchbase-cluster

## Create secret 
$ kubectl create secret generic couchbase-administrator -n couchbase-cluster --from-literal='username=Adminisitrator' --from-literal='password=p@ssword'

## Edit `values.yaml` file
$ kubectl apply -f values.yaml 

## Edit `ingress.yaml` file
$ kubectl apply -f ingress.yaml
