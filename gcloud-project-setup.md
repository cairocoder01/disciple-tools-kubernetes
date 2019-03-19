# Google Cloud Project Setup #

Setup a project within Google Cloud.

This will set up a cluster with one node/container for Wordpress using persistent storage (of Wordpress files).

The database is setup elsewhere as a Cloud SQL database so as to scale more easily than using Kubernetes with persistent storage.

*This is closely based on [Using Persistent Disks with WordPress and MySQL](https://cloud.google.com/kubernetes-engine/docs/tutorials/persistent-disk).*

1. ***Ensure Kubernetes Engine API is enabled for the given project in the [GCP console](https://console.cloud.google.com/)***
     1. `APIs & Services`
     1. `+ Enable APIs and Services`
     1. Search 'kubernetes'
     1. Select `Kubernetes Engine API`
     1. `Enable` 
1. ***Login to your account***
   * `gcloud auth login`
1. ***Set project defaults***
   * `export PROJECT_ID=my-project-name-123456`
   * `gcloud config set project my-project-name-123456`
   * `gcloud config set compute/zone europe-west2-a`
1. ***Create a cluster***
   1. `gcloud container clusters create [my-cluster-name] --num-nodes=3 --enable-ip-alias`
      * Change `num-nodes` based on how many nodes are needed
      * `gcloud container clusters create disciple-tools --enable-ip-alias`
1. ***Create PersistentVolumes and PersistentVolumeClaims***
   1. Deploy: `kubectl apply -f wordpress-volumeclaim.yaml`
   1. Check claims: `kubectl get pvc` (might take a few seconds to be provisioned)
1. ***Setup MySQL***
   1. *Instead of using Kubernetes like the tutorial above, we'll [connect to Cloud SQL from a Kubernetes container](https://cloud.google.com/sql/docs/mysql/connect-kubernetes-engine)*
   1. Open [GCP console](https://console.cloud.google.com/)
   1. Go to `SQL` (under Storage product category)
   1. Create Instance
   1. Choose MySQL
   1. Set Instance ID, root password, region
      1. **IMPORTANT:** Ensure this instance is in the same region as the cluster created above.
   1. Under `Show configuration options`...
      1. Select Private IP (enable necessary APIs if prompted)
      1. De-select Public IP
   1. Click `Create`
      1. *Setup could take a minute to finish*
      1. Once instance is configured, take note of the `Private IP Address` in the `Connect to this Instance` section of the details page.
   1. Create new database user
      1. Go to `Users` tab
      1. `Create user account`
      1. Set username to `wordpress` and create a secure password
      1. Click `Create`
   1. Create new database
      1. Go to `Databases` tab
      1. `Create database`
      1. Set database name to `wordpress`
      1. Click `Create`
   1. Create [Secret](https://cloud.google.com/kubernetes-engine/docs/concepts/secret) for username and password
      1. Syntax: `kubectl create secret [TYPE] [NAME] [DATA]`
      1. Run: `kubectl create secret generic mysql --from-literal=password=YOUR_PASSWORD --from-literal=host=PRIVATE_IP_ADDRESS` (*Replace YOUR_PASSWORD and PRIVATE_IP_ADDRESS*)
1. ***Set up PHP Config values***
   1. Copy either `env-vars.default.yaml` or `env-vars-multisite.default.yaml` and rename to `env-vars.yaml`. Update all values with your environment-specific values.
   1. Execute: `kubectl create -f env-vars.yaml`
   1. Execute: `kubectl create configmap config-files --from-file=config-files/`
1. ***Set up WordPress***
   1. Deploy: `kubectl create -f wordpress.yaml`
   1. Wait: `kubectl get pod -l app=wordpress`
1. ***Expose WordPress Service (load balancer)***
   1. Deploy: `kubectl create -f wordpress-service.yaml`
   1. Wait: `kubectl get svc -l app=wordpress`
1. ***Visit WordPress site and complete setup***
   1. Using `EXTERNAL-IP` from the command `kubectl get svc -l app=wordpress`, go to...
   1. `http://EXTERNAL-IP/wp-admin/install.php`
1. [Install Disciple Tools Theme](https://github.com/DiscipleTools/disciple-tools-theme)

*See [Project Deletion](gcloud-project-deletion.md) for details on deleting all of the resources above*