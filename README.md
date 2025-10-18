# keycloak-toolbox
Collated Keycloak Utils, Container Image Builds and Kustomize manifests for deployment.

## Containers

Series of Custom Image builds for RHSSO.

### luna-client
Simple client container to enable a Kubernetes Pod to connect to a Thales/Safenet Luna HSM. Used to test the RHSSO/Keycloak plugin for the Luna.

### rhsso-oracle
Container build using a standard RHSSO 7.x container as base, but overlaying the Oracle JDBC driver for those using OracleDB wih SSO.


## Kustomize

### RHBK

Quick example of deploying Red Hat Build of Keycloak (RHBK) to OpenShift using the RHBK Operator, using kustomize manifests. An ephemeral and a persistent version is provided. Persistence requires a PostgreSQL DB to be deployed and ready to connect.

### PostgreSQL

Deploys a very simple single pod PostgreSQL deployment, using a `StatefulSet` and `PersistentVolumeClaim` using default `StorageClass`. Adjust the StorageClass used if needed.
The database is exposed internally via a headless `Service`.

### Oracle InstantClient

Set of kustomize manifests for a simple deployment of Oracle InstantClient container on OCP/k8s, to verify Container to OracleDB connectivity, and manage of schemas.
