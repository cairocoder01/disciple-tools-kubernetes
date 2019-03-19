# Google Cloud Project Managing/Updating #

## Deploy New Version ##

Be aware that creating two pods that are trying to connect to the same persistent volume will cause an error. The second won't be able to start until the first is terminated.

### Update Config Values ###
ConfigMap values can't be updated, so they must be deleted and then re-created again.

1. `kubectl delete configmap env-vars`
1. `kubectl create -f env-vars.yaml`

### Restart/Relaunch Container/Pod ###

1. Remove existing deployment
   1. `kubectl scale deployment wordpress --replicas=0`
1. Recreate deployment
   1. `kubectl apply -f wordpress.yaml`
   

## Shell inside Running Container ##
[Official Documentation](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)

1. Get Pod name
   1. `kubectl get pods`
1. Get shell
   1. `kubectl exec -it your-pod-name -- /bin/bash`