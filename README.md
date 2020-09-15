
# kickstart-flavor gaia :: Apache2, PHP7.2 development container
[![Build Status](https://img.shields.io/endpoint.svg?url=https%3A%2F%2Factions-badge.atrox.dev%2Fnfra-project%2Fkickstart-flavor-php%2Fbadge&style=flat)](https://github.com/nfra-project/kickstart-flavor-php/actions)
[![Latest Stable Version](https://img.shields.io/github/release/nfra-project/kickstart-flavor-php.svg)](https://github.com/nfra-project/kickstart-flavor-php/releases)
[![Docker Pulls](https://img.shields.io/docker/pulls/nfra/kickstart-flavor-php.svg)](https://github.com/nfra-project/kickstart-flavor-php)

see (http://github.com/infracamp/kickstart) for more information.

Document Index:

- [Development Guide for this flavor](DEVELOPMENT.md)
- [Dockerhub page](https://hub.docker.com/r/nfra/kickstart-flavor-php/)
    - [Tags available](https://hub.docker.com/r/nfra/kickstart-flavor-php/tags/)
    - [Build details](https://hub.docker.com/r/nfra/kickstart-flavor-php/builds/)


## Tags

| Tag         | Version                 |
|-------------|-------------------------|
| latest      | 7.2                     |
| testing     | 7.2                     |
| experimental| 7.2                     |
| 7.4-experimental | 7.4 **new**        |
| 7.4-testing      | coming soon        |
| 7.4-stable       | coming soon        |


## Container specific `.kick.yml`-directives

| Key | Default | Description |
|----------------|----------------|---------------------|
| `http_port`                  | `80`          | The internal port, apache listens on |
| `apache_www_root`            | `/opt/www`    | The document root |
| `SYSLOG_HOST`                | ''            | Send apache2 error log to this host             |


## Additional configuration

In `.kick.yml` you can modify the vhost settings

```yaml
apache_vhost_config: |
    FallbackResource /index.php
    php_admin_value file_uploads 0
```


## Installed Software

Make sure your IDE has plugins installed for:

- **[editorconfig](https://editorconfig.org/#download)** 



### Using the xdebug debugger / profiler with PhpStorm

Xdebug is enabled by default if you are in development mode (the project is run by kickstart.sh)

Xdebug is configured to connect the IDE on the `Docker-Host-IP` (which is the first configured IP of your
host machine - determined by kickstart.sh) on Port `9000`

#### PhpStorm IDE settings (global) 

Activate remote debugging in **Settings**:

![Settings](doc/xdebug-config1.png)

#### Project settings

Open *Run* > *Edit configurations...*

![Project-Config](doc/xdebug-project-config1.png)

**The idekey and serverName is the name of your project!**

Enable Path Mappings:

![Remote-debug-server-config](doc/xdebug-server-config1.png)


> **Ensure `serverName`, `idekey` and `configName` is the same of your project name**


#### Start Debugging via browser

To start debugging via browser open:
```
http://localhost/?XDEBUG_SESSION_START=project123-name
```

#### Start CLI debugging

```
just set the breakpoint
```


#### Troubleshooting the debugging

If debugging won't work, use our checklist to determine the cause:

1) Check the docker-host's IP was propagated correctly.
    - The IP should be set as environment-variable `DOCKER_HOST_IP`
    - Inside the container type `echo $DOCKER_HOST_IP` should print your ip
    
    > A) Check kickstart.sh is the newest version (perform ./kickstart.sh --upgrade)
    
    > B) Solution: Check `hostname -i` returns your primary IP address on your
    > host machine.
    
2) Check the IDE is connectable from within the container on Port `9000` 
    - Install netcat and try the script below:
    ```
    sudo apt install netcat
    nc -zw3 $DOCKER_HOST_IP 9000 && echo "OK: IDE is reachable on port 9000" || echo "ERROR: IDE unreachable"
    ```
    > A) Check if you can reach this PORT from outside the container. If not your IP might be wrong,
    > or your IDE is not correctly configured.    
    
3) Check your IDE-Settings.