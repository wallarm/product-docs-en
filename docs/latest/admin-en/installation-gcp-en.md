[link-launch-instance]:     https://cloud.google.com/deep-learning-vm/docs/quickstart-marketplace

[img-ssh-key-generation]:       ../images/installation-gcp/common/ssh-key-generation.png


# Deploying on Google Cloud Platform (GCP)

To deploy a filter node on the Google Cloud Platform, perform the following steps:

1. Log in to your Google Cloud Platform account.
2. Launch a filter node instance.
3. Configure the filter node instance.
4. Connect to the filter node instance via SSH.
5. Connect the filter node to the Wallarm Cloud.
6. Set up the filter node for using a proxy server.
7. Set up filtering and proxying rules
8. Allocate more memory for the Wallarm Node.
9. Configure logging.
10. Restart NGINX.

## 1. Log In to Your Google Cloud Platform Account

Log in to [console.cloud.google.com](https://console.cloud.google.com/).

## 2. Launch a Filter Node Instance

!!! info "If Wallarm WAF instance is already launched"
    If you launch Wallarm WAF instead of already existing Wallarm WAF or need to duplicate the instance in the same environment, please keep the same WAF version as currently used or update the version of all installations to the latest.

    To check the launched version, run the following command:

    ```
    apt list wallarm-node
    ```

    * If the version `2.16.x` is installed, then follow the current instruction.
    * If the version `2.14.x` is installed, then follow the [instructions for 2.14](../../2.14/admin-en/installation-gcp-en/) or update all Wallarm WAF instances to 2.16.
    * If the version`2.12.x` or lower is installed, then please update all Wallarm WAF instances to 2.16.

    More information about WAF node versioning is available in the [WAF node versioning policy](../updating-migrating/versioning-policy.md).

### Launch the instance via the Google Cloud UI

To launch the WAF node instance via the Google Cloud UI, please open the [WAF node image on the Google Cloud Marketplace](https://console.cloud.google.com/launcher/details/wallarm-node-195710/wallarm-node) and click **LAUNCH**.

The instance will launch with a preinstalled WAF node. To see detailed information on launching instances in the Google Cloud, please proceed to the [official Google Cloud Platform documentation][link-launch-instance].

### Launch the instance via Terraform or other tools

When using a tool like Terraform to launch the WAF node instance using Wallarm GCP image, you may need to provide the name of this image in the Terraform configuration.

* Image name has the following format:

    ```bash
    wallarm-node-195710/wallarm-node-<IMAGE_VERSION>-build
    ```
* To launch the instance with the WAF node version 2.16, please use the following image name:

    ```bash
    wallarm-node-195710/wallarm-node-2-16-0-7-19878-build
    ```

To get the image name, you can also follow these steps:

1. Install [Google Cloud SDK](https://cloud.google.com/sdk/docs/install).
2. Execute the command [`gcloud compute images list`](https://cloud.google.com/sdk/gcloud/reference/compute/images/list) with the following parameters:

    ```bash
    gcloud compute images list --project wallarm-node-195710 --filter="name~'wallarm-node-2-16-*'" --no-standard-images
    ```
3. Copy the version value from the name of the latest available image and paste the copied value into the provided image name format. For example, the WAF node version 2.16 image will have the following name:

    ```bash
    wallarm-node-195710/wallarm-node-2-16-0-7-19878-build
    ```

## 3. Configure the Filter Node Instance

Perform the following actions to configure the launched filter node instance:
1.  Navigate to the *VM instances* page in the *Compute Engine* section of the menu.
2.  Select the launched filter node instance and click the *Edit* button.
3.  Allow the required types of incoming traffic by ticking the corresponding checkboxes in the *Firewalls* setting.
4.  If necessary, you can restrict connecting to the instance with the project SSH keys and use a custom SSH key pair for connecting to this instance. To do this, perform the following actions:
    1.  Tick the “Block project-wide” checkbox in the *SSH Keys* setting.
    2.  Click the *Show and edit* button in the *SSH Keys* setting to expand the field for entering an SSH key.
    3.  Generate a pair of public and private SSH keys. For example, you can use the `ssh-keygen` and `PuTTYgen` utilities.
       
        ![!Generating SSH keys using PuTTYgen][img-ssh-key-generation]

    4.  Copy an open key in OpenSSH format from the interface of the used key generator (in the current example, the generated public key should be copied from the *Public key for pasting into OpenSSH authorized_keys file* area of the PuTTYgen interface) and paste it into the field containing the “Enter entire key data” hint.
    5.  Save the private key. It will be required for connecting to the configured instance in the future.
5.  Click the *Save* button at the bottom of the page to apply the changes. 

## 4. Connect to the Filter Node Instance via SSH

To see detailed information about ways of connecting to instances, proceed to this [link](https://cloud.google.com/compute/docs/instances/connecting-to-instance).

--8<-- "../include/gcp-autoscaling-connect-ssh.md"

## 5. Connect the Filter Node to the Wallarm Cloud

--8<-- "../include/connect-cloud-node-cloud-en-2.16.md"

## 6. Set up the Filter Node for Using a Proxy Server

--8<-- "../include/setup-proxy.md"

## 7. Set Up Filtering and Proxying Rules

--8<-- "../include/setup-filter-nginx-en.md"

## 8. Allocate More Memory for the Wallarm Node

The Wallarm Node uses Tarantool, an open‑source in-memory database, to calculate traffic metrics required for automated adjusting of security rules.

By default, the amount of RAM allocated to Tarantool is 75% of the total instance memory.

You can change the amount of RAM allocated for Tarantool. To allocate the instance RAM to Tarantool:

1. Open the Tarantool configuration file:

    ```
    sudo vim /etc/default/wallarm-tarantool
    ```

2. Set the amount of allocated RAM in the `SLAB_ALLOC_ARENA` in GB. The value can be an integer or a float (a dot `.` is a decimal separator). For example, to set 24 GB:
    ```
    SLAB_ALLOC_ARENA=24
    ```

3. To apply changes, restart the Tarantool daemon:
    
    ```
    sudo systemctl restart wallarm-tarantool
    ```

## 9. Configure Logging

--8<-- "../include/installation-step-logging.md"

## 10. Restart NGINX

Restart NGINX by running the following command:

``` bash
sudo systemctl restart nginx
```

## The Installation Is Complete

The installation is now complete.

--8<-- "../include/check-setup-installation-en.md"

--8<-- "../include/filter-node-defaults.md"

--8<-- "../include/installation-extra-steps.md"