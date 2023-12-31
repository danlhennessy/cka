## DNS

CoreDNS replaced kube-dns as the default DNS service in Kubernetes since v1.11 . Enables DNS-based service discovery, each service and pod is assigned a DNS name which CoreDNS resolves to the appopriate IP address.
Runs generally as a deployment in Kubernetes and can be configured by an included configmap

## Container Networking Interface and plugins

The CNI is used by the container runtime on each node to access a network interface. When the container runtime needs to perform network operations on a container, it calls the CNI plugin with the desired command.
CNI plugins (such as Cilium, Calico) can also provide more advanced networking policies and customization.

## Metrics Server

Metrics server stores resource metrics in memory, to be used only for scaling purposes with HPA and VPA
Deployed as a daemonset

## Dashboard

Web-based UI, alternative/addition to kubectl for troubleshooting and managing kubernetes resources.
