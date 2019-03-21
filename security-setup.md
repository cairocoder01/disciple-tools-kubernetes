# Security Setup #
Additional steps to secure your WordPress site.

## Prevent PHP Execution from Uploads ##
Prevent any files that are in the `/wp-content/uploads` directory from being executed as PHP files.

`kubectl cp config-files/no-php-exec.htaccess your-pod-name:/var/www/html/wp-content/uploads/.htaccess`


## Recommended Plugins ##
See [The Best WordPress Security Plugins and Services (Both Free and Premium) 2019](https://winningwp.com/best-wordpress-security-plugins-and-services/)
and [WP Security Compared](http://wpsecuritycompared.com/)

* [Block Bad Queries (BBQ)](https://wordpress.org/plugins/block-bad-queries/) - Free
  * Simple, super-fast plugin that protects your site against malicious URL requests.
* [iThemes Security](https://wordpress.org/plugins/better-wp-security/) - Freemium
  * \#1 WordPress Security Plugin
* [All in One WP Security & Firewall](https://wordpress.org/plugins/all-in-one-wp-security-and-firewall/) - Free
  * It reduces security risk by checking for vulnerabilities, and by implementing and enforcing the latest recommended WordPress security practices and techniques.