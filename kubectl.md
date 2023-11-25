## Aliases / CLI shortcuts
alias k=kubectl
export do='--dry-run=client -o yaml'


k create

k run
  k run example --image=busybox
  k run example --image=busybox do
  k run example --image=busybox do > example.yaml

k edit

k apply