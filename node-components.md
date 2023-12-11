## kubelet

Point of contact from each node to the control plane. Responsible for using container runtime (e.g. containerD) to pull and run images.
Also monitors node workloads periodically and reports status to the apiserver

## kube-proxy

When a service is created, kube-proxy is responsible for creating rules on each node, to forward service traffic onto the linked pods. This can be done using IPtables and other methods.
When using kubeadm, kube-proxy is installed as a daemonset, so an instance will run on each node by default.

## container runtime
