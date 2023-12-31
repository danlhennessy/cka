Application lifecycle management

## Environment variables and secrets

Apply env vars / secrets directly into the container, from a configmap, or draw from mounted volumes.

spec:
  containers:
  - name: envar-demo-container
    image: gcr.io/google-samples/node-hello:1.0
    env:
    - name: DEMO_GREETING
      value: "Hello from the environment"
    - name: DEMO_FAREWELL
      value: "Such a sweet sorrow"

kubectl create configmap myconfig --from-literal=<key>=<value> --from-literal=<key2>=<value2>
kubectl create configmap myconfig --from-file=<filepath>

spec:
  containers:
    - name: test-container
      image: registry.k8s.io/busybox
      envFrom:
      - configMapRef:
          name: myconfig

## Rolling updates / rollbacks

kubectl set image deployment/<deploymentname> <containername>=<imagename:tag>
kubectl rollout status deployment <deploymentname>
kubectl rollout undo deployment <deploymentname>
    The default rollout strategy is RollingUpdate which updates pods in a rolling fashion, you can also use Recreate which will terminate all old pods before creating new ones.