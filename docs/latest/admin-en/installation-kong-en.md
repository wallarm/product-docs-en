[kong-install]:         https://konghq.com/get-started/#install
[kong-docs]:            https://getkong.org/docs/
[kong-admin-api]:       https://getkong.org/docs/0.10.x/admin-api/

[doc-wallarmblockpage]: configure-parameters-en.md#wallarm_block_page
[doc-postanalytics]:    installation-postanalytics-en.md
[doc-supported-os]:     supported-platforms.md

# Installing with Kong

!!! info "Prerequisites"
    Requirements for the Kong platform:

    * Kong version 1.4.3 or lower
    * Kong installed on a platform [supported by Wallarm][doc-supported-os] according to Kong's [official instructions][kong-install]
    
    One of the following points is required for proper Kong operation:
    
    * prepared configuration files,
    * configured database.
    
    Please make sure that the installed Kong meets the prerequisites before proceeding with Wallarm installation.
    
    The official Kong documentation is available at this [link][kong-docs].

!!! warning "Known Limitations"
    * The [`wallarm_block_page`][doc-wallarmblockpage] directive is not supported.
    * Wallarm configuration via [Kong Admin API][kong-admin-api] is not supported.

## Installation

!!! warning "Installation of postanalytics on a separate server"
    If you are planning to install postanalytics on a separate server, you must install postanalytics first. 
    
    See details in [Separate postanalytics module installation][doc-postanalytics].

To install the Wallarm module with Kong, you need to:

1. Add Wallarm repositories.
2. Install Wallarm packages.
3. Configure postanalytics.
4. Set up the filter node for using a proxy server.
5. Connect the filter node to the Wallarm cloud.
6. Configure the postanalytics server addresses.
7. Configure the filtration mode.
8. Configure logging.

--8<-- "../include/elevated-priveleges.md"

!!! info "If Wallarm WAF is already installed in your environment"
    If you install Wallarm WAF instead of an already existing Wallarm WAF or need to duplicate the installation in the same environment, then please keep the same WAF version as currently used or update all installations to the latest version. For the postanalytics installed separately, versions of substite or duplicate installations must be the same as already installed postanalytics too.

    To check the installed version of WAF node and postanalytics installed on the same server:

    === "Debian"
        ```bash
        apt list wallarm-node
        ```
    === "Ubuntu"
        ```bash
        apt list wallarm-node
        ```
    === "CentOS"
        ```bash
        yum list wallarm-node
        ```

    To check the versions of WAF node and postanalytics installed on different servers:

    === "Debian"
        ```bash
        # run from the server with installed WAF node
        apt list wallarm-node-nginx
        # run from the server with installed postanalytics
        apt list wallarm-node-tarantool
        ```
    === "Ubuntu"
        ```bash
        # run from the server with installed WAF node
        apt list wallarm-node-nginx
        # run from the server with installed postanalytics
        apt list wallarm-node-tarantool
        ```
    === "CentOS"
        ```bash
        # run from the server with installed WAF node
        yum list wallarm-node-nginx
        # run from the server with installed postanalytics
        yum list wallarm-node-tarantool
        ```

    * If the version `2.16.x` is installed, then follow the current instruction for the WAF node and for [separate postanalytics](installation-postanalytics-en.md).
    * If the version `2.14.x` is installed, then follow the instruction for [WAF node 2.14](../../2.14/admin-en/installation-kong-en/) and for [separate postanalytics 2.14](../../2.14/admin-en/installation-postanalytics-en/) or update [WAF node packages](../updating-migrating/nginx-modules.md) and [separate postanalytics packages](../updating-migrating/separate-postanalytics.md) to 2.16 in all installations.
    * If the version `2.12.x` or lower is installed, then please update the [WAF node packages](../updating-migrating/nginx-modules.md) and [separate postanalytics packages](../updating-migrating/separate-postanalytics.md) to 2.16 in all installations.

    More information about WAF node versioning is available in the [WAF node versioning policy](../updating-migrating/versioning-policy.md).

## 1. Add Wallarm Repositories

The filter node installs and updates from the Wallarm repositories.

Depending on your operating system, run one of the following commands:

=== "Debian 9.x (stretch)"
    ```bash
    sudo apt install dirmngr
    curl -fsSL https://repo.wallarm.com/wallarm.gpg | sudo apt-key add -
    sh -c "echo 'deb http://repo.wallarm.com/debian/wallarm-node stretch/2.16/' | sudo tee /etc/apt/sources.list.d/wallarm.list"
    sudo apt update
    ```
=== "Ubuntu 16.04 LTS (xenial)"
    ```bash
    curl -fsSL https://repo.wallarm.com/wallarm.gpg | sudo apt-key add -
    sh -c "echo 'deb http://repo.wallarm.com/ubuntu/wallarm-node xenial/2.16/' | sudo tee /etc/apt/sources.list.d/wallarm.list"
    sudo apt update
    ```
