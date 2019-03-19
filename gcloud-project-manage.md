# Google Cloud Project Managing/Updating #

## Deploy New Version ##

Be aware that creating two pods that are trying to connect to the same persistent volume will cause an error. The second won't be able to start until the first is terminated.

### Update Config Values ###

#### Environment Variables ####
1. `kubectl apply -f env-vars.yaml`

#### Config Files ####
ConfigMap values from files can't be updated, so they must be deleted and then re-created again.

1. `kubectl delete configmap config-files`
1. `kubectl create configmap config-files --from-file=config-files/`

### Restart/Relaunch Container/Pod ###

1. Remove existing deployment
   1. `kubectl scale deployment wordpress --replicas=0`
1. Recreate deployment
   1. `kubectl apply -f wordpress.yaml`
   
### Edit to wp-config ###
Any changes to WORDPRESS_CONFIG_EXTRA need the wp-config.php file to be re-generated. Because of the persistent storage, that doesn't happen unless the file is deleted before deploying.

1. [Open shell into running container](#shell-inside-running-container)
1. You should be put into the root web directory: `/var/www/html`
1. `rm wp-config.php`
1. `exit`
1. [Restart/Relaunch container](#restart/relaunch-container/pod)

## Shell inside Running Container ##
[Official Documentation](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)

1. Get Pod name
   1. `kubectl get pods`
1. Get shell
   1. `kubectl exec -it your-pod-name -- /bin/bash`