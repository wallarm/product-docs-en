[wallarm-status-instr]:             ../admin-en/configure-statistics-service.md
[sqli-attack-desc]:                 ../attacks-vulns-list.md#sql-injection
[xss-attack-desc]:                  ../attacks-vulns-list.md#crosssite-scripting-xss
[img-test-attacks-in-ui]:           ../images/admin-guides/test-attacks.png
[waf-mode-instr]:                   ../admin-en/configure-wallarm-mode.md
[logging-instr]:                    ../admin-en/configure-logging.md
[proxy-balancer-instr]:             ../admin-en/using-proxy-or-balancer-en.md
[scanner-whitelisting-instr]:       ../admin-en/scanner-ips-whitelisting.md
[process-time-limit-instr]:         ../admin-en/configure-parameters-en.md#wallarm_process_time_limit
[configure-selinux-instr]:          ../admin-en/configure-selinux.md
[configure-proxy-balancer-instr]:   ../admin-en/configuration-guides/access-to-wallarm-api-via-proxy.md
[install-postanalytics-instr]:      ../admin-en/installation-postanalytics-en.md
[dynamic-dns-resolution-nginx]:     ../admin-en/configure-dynamic-dns-resolution-nginx.md
[enable-libdetection-docs]:         ../admin-en/configure-parameters-en.md#wallarm_enable_libdetection

# Updating Linux WAF packages

These instructions describe the steps to update Linux WAF packages to version 2.16. Linux WAF packages are packages installed in accordance with one of the following instructions:

* [NGINX `stable` module](../waf-installation/nginx/dynamic-module.md)
* [Module for NGINX from CentOS/Debian repositories](../waf-installation/nginx/dynamic-module-from-distr.md)
* [NGINX Plus module](../waf-installation/nginx-plus.md)
* [Kong module](../admin-en/installation-kong-en.md)

## Update procedure

* If WAF node and postanalytics modules are installed on the same server, then follow the instrutions below to update all packages.
* If WAF node and postanalytics modules are installed on different servers, then first update the postanalytics module following these [instructions](separate-postanalytics.md) and perform the steps below for WAF node modules.

## Step 1: Add new Wallarm WAF repository

Delete the previous Wallarm WAF repository address and add a repository with a new WAF node version package. Please use the commands for the appropriate platform.

**CentOS and Amazon Linux 2**

```bash
sudo yum remove wallarm-node-repo
sudo rpm -i https://repo.wallarm.com/centos/wallarm-node/7/2.16/x86_64/Packages/wallarm-node-repo-1-5.el7.noarch.rpm
```

**Debian and Ubuntu**

1. Open the file with the Wallarm WAF repository address in the installed text editor. In these instructions, **vim** is used.

    ```bash
    sudo vim /etc/apt/sources.list.d/wallarm.list
    ```
2. Comment out or delete the previous repository address.
3. Add a new repository address:

    === "Debian 9.x (stretch)"
        ``` bash
        deb http://repo.wallarm.com/debian/wallarm-node stretch/2.16/
        ```
    === "Debian 9.x (stretch-backports)"
        ```bash
        deb http://repo.wallarm.com/debian/wallarm-node stretch/2.16/
        deb http://repo.wallarm.com/debian/wallarm-node stretch-backports/2.16/
        ```
    === "Debian 10.x (buster)"
        ```bash
        deb http://repo.wallarm.com/debian/wallarm-node buster/2.16/
        ```
    === "Ubuntu 16.04 LTS (xenial)"
        ```bash
        deb http://repo.wallarm.com/ubuntu/wallarm-node xenial/2.16/
        ```
    === "Ubuntu 18.04 LTS (bionic)"
        ```bash
        deb http://repo.wallarm.com/ubuntu/wallarm-node bionic/2.16/
        ```

## Step 2: Update Wallarm WAF packages

### WAF node and postanalytics on the same server

=== "Debian"
    ```bash
    sudo apt update
    sudo apt dist-upgrade -o Dir::Etc::sourcelist=/etc/apt/sources.list.d/wallarm.list -o Dir::Etc::sourceparts=""
    ```
=== "Ubuntu"
    ```bash
    sudo apt update
    sudo apt dist-upgrade -o Dir::Etc::sourcelist=/etc/apt/sources.list.d/wallarm.list -o Dir::Etc::sourceparts=""
    ```
=== "CentOS или Amazon Linux 2"
    ```bash
    sudo yum update
    ```

### WAF node and postanalytics on different servers

!!! warning "Sequence of steps to update the WAF node and postanalytics modules"
    If the WAF node and postanalytics modules are installed on different servers, then it is required to update the postanalytics packages before updating the WAF node packages.

1. Update postanalytics packages following these [instructions](separate-postanalytics.md).
2. Update WAF node packages:

    === "Debian"
        ```bash
        sudo apt update
        sudo apt dist-upgrade -o Dir::Etc::sourcelist=/etc/apt/sources.list.d/wallarm.list -o Dir::Etc::sourceparts=""
        ```
    === "Ubuntu"
        ```bash
        sudo apt update
        sudo apt dist-upgrade -o Dir::Etc::sourcelist=/etc/apt/sources.list.d/wallarm.list -o Dir::Etc::sourceparts=""
        ```
    === "CentOS или Amazon Linux 2"
        ```bash
        sudo yum update
        ```

## Step 3: Restart NGINX

--8<-- "../include/waf/restart-nginx-2.16.md"

## Step 4: Test Wallarm WAF operation

--8<-- "../include/waf/installation/test-waf-operation.md"

## Settings customization

Wallarm WAF modules are updated to version 2.16. Previous WAF node settings will be applied to the new version automatically. To make additional settings, use the [available directives](../admin-en/configure-parameters-en.md).

--8<-- "../include/waf/installation/common-customization-options-nginx-216.md"
