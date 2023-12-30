Application lifecycle management

## Environment variables and secrets

Apply env vars directly into the container, from a configmap, or draw from mounted volumes.

spec:
  containers:
  - name: envar-demo-container
    image: gcr.io/google-samples/node-hello:1.0
    env:
    - name: DEMO_GREETING
      value: "Hello from the environment"
    - name: DEMO_FAREWELL
      value: "Such a sweet sorrow"

## Rolling updates / rollbacks

kubectl rollout deployment <deploymentname>
    The default rollout strategy is RollingUpdate which updates pods in a rolling fashion, you can also use Recreate which will terminate all old pods before creating new ones.