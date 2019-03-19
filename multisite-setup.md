# Multi-Site Setup #

To setup a multi-site instance of WordPress, be sure to use `env-vars-multisite.default.yaml` as the base for your configuration. It includes the needed WordPress settings to get started.

1. After initial site setup, go to ***Tools->Network Setup*** (/wp-admin/network.php)
1. Complete the given fields and click ***Install***
1. On the subsequent page, you'll be shown some values to update in `wp-config.php` and `.htaccess`
   1. ***.htaccess***
      1. These changes should already be in the included file `config-files/.htaccess`, so it can just be copied to your container.
      1. Get pod name: `kubectl get pod -l app=wordpress`
      1. Copy file: `kubectl cp config-files/.htaccess your-pod-name:/var/www/html/.htaccess`
   1. ***wp-config.php***
      1. Most of these values are already in the `wp-config.php` but commented it, so you'll need to login to a shell to edit the file.
      1. Get pod name: `kubectl get pod -l app=wordpress`
      1. Open shell: `kubectl exec -it your-pod-name -- /bin/bash`
      1. `apt-get update`
      1. `apt-get install nano`
      1. `nano wp-config.php`
      1. Uncomment and/or edit the needed lines, then save and exit (CTRL + X, yes, ENTER)
1. You will have to login again, but you will now see the ***My Sites*** option in the top left to manage your sites.