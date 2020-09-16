# Node Red Kubernetes Setup

[Node-Red](https://nodered.org/) is a programming tool for wiring together hardware devices, APIs and online services in new and interesting ways. 

## TL;DR;

```bash
$ git clone https://scm.dimensiondata.com/Wei-Xiang.Koh/node-red-kubernetes-setup.git
$ kubectl apply -f node-red-setup.yml
```

## Prerequisites

- Tested working on Kubernetes v1.18.6
- Persistent storage class

## Installing the deployment

To install the deployment, services and PVC in namespace `node-red`:

```bash
$ git clone https://scm.dimensiondata.com/Wei-Xiang.Koh/node-red-kubernetes-setup.git
$ kubectl create namespace node-red
$ kubectl apply -f node-red-setup.yml
```

The command deploys node-red on the Kubernetes cluster in the default configuration.
Any customization would require changing the parameters in the deployment file before applying to kubernetes cluster
The [configuration](#configuration) section lists the parameters that can be configured during installation.

## Uninstalling Node-red

To uninstall/delete the `node-red` namespace:

```bash
$ kubectl delete namespaces node-red
```

The command removes all the Kubernetes components associated with the node-red and deletes the persistent disk.


|            Component              |                Description                    |       Default        |
|-----------------------------------|-----------------------------------------------|----------------------|
| `persistence.storageClass`        | Kubernetes Storage Class                      | `fast`               |
| `persistence.size`                | Number of GB to request from storage class.   | `5Gi`                |
| `service.type`                    | Kubernetes service type to access node-red UI | `LoadBalander`       |
| `deployment.replicas`             | Number of replica                             | `1`                  |

To change the parameters do update the default deployment file with your own values


## Persistence


The [node-red](https://nodered.org/docs/getting-started/docker#managing-user-data) image stores the user configuration and palettes data at the `/data` path of the container.

The deployment mounts a [Persistent Volume](kubernetes.io/docs/user-guide/persistent-volumes/) volume at this location. The volume is created using dynamic volume provisioning.