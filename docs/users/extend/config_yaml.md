<h1>.ddev/config.yaml options</h1>

Each project has a (hidden) directory named .ddev, and
the .ddev/config.yaml is the primary configuration for the project.

* You can override the config.yaml with extra files named "config.*.yaml". For example, many teams use `config.local.yaml` for configuration that is specific to one environment, and that is not intended to be checked into the team's default config.yaml.
* Most configuration options take effect on `ddev start`
* Nearly all configuration options have equivalent `ddev config` flags, so you can use ddev config instead of editing the config.yaml. See `ddev help config` to see all the flags.

| Item  | Description   | Notes  |
|---|---|---|
| name  | Project name   | Must be unique on the host (no two projects can have the same name). It's best if this is the same as the directory name.   |
| type | Project type | php/drupal6/drupal7/drupal8/backdrop/typo3wordpress. Project type "php" does not try to do any CMS configuration or settings file management, and can work with any project|
| docroot | Relative path of the docroot (where index.php or index.html is) from the project root| |
| php_version | 5.6/7.0/7.1/7.2/7.3 | It is only possible to provide the major version (like "7.3"), not a minor version like "7.3.2", and it is only possible to use the provided php versions. |
| webimage | docker image to use for webserver | It is unusual to change the default and is not recommended, but the webimage can be overridden with a correctly crafted image, probably derived from drud/ddev-webserver |
| dbimage | docker image to use for db server | It is unusual to change the default and is not recommended, but the dbimage can be overridden with a correctly crafted image, probably derived from drud/ddev-dbserver |
| dbaimage | docker image to use for dba server (phpMyAdmin server) | It is unusual to change the default and is not recommended, but the dbimage can be overridden with a correctly crafted image, probably derived from drud/phpmyadmin |
| router_http_port | Port used by the router for http |  Defaults to port 80. This can be changed if there is a conflict on the host over port 80 |
| router_https_port | Port used by the router for https |Defaults to 443, usually only changed if there is a conflicting process using port 443 |
| xdebug_enabled | "true" enables xdebug | Many people prefer to use `ddev exec enable_xdebug` and `ddev exec disable_xdebug` instead of configuring this, because xdebug has a significant performance impact. |
| webserver_type | nginx-fpm or apache-fpm or apache-cgi | The default is nginx-fpm, and it works best for many projects |
| timezone | timezone to use in container and in PHP configuration | It can be set to any valid timezone, see [timezone list](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones). For example "Europe/Dublin" or "MST7MDT". The default is UTC. |
| additional_hostnames | array of extra hostnames | `additional_hostnames: ["somename", "someothername"]` would provide http and https URLs for "somename.ddev.site" and "someothername.ddev.site". |
| additional_fqdns | extra fully-qualified domain names | `additional_fqdns: ["example.com", "sub1.example.com"]` would provide http and https URLs for "example.com" and "sub1.example.com". Please take care with this because it can cause great confusion and adds extraneous items to your /etc/hosts file. |
| upload_dir | Relative path to upload directory used by `ddev import-files` | |
| working_dir | explicitly specify the working directory used by `ddev exec` and `ddev ssh` | `working_dir: { web:  "/var/www", db: "/etc" }` would set the working directories for the web and db containers. |
| omit_containers | Allows the project to not load specified containers | For example, `omit_containers: ["dba", "ddev-ssh-agent"]`. Currently only these containers are supported. These containers can also be omitted globally in the ~/.ddev/global_config.yaml. |
| nfs_mount_enabled | Allows using NFS to mount the project into the container for performance reasons | See [nfsmount_enabled documentation](../../performance.md). This requires configuration on the host before it can be used. |
| host_https_port | Specify a specific and persistent https port for direct binding to the localhost interface | This is not commonly used, but a specific port can be provided here and the https URL will always remain the same. For example, if you put "59001", the project will always use "https://127.0.0.1:59001". for the localhost URL. (Note that the named URL is more commonly used and for most purposes is better.) If this is not set the port will change from `ddev start` to `ddev start` |
| host_webserver_port | Specify a specific and persistent http port for direct binding to the localhost interface | This is not commonly used, but a specific port can be provided here and the https URL will always remain the same. For example, if you put "59000", the project will always use "http://127.0.0.1:59000". for the localhost URL. (Note that the named URL is more commonly used and for most purposes is better.) If this is not set the port will change from `ddev start` to `ddev start` |
| host_db_port | localhost binding port for database server | If specified here, the database port will remain consistent. This is useful for configuration of host-side database browsers. Note, though, that `ddev sequelpro` and `ddev mysql` do all this automatically, as does the sample command `ddev mysqlworkbench`. |
| phpmyadmin_port | port used for PHPMyAdmin URL | This is sometimes changed from the default 8036 when a port conflict is discovered |
| mailhog_port | port used in Mailhog URL | this can be changed from the default 8025 in case of port conflicts |
| webimage_extra_packages| Add extra Debian packages to the ddev-webserver. | For example, `webimage_extra_packages: [php-yaml, php7.3-ldap]` will add those two packages |
| dbimage_extra_packages| Add extra Debian packages to the ddev-dbserver. | For example, `dbimage_extra_packages: ["less"]` will add those that package. |
| use_dns_when_possible | defaults to true (using DNS instead of editing /etc/hosts) | If set to false, ddev will always update the /etc/hosts file with the project hostname instead of using DNS for name resolution |
| project_tld | defaults to "ddev.site" so project urls become "someproject.ddev.site" | This can be changed to anything that works for you; to keep things the way they were before ddev v1.9, use "ddev.local" |
| ngrok_args | Extra flags for ngrok when using the `ddev share` command | For example, `--subdomain mysite --auth user:pass`. See [ngrok docs on http flags](https://ngrok.com/docs#http) |
| provider| hosting provider for `ddev pull` | "pantheon" or "drud-aws" or "default" |
| hooks | | See [Extending Commands](../../extending-commands.md) for more information. |
