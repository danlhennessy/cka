# Cluster component maintenance

## Node drain / cordon

Drain a node of workloads to move them to other available nodes: kubectl drain <nodename>
The node will be cordoned and pods will not be scheduled to run on the node, until it is uncordoned: kubectl uncordon <nodename>
Cordons can be applied without draining existing workloads, for example to prevent any new pods being scheduled to a node: kubectl cordon <nodename>
Ignore critical daemonsets with the arg: kubectl drain <nodename> --ignore-daemonsets

## Upgrades

Recommended to upgrade versions one minor version at a time, e.g. 1.15 -> 1.16 -> 1.17 ...
    kubectl drain <nodename>
    apt-get upgrade kubelet=1.17.0-00
    systemctl restart kubelet
    kubectl uncordon <nodename>

## Backup / Restore

Backup:
    Resource configurations: kubectl get all -A -oyaml > all.yaml
    ETCD Cluster:
        Find the data directory (Configured in the etcd.service systemd file) and backup all files
        OR
        export ETCDCTL_API=3
        etcdctl snapshot save snapshot.db
        etcdctl snapshot status snapshot.db

Restore:
    ETCD Cluster (systemd services):
        service kube-apiserver stop
        etcdctl snapshot restore snapshot.db --data-dir=/var/lib/etcd-backup
        Adjust etcd.service to use new data dir (Ensure permissions are correct - etcd:etcd)
        systemctl daemon-reload
        service etcd restart
        service kube-apiserver start
    ETCD Cluster (kubeadm pod):
        etcdctl snapshot restore snapshot.db --data-dir=/var/lib/etcd-backup
        Adjust etcd pod to use new host volume path
        Apply pod manifest after adjustments
        Wait for changes to propagate to cluster