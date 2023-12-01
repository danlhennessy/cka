# Components

## ETCD

Key-Value store as opposed to the usual table format of relational databases
Stores the state of a kubernetes cluster, "Source of truth"
Runs as a standard binary. Default port: 2379
cli tool: etcdctl
Built on the Raft consensus algorithm, which is designed to maintain data consistency on each node in a cluster.

etcd is well known for its inclusion in kubernetes control planes, and the watch function is what allows kubernetes to react to drift from desired state
Watch stores values representing the actual and desired state of the cluster and can initiate a response when drift is detected