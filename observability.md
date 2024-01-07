## Logs

The stdout logs of containers inside pods is accessible with kubectl logs <podname> <containername>

## Metrics

Metrics server stores resource metrics in memory, to be used only for scaling purposes with HPA and VPA
Deployed as a daemonset so will run on each node