
The role of scheduling pods onto nodes is assigned to the kube scheduler. Without a scheduler, pods will remain in a pending state
Alternatively, pods can be manually scheduled onto specific nodes using the spec.nodeName field. This can only be done at creation time, not applied to an existing pod.
A binding resource is required to schedule an existing pod onto a node.