# Deploy Oracle InstantClient Container Image on OCP

I had a need to test connections to an external Oracle DB from Keycloak (running OCP).

This is a very simple deployment of the Oracle InstantClient container image.
Nothing much to see here, it mostly just works.

There is a very basic set of kustomize manifests to deploy to OCP/k8s. You can either launch a deployment (recommended) or simply launch a Pod.

Once deployed you can attempt to access the DB using sqlplus from within the container.


## Connecting with SQLPlus using instantclient container

### RSH to the container
```shell
$ oc rsh instantclient19
sh-4.4#
```

### populate tnsnames.ora

It would be better to use a configmap here.

```shell
sh-4.4# cat /usr/lib/oracle/21/client64/lib/network/admin/tnsnames.ora
AWS =
   (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = sso-2.qwertyuiop.eu-west-2.rds.amazonaws.com)(PORT = 1521))
    (CONNECT_DATA =
      (SID=ORCL)
    )
)
```

### Connect to DB using SQLPlus

```shell
sh-4.4# sqlplus oraadmin@AWS

SQL*Plus: Release 21.0.0.0.0 - Production on Tue Mar 15 11:46:55 2022
Version 21.4.0.0.0

Copyright (c) 1982, 2021, Oracle.  All rights reserved.

Enter password:

Connected to:
Oracle Database 19c Standard Edition 2 Release 19.0.0.0.0 - Production
Version 19.13.0.0.0
```

### Run Query to check DB Existence

```shell
SQL> select name from v$database;

NAME
---------
RHSSO
```