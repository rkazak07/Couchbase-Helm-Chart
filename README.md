# Couchbase

This chart provide the [Couchbase][1] deployments.
Note: It is strongly recommend to use on Official Couchbase image to run this chart.


## Quick Installation


```bash
kubectl create namespace couchbase-cluster

helm install my-release . -f values.yaml -n couchbase-cluster
```

## Install Chart

To install the Couchbase Chart into your Kubernetes cluster (This Chart requires persistent volume by default, you may need to create a storage class before install chart.

```bash
helm install my-release . -f values.yaml -n couchbase-cluster \
  --set Username=Administrator \
  --set Password=ChangePassword \
  --set replicaCount=3 \
  --set storageClass=local-path
```

After installation succeeds, you can get a status of Chart

```bash
helm status my-release -n couchbase-cluster
```

If you want to delete your Chart, use this command

```bash
helm delete my-release -n couchbase-cluster
```

### Using node selector

For example, you have 3 vms in node pools and you want to deploy graylog to node which labeled as `cloud.google.com/gke-nodepool: couchbase-pool`
Set the following values in `values.yaml`

```yaml
   nodeSelector: { cloud.google.com/gke-nodepool: couchbase-pool }
```

### Using tolerations

For example, you have 6 vms in node pools and 3 nodes are tainted with `NO_SCHEDULE couchbase=true`
Set the following values in `values.yaml`

```yaml
  tolerations:
    - key: couchbase
      value: "true"
      operator: "Equal"
```

## Configuration

The following table lists the configurable parameters of the Couchbase chart and their default values.

| Parameter                                      | Description                                                                                                                                           | Default                           |
|------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------|
| `image.repository`                     | `couchbase` image repository                                                                                                                            | `couchbase/server`                 |
| `image.tag`                            | `couchbase` image tag                                                                                                                                   | `community-7.1.1`                         |
| `image.pullPolicy`                     | Image pull policy                                                                                                                                     | `IfNotPresent`                    |
| `replicas`                             | The number of Couchbase instances in the cluster. The chart will automatic create assign master to one of replicas                                      | `3`                               |
| `resources`                            | CPU/Memory resource requests/limits                                                                                                                   | Memory: `1024Mi`, CPU: `500m`     |
| `nodeSelector`                         | couchbase server pod assignment                                                                                                                         | `{}`                              |
| `affinity`                             | couchbase server affinity                                                                                                                               | `{}`                              |
| `tolerations`                          | couchbase server tolerations                                                                                                                            | `[]`                              |
| `nodeSelector`                         | couchbase server node selector                                                                                                                          | `{}`                              |
| `env`                                  | couchbase server env variables                                                                                                                          | `{}`                              |
| `podSecurityContext`                   | Set security context for defining privilege and accessing control settings entire Pod                                                                 | `{}`                              |
| `securityContext`                      | Set security context for defining privilege and accessing control settings for couchbase container                                                      | `privileged: false`               |
| `service.type`                         | Kubernetes Service type                                                                                                                               | `ClusterIP`                       |
| `service.port`                         | couchbase Service port                                                                                                                                  | `9000`                            |
| `podAnnotations`                       | Kubernetes Pod annotations                                                                                                                            | `{}`                              |
| `terminationGracePeriodSeconds`        | Pod termination grace period                                                                                                                          | `120`                             |
| `persistence.enabled`                  | Use a PVC to persist data                                                                                                                             | `true`                            |
| `persistence.storageClass`             | Storage class of backing PVC (uses storage class annotation)                                                                                          | `nil`                             |
| `persistence.accessMode`               | Use volume as ReadOnly or ReadWrite                                                                                                                   | `ReadWriteOnce`                   |
| `persistence.size`                     | Size of data volume                                                                                                                                   | `10Gi`                            |
| `ingress.enabled`                      | If true, Couchbase Ingress will be created                                                                                                              | `false`                           |
| `ingress.ingressClassName`             | couchbase Ingress class name                                                                                                                            |                                   |
| `ingress.port`                         | couchbase Ingress port                                                                                                                                  | `false`                           |
| `ingress.annotations`                  | couchbase Ingress annotations                                                                                                                           | `{}`                              |
| `ingress.hosts`                        | couchbase Ingress host names                                                                                                                            | `[]`                              |
| `ingress.tls`                          | couchbase Ingress TLS configuration (YAML)                                                                                                              | `[]`                              |
| `ingress.pathType`                     | couchbase Ingress path type                                                                                                                             | `Prefix`                          |
| `Username`                             | couchbase Admin user name                                                                                                                                | `admin`                           |
| `Password`                             | couchbase Admin password. If not set, random 16-character alphanumeric string                                                                            |                                   |
| `existingRootSecret`                   | Couchbase existing Administrator secret                                                                                                                          |                                   |
| `serviceAccount.create`                        | If true, create the Couchbase service account                                                                                                           | `true`                            |
| `serviceAccount.name`                          | Name of the server service account to use or create                                                                                                   | `{{ fullname }}`          |
| `serviceAccount.annotations`                   | Service Account annotations                                                                                                                           | `{}`                              |
| `imagePullSecrets`                             | Configuration for [imagePullSecrets][3] so that you can use a private registry for your images                                                        | `[]`                              |



Note: All uncommitted logs will be permanently DELETED when this value is true

[1]: https://www.couchbase.com/
[2]: https://kubernetes-sigs.github.io/aws-alb-ingress-controller/guide/ingress/annotation/#actions
[3]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-pod-that-uses-your-secret
[4]: https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/
[5]: https://kubernetes.io/docs/tasks/access-application-cluster/create-external-load-balancer/#preserving-the-client-source-ip
