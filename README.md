# keycloak-toolbox
Collated Keycloak Utils, Container Image Builds and Kustomize manifests for deployment.

## Containers

Series of Custom Image builds for RHSSO.

.luna-client
Simple client container to enable a Kubernetes Pod to connect to a Thales/Safenet Luna HSM.

.rhsso-oracle
Container build using a standard RHSSO 7.x container as base, but overlaying the Oracle JDBC driver for customers using OracleDB wih SSO.

## Kustomize

Quick example of deploying Red Hat Build of Keycloak (RHBK) to OpenShift using the RHBK Operator, using kustomize manifests. A ephemeral and a persistent version is enclosed, with an additional manifest for a persistent PostgreSQL deployment for the latter.

### 389ds

In addition there is a series of kustomize manifests to deploy 389 Directory Server to Kubernetes/OpenShift. If you need a quick and easy LDAP server to be used as an external User datastore for Keycloak, 389ds is an excellent choice. Persistent and non-persistent overlays are available.



