# Components Overview
https://kubernetes.io/docs/concepts/overview/components/

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

## etcd

Key-Value store as opposed to the usual table format of relational databases
Stores the state of a kubernetes cluster, acts as the "source of truth"
Runs as a standard binary. Default port: 2379
cli tool: etcdctl
Built on the Raft consensus algorithm, which is designed to maintain data consistency on each node in a cluster.

etcd is part of the kubernetes control plane, and the watch function is what allows kubernetes to react to drift from desired state
Watch stores values representing the actual and desired state of the cluster and can initiate a response when drift is detected

## kube scheduler

Decides which pod is scheduled onto which node, and communicates this to the apiserver. The apiserver will forward this to the kubelet on the relevant node for the action to be taken.
The scheduler uses a priority ranking to determine the most suitable node for a requested pod. This can be influenced by configuring the scheduler itself, or by applying taints/tolerations

## kube controller manager

Controllers watch for the status of specific resources, for example a service controller will constantly monitor services in the cluster, and take action to restore services to the desired state. When taking action it will communicate via the apiserver
There are built in controllers for the default kubernetes resources, and custom controllers to deal with custom resources (CRDs)
kube controller manager is a single process containing all controllers

## cloud-controller-manager

An optional component of the control plane that allows separation of resources that interact with the specific cloud platform.
It deals with the connection between the cluster and Cloud providers API. Contains multiple controllers running as a single process: Node, Route and Service - responsible for managing cloud server and networking services.