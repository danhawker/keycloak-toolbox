# Luna Init

Experimental init container to enable a Kubernetes Pod to connect to a Thales/Safenet Luna HSM.

## Building

:warning: You will need to provide the Luna client to build this container image. Download from Thales and extract to the lunasrc directory.

```
$ podman build . -t luna-init:<tag_name>
```

I usually tag based on the Luna client version used, eg 7.11

## Running As-Is

You can test the container as-is to check connectivity to the HSM. 

```
$ podman run -it --rm --name luna-init localhost/luna-init:7.11 /bin/bash
[root@490a4ae3212c /]# /opt/safenet/lunaclient/bin/lunacm
lunacm (64-bit) v7.11.1-5 (7.11.1-5-ga24a9e8). Copyright (c) 2020 SafeNet Assured Technologies, LLC. All rights reserved.


	Available HSMs:


	Current Slot Id: None

lunacm:>exit

```

## Usage within Kubernetes

:warning: Not quite ready for prime time yet!! Need to document and upload sanitised instructions.

This repo has a sample script `connect2HSM.sh` that can be used to connect to the HSM if appropriate certificates have been created and distributed in the container, and the container has been registered with the HSM.

Adjust the script and add the images as an initContainer to a Deployment or StatefulSet.


