# Deploying WAF on the cloud platform from source packages

1. Create a new instance on the cloud platform:

   * Instructions for GCP
   * Instructions for AWS
   * Instructions for Yandex.Cloud
   * Instructions for Azure
   * Instructions for Alibaba
   * If deploying on the private cloud, use your own method.
2. Connect to instance via SSH.
3. Install NGINX and Wallarm WAF following the instructions:

   * For NGINX `stable` and Wallarm WAF installed from appropriate official repositories
   * For NGINX `stable` and Wallarm WAF installed from official Wallarm repository (Debian and CentOS)
   * For NGINX Plus and Wallarm WAF installed from appropriate official repositories
4. Configure proxying requests to the instance with installed WAF node (if required?).
