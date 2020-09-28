[link-wallarm-account-eu]:         https://my.wallarm.com
[link-wallarm-account-us]:         https://us1.my.wallarm.com

[link-doc-nginx-overview]:      installation-nginx-overview.md

[link-ig-ingress-nginx]:        installation-kubernetes-en.md
[link-ig-ingress-nginx-d2iq]:   https://docs.d2iq.com/ksphere/konvoy/partner-solutions/wallarm/
[link-ig-aws]:                  installation-ami-en.md
[link-ig-gcloud]:               installation-gcp-en.md
[link-ig-docker-nginx]:         installation-docker-en.md
[link-ig-kong]:                 installation-kong-en.md

<!-- Uncomment when Envoy docs will be required
[link-ig-docker-envoy]:         installation-guides/envoy/envoy-docker.md 
-->

#   Supported Platforms

!!! info "Installation Prerequisites"
    Make sure that you acquire the following before installing the filter node:

    *   The supported platform 
    *   The permissions to execute commands with `root` rights
    *   A Wallarm account in one of the following clouds:

        *   [The Wallarm EU cloud][link-wallarm-account-eu]
        *   [The Wallarm US cloud][link-wallarm-account-us]

The Wallarm filter node can be installed on the following platforms:
*   NGINX and NGINX Plus
    
    The integration of the filter node with NGINX or NGINX Plus is performed using several modules.
    
    There are [several options for module installation][link-doc-nginx-overview]. The option you select will depend on the way you install NGINX or NGINX Plus.
    
    Additionally, you can deploy a filter node as a Docker container. The resulting filter node installation will contain all necessary modules (see [installation instructions][link-ig-docker-nginx]).
    
    !!! info "Supported Operating Systems"
        The Wallarm modules can be installed on the following operating systems:
        
        *   Debian 9.x (stretch)
        *   Debian 10.x (buster)
        *   Ubuntu 16.04 LTS (xenial)
        *   Ubuntu 18.04 LTS (bionic)
        <!-- *   Ubuntu 20.04 LTS (focal) -->
        *   CentOS 7.x
        *   Amazon Linux 2
        *   CentOS 8.x
        
    !!! warning "Operating System Requirements"
        The modules can only be installed on 64-bit operating systems.

* Envoy

    You can deploy a WAF node as a Docker container. The resulting WAF node installation will contain all necessary modules (see [installation instructions](installation-guides/envoy/envoy-docker.md)).
    
*   The Kubernetes Cluster

    !!! warning "Supported Kubernetes Platform"
        Please note, that the Wallarm NGINX Ingress controllers run only on the Kubernetes platform version 1.15 and lower.
    
    Installing the filter node on the Kubernetes Cluster provides the following options:
    
    *   The NGINX Ingress Controller with Integrated Wallarm Services ([installation instructions][link-ig-ingress-nginx])
    
        !!! info "Konvoy Support"
            Note that you can deploy this Ingress controller on the Konvoy by D2IQ (formerly Mesosphere).
            
            [The Wallarm's installation instructions][link-ig-ingress-nginx] are suitable if you are deploying the Ingress Controller with integrated Wallarm services on the Konvoy. However, you may want to look at [the D2IQ's installation instructions][link-ig-ingress-nginx-d2iq].  
    
    * The Wallarm sidecar container ([installation instructions](installation-guides/kubernetes/wallarm-sidecar-container/))
*   The cloud platforms:
    *   Amazon AWS ([installation instructions][link-ig-aws])
    *   Google Cloud ([installation instructions][link-ig-gcloud])
*   Kong ([installation instructions][link-ig-kong])

    !!! info "Supported Operating Systems"
        The Kong platform must be version 1.4.3 or lower and must be installed on one of the following operating systems:
        
        *   Debian 9.x (stretch)
        *   Ubuntu 16.04 LTS (xenial)
        *   Ubuntu 18.04 LTS (bionic)
        *   CentOS 7.x
