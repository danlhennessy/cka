# Components

## etcd

Key-Value store as opposed to the usual table format of relational databases
Stores the state of a kubernetes cluster, acts as the "source of truth"
Runs as a standard binary. Default port: 2379
cli tool: etcdctl
Built on the Raft consensus algorithm, which is designed to maintain data consistency on each node in a cluster.

etcd is part of the kubernetes control plane, and the watch function is what allows kubernetes to react to drift from desired state
Watch stores values representing the actual and desired state of the cluster and can initiate a response when drift is detected

## kube-apiserver

kubectl commands are sent to the apiserver. Commands are authenticated, validated, and resource changes are updated in etcd. The apiserver can also be queried using HTTP requests
It is the only control plane component that communicates directly with etcd

An example process for a pod being created via a command to the apiserver:
- kubectl command to the API server requesting a new pod ` kubectl run example --image=busybox`
- apiserver first authenticates, then validates the request
- apiserver creates a pod object which is yet not assigned to a node, and updates etcd and the user to confirm the pod has been created
- kube-scheduler monitors the apiserver and finds an unassigned pod. It schedules the pod onto the correct node and confirms this back to apiserver
- apiserver updates etcd and kubelet on the chosen node.
- The kubelet creates the pod, and instructs the container runtime engine to deploy the busybox image. Kubelet updates the apiserver with the results
- apiserver updates etcd with the final state