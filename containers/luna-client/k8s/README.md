== Luna Client k8s

This directory contains some basic k8s resource definitions that can launch the luna-client container as detailed in this repo.

.simple_luna_deployment.yaml
A simple k8s deployment

.luna-config-configmap.yaml
A sample configmap that encapsulates a Chrystoki.conf file that can be injected by k8s into the deployment

.simple_sidecar_deployment.yaml
A simple deployment that uses the sidecar pattern to attach the luna-client container to Pod with a Nginx primary container. 





