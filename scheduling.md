
The role of scheduling pods onto nodes is assigned to the kube scheduler. Without a scheduler, pods will remain in a pending state
Alternatively, pods can be manually scheduled onto specific nodes using the spec.nodeName field. This can only be done at creation time, not applied to an existing pod.
A binding resource is required to schedule an existing pod onto a node. By default, the scheduler will attempt to load each node equally with pods.

## Labels / Selectors

Labels and selectors allow you to group kubernetes resources.
Label resources first, e.g. label a pod with environment: development and app: newapp. Then use selectors to reference the resources, e.g. select the pod when creating a service to route traffic to it: app=newapp , environment=development
kubectl can use labels to narrow down results: kubectl get pod --selector app=newapp

## Taints / Tolerations

Used to influence which node a pod is scheduled onto. A taint is applied to a node to prevent pods matching chosen criteria from being scheduled onto it. By default the master node is tainted to prevent new workload pods from being scheduled onto it.
A toleration is an assigned property of a pod.
Taint 
  kubectl taint nodes <nodename> key=value:<tainteffect>
  tainteffects: NoSchedule = will not schedule new pods, PreferNoSchedule = scheduler will try to avoid scheduling new pods, NoExecute = will not schedule new pods and will evict existing pods that do not match taint.
Tolerations
  spec:
    tolerations:
    - key: "env"
      operator: "Equal"
      value: "dev"
      effect: "NoExecute"

## Node Selectors / Affinity

A nodeSelector can be applied to a pod to reference a label on a specific node. e.g. sku: memoryOptimised. The pod will only be scheduled onto nodes with matching labels.
Node affinity /anti affinity is used for more complicated scheduling requirements. e.g:
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: sku
            operator: In
            values:
            - memoryOptimised
            - storageOptimised

A pod with above spec will only be scheduled onto nodes with labels: sku: memoryOptimised OR sku: storageOptimised. Alternatively you can use the operator: Exists just to check if the label exists with any value on a node.
If there are no nodes matching the affinity, the pod will stay pending.