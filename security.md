# Security

Authentication - Variety of auth methods to establish secure connection to API server: Username/Password, OAUTH, Certificates etc
Authorisation - RBAC to manage what an entity can do on the cluster

TLS: All communication between control plane components is secured with TLS encryption
All Pod to Pod communication is enabled by default and unencrypted traffic allowed, this can be restricted with networkPolicies.