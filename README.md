# PV_PVC_STORAGE-CLASS

A PersistentVolume (PV) is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes. It is a resource in the cluster just like a node is a cluster resource. PVs are volume plugins like Volumes, but have a lifecycle independent of any individual Pod that uses the PV. This API object captures the details of the implementation of the storage, be that NFS, iSCSI, or a cloud-provider-specific storage system.

A PersistentVolumeClaim (PVC) is a request for storage by a user. It is similar to a Pod. Pods consume node resources and PVCs consume PV resources. Pods can request specific levels of resources (CPU and Memory). Claims can request specific size and access modes (e.g., they can be mounted ReadWriteOnce, ReadOnlyMany or ReadWriteMany)

A Kubernetes StorageClass is a Kubernetes storage mechanism that lets you dynamically provision persistent volumes (PV) in a Kubernetes cluster. Kubernetes administrators define classes of storage, and then pods can dynamically request the specific type of storage they need

---
## Lifecycle for PV and PVC

In Kubernetes, the lifecycle of a Persistent Volume (PV) and its associated Persistent Volume Claim (PVC) follows a set of well-defined stages. Here's an overview of the typical lifecycle of a PV and a PVC:

- Provisioning (PV): The provisioning of a PV is the initial stage. Cluster administrators typically set up PVs, either statically by creating them manually or dynamically using a storage provisioner. When a PV is provisioned, it is created but remains in an "Available" state, meaning it's ready for use but hasn't been claimed by a PVC yet.

- Creating a PVC: Application developers create a PVC by defining it in a YAML file and specifying details such as storage size, access mode, and optionally, a storage class. The PVC is then deployed to the cluster.

- Binding (PVC): When a PVC is created, Kubernetes attempts to find a suitable PV that matches the PVC's requirements (capacity, access mode, and storage class). If a matching PV is available, the PVC enters the "Bound" state, and it is bound to the PV.

- Using the Claim: With the PVC bound to a PV, it can be used within pods. Pods are defined to use the PVC as a volume, and the storage from the associated PV is mounted to the pod. The application running in the pod can read from and write to this storage.

- Expanding or Modifying the Claim (PVC): If additional storage capacity is required or if the access mode needs to be changed, the PVC can be modified to reflect the new requirements. Note that some changes, such as access mode changes, may not be possible without recreating the PVC.

- Releasing the Claim (PVC): When the application or workload no longer needs the PVC, it can be deleted. Deleting the PVC does not immediately delete the associated data; it only releases the claim to the PV.

- Reclaim Policy (PV): Depending on the reclaim policy specified when the PV was created, the data associated with the PV is handled differently:

  `Retain`: Data is not deleted, and the PV remains in an "Released" state.

  `Recycle` (deprecated): Data is deleted, but it's not securely wiped. This reclaim policy is not recommended.

  `Delete`: Data is securely deleted, and the PV is made "Available" for reuse.

- Reclaiming the PV: After a PVC is deleted and the PV's data is safely handled according to the reclaim policy, the PV returns to the "Available" state and can be bound to a new PVC.

---
## Access Modes of PV

- ReadWriteOnce (RWO): This access mode allows the volume to be mounted as read-write by a single node (pod). In other words, only one pod running on one node can write to the volume, while other pods on different nodes can only mount it as read-only. RWO is typically used for scenarios where data must not be shared between multiple pods.

= ReadOnlyMany (ROX): The ReadOnlyMany access mode allows the volume to be mounted as read-only by multiple nodes (pods). Multiple pods across different nodes can read data from the volume but cannot write to it. This is useful for scenarios where you want multiple pods to share the same read-only data, such as for scalability or redundancy.

= ReadWriteMany (RWX): RWX is the most permissive access mode, allowing the volume to be mounted as read-write by multiple nodes (pods) simultaneously. All pods, regardless of which node they are running on, can both read from and write to the volume. This access mode is suitable for scenarios where data needs to be shared and modified by multiple pods running on different nodes.



