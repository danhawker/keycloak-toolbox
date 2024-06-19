# Red Hat build of Keycloak (RHBK)

This directory contains kustomize definitions for the installation of the RHBK Operator and the set up of RHBK accordingly.

RHBK is deployed using a dedicated operator that deploys in a namespace context (rather than globally). Within this directory, there are kustomize definitions for the installation of the Operator in a designated namespace, and the deployment of a RHBK instance in that namespace.

Once deployed, users can configure RHBK/Keycloak using either the standard Keycloak Admin UI or the KeycloakRealmImport CR provided by the Operator.

NOTE: The RHBK Operator, unlike the previous RHSSO operator, cannot manage individual Keycloak resources (eg Realm, Users, Clients) independently, or have a Backup/restore CR. It can only use the RealmImport mechanism. If you want to automate more, use another tool such as Ansible to do so.

NOTE: The operator does NOT necessarily support all Keycloak config options. There be dragons.

## Installation Steps

This example creates and deploys the RHBK operator, RHBK itself and optionally a PostgreSQL database to the `keycloak-system` namespace. I use this for system-wide auth on OpenShift. Adjust to fit your needs.

### Install the Operator using the `oc` CLI

The Operator needs installing first.

An overlay is provided to to deploy the RHBK Operator, targeting version 24.

```
$ oc create -k rhbk/operator/overlays/keycloak-system-24
```

Verify the Operator has been deployed successfully. You should see output similar to the below.

```
$ oc get csv -n keycloak-system
NAME                               DISPLAY                    VERSION         REPLACES        PHASE
rhbk-operator.v24.0.5-opr.1        Keycloak Operator           24.0.5-opr.1                   Succeeded
```

### OPTIONAL: Deploy a PostgreSQL Instance

If you want a persistent RHBK install, you need a database. This overlay will deploy a simple PostgreSQL statefulset and PVC within the `keycloak-system` namespace. It also creates a service and secret to enable RHBK to connect to PostgreSQL.

NOTE: The credentials created by the SecretGenerator in the overlay, are stored in a secret and consumed by both Postgres during database init, and Keycloak when connecting. Adjust to fit your environment.

```
$ oc create -k postgres/overlays/keycloak-system
```

Verify the Postgres StatefulSet has deployed successfully

```
$ oc get pod postgres-rhbk-0 -n keycloak-system
NAME              READY   STATUS    RESTARTS   AGE
postgres-rhbk-0   1/1     Running   0          23s
```

### Deploy RHBK Instance

Decide on if you want a persistent or ephemeral RHBK instance. Use the `keycloak-system` overlay for ephemeral, and `keycloak-system-persistent` overlay for persistent. If wanting persistence, remember to deploy the DB first.

NOTE: You will need to adjust the Keycloak CR to set the correct hostname for your environment. Without this the instance will not launch successfully.

```
  hostname:
    hostname: keycloak.apps.ocp.example.com
    strict: false
    adminUrl: https://keycloak.apps.ocp.example.com
```

```
$ oc create -k rhbk/instances/overlays/keycloak-system-persistence
```

Verify RHBK has deployed successfully

```
$ oc get pod keycloak-system-0 -n keycloak-system
NAME                READY   STATUS    RESTARTS   AGE
keycloak-system-0   1/1     Running   0          1m
```

Discover the route and connect to your instance.
```
$ oc get route -n keycloak-system
NAME                            HOST/PORT                   PATH   SERVICES                  PORT   TERMINATION     WILDCARD
keycloak-system-ingress-27chd   keycloak.apps.gaia.pv.lan          keycloak-system-service   http   edge/Redirect   None
```

RHBK automatically generates a k8s secret containing initial admin credentials to enable admins to login.
