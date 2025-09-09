# Container Storage Is More Than Persistent Volumes (Moritz Wanzenböck)

started as a sales pitch for linbit/piraeus storage. then a few interesting features were presented:

- Changed Block Tracking (CBT) incorporated into the CSI spec -> permits to speed up incremental backups <https://github.com/kubernetes/enhancements/pull/4082>
- VolumeSnapshotGroup -> consistent backup for multiple PVC at once <https://kubernetes.io/blog/2024/12/18/kubernetes-1-32-volume-group-snapshot-beta/>
- Ephemeral volumes -> better/faster emptyDir
- VolumePopulators -> write data to a PVC on creation from any `dataSource` <https://kubernetes.io/blog/2025/05/08/kubernetes-v1-33-volume-populators-ga/>

piraeus storage provider is a cncf sandbox project and sounds interesting after
all. uses Distributed Replicated Block Device 9 kernel module:
<https://piraeus.io/>
