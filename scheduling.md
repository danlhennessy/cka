
The role of scheduling pods onto nodes is assigned to the kube scheduler. Without a scheduler, pods will remain in a pending state
Alternatively, pods can be manually scheduled onto specific nodes using the spec.nodeName field. This can only be done at creation time, not applied to an existing pod.
A binding resource is required to schedule an existing pod onto a node.

## Labels / Selectors

Labels and selectors allow you to group kubernetes resources.
Label resources first, e.g. label a pod with environment: development and app: newapp. Then use selectors to reference the resources, e.g. select the pod when creating a service to route traffic to it: app=newapp , environment=development
kubectl can use labels to narrow down results: kubectl get pod --selector app=newapp

## Taints / Tolerations

Used to influence which node a pod is scheduled onto. A taint is applied to a node to prevent pods matching specific criteria from being scheduled onto it.
A toleration is an assigned property of a pod.