=== "Ubuntu 18.04 LTS (bionic)"
    ```bash
    curl -fsSL https://repo.wallarm.com/wallarm.gpg | sudo apt-key add -
    sh -c "echo 'deb http://repo.wallarm.com/ubuntu/wallarm-node bionic/2.16/' | sudo tee /etc/apt/sources.list.d/wallarm.list"
    sudo apt update
    ```
=== "CentOS 7.x"
    ```bash
    sudo yum install -y epel-release
    sudo rpm -i https://repo.wallarm.com/centos/wallarm-node/7/2.16/x86_64/Packages/wallarm-node-repo-1-5.el7.noarch.rpm
    ```

--8<-- "../include/access-repo-en.md"

## 2. Install Wallarm Packages

To install the filter node and postanalytics on the same server, run the command:

=== "Debian 9.x (stretch)"
    ```bash
    sudo apt install --no-install-recommends wallarm-node kong-module-wallarm
    ```
=== "Ubuntu 16.04 LTS (xenial)"
    ```bash
    sudo apt install --no-install-recommends wallarm-node kong-module-wallarm
    ```
=== "Ubuntu 18.04 LTS (bionic)"
    ```bash
    sudo apt install --no-install-recommends wallarm-node kong-module-wallarm
    ```
=== "CentOS 7.x"
    ```bash
    sudo yum install wallarm-node kong-module-wallarm
    ```

To install the filter node alone, run the command:

=== "Debian 9.x (stretch)"
    ```bash
    sudo apt install --no-install-recommends wallarm-node-nginx kong-module-wallarm
    ```
=== "Ubuntu 16.04 LTS (xenial)"
    ```bash
    sudo apt install --no-install-recommends wallarm-node-nginx kong-module-wallarm
    ```
=== "Ubuntu 18.04 LTS (bionic)"
    ```bash
    sudo apt install --no-install-recommends wallarm-node-nginx kong-module-wallarm
    ```
=== "CentOS 7.x"
    ```bash
    sudo yum install wallarm-node-nginx kong-module-wallarm
    ```

## 3. Configure Postanalytics 

!!! info
    Skip this step if you installed postanalytics on a separate server as you already have your postanalytics configured.

The amount of memory determines the quality of work of the statistical algorithms.

The recommended value is 75% of the total server memory. For example, if the server has 32 GB of memory, the recommended allocation size is 24 GB.

**Allocate the operating memory size for Tarantool:**

Open for editing the configuration file of Tarantool:

=== "Debian 9.x (stretch)"
    ``` bash
    sudo vim /etc/default/wallarm-tarantool
    ```
=== "Ubuntu 16.04 LTS (xenial)"
    ``` bash
    sudo vim /etc/default/wallarm-tarantool
    ```
=== "Ubuntu 18.04 LTS (bionic)"
    ``` bash
    sudo vim /etc/default/wallarm-tarantool
    ```
=== "CentOS 7.x"
    ``` bash
    sudo vim /etc/sysconfig/wallarm-tarantool
    ```

Set the allocated memory size in the configuration file of Tarantool via the `SLAB_ALLOC_ARENA` directive. The value can be an integer or a float (a dot `.` is a decimal separator).

For example:

```
SLAB_ALLOC_ARENA=24
```

**Restart Tarantool:**

=== "Debian 9.x (stretch)"
    ``` bash
    sudo systemctl restart wallarm-tarantool
    ```
=== "Ubuntu 16.04 LTS (xenial)"
    ``` bash
    sudo service wallarm-tarantool restart
    ```
=== "Ubuntu 18.04 LTS (bionic)"
    ``` bash
    sudo service wallarm-tarantool restart
    ```
=== "CentOS 7.x"
    ``` bash
    sudo systemctl restart wallarm-tarantool
    ```

## 4. Set up the Filter Node for Using a Proxy Server

--8<-- "../include/setup-proxy.md"

## 5. Connect the Filter Node to the Wallarm Cloud

--8<-- "../include/connect-cloud-en.md"

## 6. Configure the Postanalytics Server Addresses

!!! info
    * Skip this step if you installed postanalytics and the filter node on the same server.
    * Do this step if you installed postanalytics and the filter node on separate servers.

--8<-- "../include/configure-postanalytics-address-kong-en.md"

## 7. Set up the Filtration Mode

--8<-- "../include/setup-filter-kong-en.md"

## 8. Configure Logging

--8<-- "../include/installation-step-logging.md"

## Start Kong

To start Kong with the installed Wallarm module, run the command:

```
kong start --nginx-conf /etc/kong/nginx-wallarm.template
```

## The Installation Is Complete

--8<-- "../include/check-setup-installation-en.md"

--8<-- "../include/filter-node-defaults.md"

--8<-- "../include/installation-extra-steps.md"