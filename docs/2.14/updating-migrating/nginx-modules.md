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

# Updating NGINX WAF modules to 2.14

This instruction describes the steps to update installed WAF modules to version 2.14.

## Update procedure

* If WAF node and postanalytics modules are installed on the same server, follow the instrutions below to update all packages.
* If WAF node and postanalytics modules are installed on different servers, first update the postanalytics module following the [instruction](separate-postanalytics.md) and perform the steps below for WAF node modules.

## Step 1: Add new Wallarm WAF repositories

--8<-- "../include/migration-212-214/add-new-repo.md"

## Step 2: Update Wallarm WAF packages

### WAF node and postanalytics on the same server

=== "Debian"
    ```bash
    sudo apt install wallarm-node --no-install-recommends
    ```
=== "Ubuntu"
    ```bash
    sudo apt install wallarm-node --no-install-recommends
    ```
=== "CentOS or Amazon Linux 2"
    ```bash
    sudo yum update wallarm-node
    ```

### WAF node and postanalytics on different servers

1. Update postanalytics packages following the [instruction](separate-postanalytics.md).
2. Update WAF node packages:

    === "Debian"
        ```bash
        sudo apt install wallarm-node-nginx --no-install-recommends
        ```
    === "Ubuntu"
        ```bash
        sudo apt install wallarm-node-nginx --no-install-recommends
        ```
    === "CentOS или Amazon Linux 2"
        ```bash
        sudo yum update wallarm-node-nginx
        ```

## Step 3: Restart NGINX

--8<-- "../include/waf/restart-nginx.md"

## Step 4: Test Wallarm WAF operation

--8<-- "../include/waf/installation/test-waf-operation.md"

## Settings customization

Wallarm WAF modules are updated to version 2.14. Previous settings saved on the NGINX level will be applied to a new version automatically. To make additional settings, use the [available directives](../admin-en/configure-parameters-en.md).

--8<-- "../include/waf/installation/common-customization-options.md"