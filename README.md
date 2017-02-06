# MariaDB Galera Cluster

[MariaDB](https://mariadb.org/) is one of the most popular database servers in the world. It’s made by the original developers of MySQL and guaranteed to stay open source. Notable users include Wikipedia, Facebook and Google.

[Galera Cluster](http://galeracluster.com/) is a multi-master solution for MariaDB which provides an easy-to-use, high-availability solution for MariaDB based databases.

## Introduction

This chart bootstraps a [MariaDB Galera Cluster](http://github.com/adfinis-sygroup/openshift-mariadb-galera) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.5+ with Beta APIs enabled
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release incubator/mariadb-galera
```

The command deploys MariaDB Galera Cluster on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of the MariaDB chart and their default values.

| Parameter                  | Description                                | Default                                                    |
| -----------------------    | ----------------------------------         | ---------------------------------------------------------- |
| `image`                    | MariaDB Galera image                       | `adfinissygroup/k8s-mariadb-galera-centos:v002`            |
| `imagePullPolicy`          | Image pull policy.                         | `IfNotPresent`                                             |
| `mysqlRootPassword`        | Password for the `root` user.              | `nil`                                                      |
| `mysqlUser`                | Username of new user to create.            | `nil`                                                      |
| `mysqlPassword`            | Password for the new user.                 | `nil`                                                      |
| `mysqlDatabase`            | Name for new database to create.           | `nil`                                                      |
| `replicas`                 | Number of replicas to deploy               | 3                                                          |
| `persistence.enabled`      | Use a PVC to persist data                  | `true`                                                     |
| `persistence.storageClass` | Storage class of backing PVC               | `generic`                                                  |
| `persistence.accessMode`   | Use volume as ReadOnly or ReadWrite        | `ReadWriteOnce`                                            |
| `persistence.size`         | Size of data volume                        | `8Gi`                                                      |
| `resources`                | CPU/Memory resource requests/limits        | Memory: `256Mi`, CPU: `250m`                               |

The above parameters map to the env variables defined in the [adfinis-sygroup/openshift-mariadb-galera](http://github.com/adfinis-sygroup/openshift-mariadb-galera).

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
$ helm install --name my-release \
  --set mysqlRootPassword=secretpassword,mysqlUser=my-user,mysqlPassword=my-password,mysqlDatabase=my-database \
    stable/mariadb-galera
```

The above command sets the MariaDB `root` account password to `secretpassword`. Additionally it creates a standard database user named `my-user`, with the password `my-password`, who has access to a database named `my-database`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install --name my-release -f values.yaml stable/mariadb-galera
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Persistence

The [MariaDB Galera Chart](https://github.com/kubernetes/charts/) image stores the MariaDB data files at the `/var/lib/mysql` path of the container.

The chart mounts a [Persistent Volume](kubernetes.io/docs/user-guide/persistent-volumes/) at this location in every pod of the StatefulSet. The volume is either created using dynamic volume provisioning or must be created manually.
