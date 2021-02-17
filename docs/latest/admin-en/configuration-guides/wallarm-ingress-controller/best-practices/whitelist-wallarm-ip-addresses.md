# Configuration of IP Whitelisting for Wallarm Scanner

--8<-- "../include/ingress-controller-best-practices-intro.md"

To ensure proper functionality of the [Wallarm vulnerability scanner](../../../../user-guides/scanner/intro.md), it is necessary to whitelist (disable the WAF functionality for) the IP addresses of Wallarm’s scanner nodes. This allows the scanner to reach the protected application in order to validate that detected attacks are not targeting real application vulnerabilities.

The list of Wallarm IP addresses is available at the following links:

* [Scanners for the European Cloud](../../../scanner-address-en.md)
* [Scanners for the US Cloud](../../../scanner-address-us-en.md)

To whitelist the scanner nodes, please use the following instructions:

1. Add the `controller.config.http-snippet` attribute to the `values.yaml` file and adjust the filter node mode, Wallarm IP addresses and the filter node mode for Wallarm IP addresses:
    ```
    controller:
      config:
        use-forwarded-headers: "true"
        http-snippet: |
          geo $remote_addr $wallarm_mode_real {
            default block; # default filter node mode: off to disable request processing, monitoring to process but not block requests, block to process all requests and block the malicious ones
            # IP addresses and rules for US Cloud scanners
            50.116.11.251 off;45.79.143.18 off;172.104.21.210 off;74.207.237.202 off;45.79.186.159 off;45.79.216.187 off;45.33.16.32 off;96.126.127.23 off;172.104.208.113 off;192.81.135.28 off;104.237.155.105 off;45.56.71.221 off;45.79.194.128 off;104.237.151.202 off;45.33.15.249 off;45.33.43.225 off;45.79.10.15 off;45.33.79.18 off;45.79.75.59 off;23.239.30.236 off;172.104.22.150 off;45.33.86.254 off;45.56.72.191 off;45.79.75.91 off;192.155.92.134 off;23.239.4.41 off;45.79.93.164 off;45.56.122.184 off;96.126.124.141 off;45.79.115.178 off;66.228.36.28 off;192.81.134.116 off;45.79.138.122 off;45.56.68.241 off;69.164.216.244 off;104.237.139.12 off;23.239.18.250 off;45.56.123.144 off;50.116.35.43 off;50.116.43.110 off;72.14.181.105 off;45.56.102.9 off;173.255.192.83 off;173.255.200.80 off;173.230.156.200 off;173.255.214.180 off;45.33.81.109 off;173.230.158.207 off;50.116.42.181 off;72.14.184.100 off;66.175.222.237 off;69.164.202.55 off;45.56.113.41 off;192.155.82.205 off;23.92.30.204 off;45.33.65.37 off;45.56.114.24 off;173.255.193.92 off;45.33.64.71 off;104.200.29.36 off;45.56.119.39 off;45.33.105.35 off;45.33.73.43 off;45.33.72.81 off;45.33.80.65 off;104.237.151.23 off;45.33.88.42 off;72.14.191.76 off;45.33.98.89 off;173.230.130.253 off;45.33.97.86 off;23.92.18.13 off;45.33.33.19 off;23.239.11.21 off;45.33.41.31 off;173.230.138.206 off;66.228.58.101 off;45.56.104.7 off;50.116.23.110 off;45.33.115.7 off;45.79.16.240 off;45.56.69.211 off;34.94.100.64 off;35.235.101.133 off;34.94.16.235 off;35.236.51.79 off;35.236.55.214 off;35.236.127.211 off;35.236.126.84 off;35.236.3.158 off;34.94.218.5 off;35.236.118.146 off;35.236.1.4 off;35.236.20.89 off;
            # IP addresses and rules for European Cloud scanners
            139.162.148.184 off;139.162.151.155 off;139.162.159.244 off;139.162.151.10 off;139.162.159.137 off;139.162.157.131 off;139.162.144.202 off;139.162.130.66 off;139.162.158.79 off;139.162.156.102 off;85.90.246.120 off;172.104.128.215 off;139.162.174.26 off;139.162.182.20 off;139.162.190.22 off;139.162.168.17 off;139.162.167.19 off;139.162.170.84 off;139.162.191.89 off;139.162.176.169 off;139.162.184.225 off;139.162.185.243 off;172.104.143.34 off;139.162.178.148 off;139.162.186.136 off;139.162.163.61 off;139.162.171.141 off;139.162.179.214 off;139.162.187.138 off;139.162.164.41 off;139.162.172.35 off;139.162.180.37 off;139.162.188.246 off;172.104.139.18 off;172.104.152.28 off;139.162.177.83 off;172.104.240.115 off;172.105.64.135 off;139.162.153.16 off;172.104.241.162 off;139.162.167.48 off;172.104.233.100 off;172.104.157.26 off;172.105.65.182 off;172.104.138.5 off;172.104.150.243 off;139.162.190.165 off;139.162.166.202 off;139.162.174.220 off;139.162.182.156 off;139.162.190.86 off;139.162.167.51 off;139.162.175.71 off;172.104.152.54 off;172.104.151.59 off;172.104.128.67 off;172.104.152.96 off;139.162.145.238 off;172.104.128.103 off;172.104.139.37 off;139.162.184.33 off;139.162.186.129 off;172.104.154.128 off;172.104.146.90 off;172.104.128.44 off;85.90.246.49 off;139.162.146.245 off;139.162.171.208 off;172.104.252.112 off;139.162.132.87 off;139.162.162.71 off;172.104.229.59 off;172.104.152.244 off;172.104.250.27 off;139.162.130.123 off; 178.32.42.221 off;46.105.75.84 off;51.254.85.145 off;188.165.30.182 off;188.165.136.41 off;188.165.137.10 off;54.36.135.252 off;54.36.135.253 off;54.36.135.254 off;54.36.135.255 off;54.36.131.128 off;54.36.131.129 off;
          }
    ```
2. Go to the configuration of the associated Ingress resource and set the value of the `nginx.ingress.kubernetes.io/wallarm-mode` annotation to the value `"$wallarm_mode_real"`:
    ```
    apiVersion: extensions/v1beta1
        kind: Ingress
        metadata:
            annotations:
                nginx.ingress.kubernetes.io/wallarm-mode: "$wallarm_mode_real"
    ```
3. Apply the updated configurations of the Ingress controller and Ingress resource.

!!! warning
    Please note that after the change the default filter node mode for the protected resource is controlled by the configuration of the Ingress controller (in the file `values.yaml`), not the configuration of the Ingress resource.
