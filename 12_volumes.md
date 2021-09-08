# Volumes

Data in stored in a Pod is lost, if the Pod dies.
`volumes` are a concept, that allows a Pod to persistently store data.
They are a mount point on the containers file system that points to a storage medium.
A Volume can be shared by multiple containers in a Pod.

- `emptyDir`: An empty Volume is created for the Pod as soon as it is scheduled on the worker node. The Volume's life is tightly coupled with the Pod. If the Pod is terminated, the content of emptyDir is deleted forever.  
- `hostPath`: With the hostPath Volume Type, we can share a directory between the host and the Pod. If the Pod is terminated, the content of the Volume is still available on the host.
- `gcePersistentDisk`: With the gcePersistentDisk Volume Type, we can mount a Google Compute Engine (GCE) persistent disk into a Pod.
- `awsElasticBlockStore`: With the awsElasticBlockStore Volume Type, we can mount an AWS EBS Volume into a Pod. 
- `azureDisk`: With azureDisk we can mount a Microsoft Azure Data Disk into a Pod.
- `azureFile`: With azureFile we can mount a Microsoft Azure File Volume into a Pod.
- `cephfs`: With cephfs, an existing CephFS volume can be mounted into a Pod. When a Pod terminates, the volume is unmounted and the contents of the volume are preserved.
- `nfs`: With nfs, we can mount an NFS share into a Pod.
- `iscsi`: With iscsi, we can mount an iSCSI share into a Pod.
- `secret`: With the secret Volume Type, we can pass sensitive information, such as passwords, to Pods.
- `configMap`: With configMap objects, we can provide configuration data, or shell commands and arguments into a Pod.
- `persistentVolumeClaim`: We can attach a PersistentVolume to a Pod using a persistentVolumeClaim.


The possibilities are diverse.
Kubernetes uses `PersistentVolume`, to abstract these possibilities to a common API.
This is done using a `PersistentVolumeClaim`

## PersistentVolumeClaims

A `PersistentVolumeClaim` is a request for storage by a user.
Theres are three types of access modes:

- `ReadWriteOnce`: r+w for a single node
- `ReadOnlyMany`: r for many nodes
- `ReadWriteMany`: r+w for many nodes

If a PersistentVolume is no longer needed it can be:

- `reclaimed`: verify and/or aggregate data
- `deleted`: data and volume are lost
- `recycled`: only data is lost

As there are more container orchstrators than Kubernetes, storage providers need a common interface, to provide storage for all of them.
This is what the `Container Storage Interface (CSI)` aims to provide.
