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

Resource configurations: kubectl get all -A -oyaml > all.yaml
ETCD Cluster:
    Find the data directory (Configured in the etcd.service systemd file)
    etcdctl snapshot save snapshot.db