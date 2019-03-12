# Google Cloud Project Deletion #

Process/commands to delete the resources created in [Google Cloud Project Creation](gcloud-project-setup.md)

1. Delete exposed service (load balancer) and deployment
   1. `kubectl delete service,deployment -l app=wordpress`
   1. Wait for deletion: `gcloud compute forwarding-rules list`
1. Delete Persistent Volumes
   1. `kubectl delete pvc wordpress-volumeclaim`
   1. Wait for deletion: `kubectl get pvc`
1. Delete container cluster
   1. `gcloud container clusters delete disciple-tools`
   1. Might take a few minutes...   
