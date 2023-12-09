## Aliases / CLI shortcuts
alias k=kubectl


Create:
k create -f FILENAME
k create <resourcetype> <resourename> args
k create deploy NAME --image=image -- [COMMAND] [args...]
  k create deploy test --image=nginx -r 3 --dry-run=client -o yaml
  k create deploy nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml

Run:
k run NAME --image=image [--env="key=value"] [--port=port] [--dry-run=server|client] [--overrides=inline-json] [--command] -- [COMMAND] [args...]
  k run example --image=busybox
  k run example --image=busybox --dry-run=client -o yaml
  k run example --image=busybox --dry-run=client -o yaml > example.yaml

k apply
  k apply -f filename.yaml
  k apply -f filename.yaml -n my-namespace

k edit
  k edit deploy/my-deployment

k replace
  k replace -f my-deployment.yaml

k delete
  k delete pod example
  k delete pod example --grace-period 0 --force

k get
  k get po -A
  k get deploy my-deployment -n my-namespace

k describe
  k describe cm my-configmap -n my-namespace

k scale
  k scale rs my-replicaset --replicas=3