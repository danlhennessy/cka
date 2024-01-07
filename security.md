# Security

Authentication - Variety of auth methods to establish secure connection to API server: Username/Password, LDAP, Certificates etc
Authorisation - RBAC to manage what an entity can do on the cluster

TLS: All communication between control plane components is secured with TLS encryption
All Pod to Pod communication is enabled by default and unencrypted traffic allowed, this can be restricted with networkPolicies.

## Authentication

Auth Options:                                       | apiserver command
- Static Token file                                 | --token-auth-file=user-tokens.csv
- Certificates                                      |
- Identity Services (e.g. Microsoft Entra ID)       |

kube-apiserver has its own server certificate and key, used to validate any incoming requests
A client, for example a human admin or kube-controller-manager, requires a client cert/key pair to communicate with the apiserver

At least one Certificate Authority (CA) is required for the cluster in order to sign the certificates
Example of creating CA key/csr/cert:
    openssl genrsa -out ca.key 2048
    openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr
    openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
Human admin user client key/crt:
    openssl genrsa -out admin.key 2048
    openssl req -new -key admin.key -subj "/CN=kube-admin/O=system:masters" -out admin.csr  # For the user to have admin priveleges, the group system:masters should be included
    openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt
kube-scheduler client key/crt:
    openssl genrsa -out scheduler.key 2048
    openssl req -new -key scheduler.key -subj "/CN=kube-admin/O=system:kube-scheduler" -out scheduler.csr
    openssl x509 -req -in scheduler.csr -CA ca.crt -CAkey ca.key -out scheduler.crt

The admin user crt/key can be used when making http requests to the apiserver e.g.: curl https://kube-apiserver:6443/api/v1/services --key admin.key --cert admin.crt --cacert ca.crt
Or incorporated into a kubeconfig.