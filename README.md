# NGINX Ingress Controllers

This repo provides implementations of an Ingress controller for NGINX and NGINX Plus.

## What is Ingress?

An Ingress is a Kubernetes resource that lets you configure an HTTP load balancer for your Kubernetes services. Such a load balancer usually exposes your services to the clients outside of your Kubernetes cluster. An Ingress resource supports:
* Exposing services:
    * Via custom URLs (for example, service A at the URL `/serviceA` and service B at the URL `/serviceB`).
    * Via multiple host names (for example, `foo.example.com` for one group of services and `bar.example.com` for another group).
* Configuring SSL termination per each exposed host name.

See the [Ingress User Guide](http://kubernetes.io/docs/user-guide/ingress/) to learn more.

## What is an Ingress Controller?

An Ingress controller is an application that monitors Ingress resources via the Kubernetes API and updates the configuration of a load balancer in case of any changes. Different load balancers require different Ingress controller implementations. Typically, an Ingress controller is deployed as a pod in a cluster. In the case of software load balancers, such as NGINX, an Ingress controller is deployed in a pod along with a load balancer.

See https://github.com/kubernetes/contrib/tree/master/ingress/controllers/ to learn more about Ingress controllers and find out about different implementations.

## NGINX and NGINX Plus Ingress Controllers

We provide Ingress controllers for NGINX and NGINX Plus that support the following Ingress features:
* SSL termination
* Path-based rules
* Multiple host names

Additionally, we provide a mechanism to customize the NGINX configuration.

Refer to the [examples folder](examples) to find out how to [deploy](examples/complete-example) NGINX Ingress Controllers and [customize](examples/customization) the NGINX configuration.

## Difference between NGINX and NGINX Plus controllers

[NGINX Plus](https://www.nginx.com/products/) is a commercial version of NGINX that comes with advanced features and support.

Deployment of the NGINX Plus Ingress Controller requires you to do one extra step: build your own [Docker image](nginx-plus-controller) using the certificate and key of your license.
The Docker image of the NGINX Ingress controller is [available on Docker Hub](https://hub.docker.com/r/nginxdemos/nginx-ingress/).

The NGINX Plus Ingress Controller leverages the advanced features of NGINX Plus, which gives you the following additional benefits:

* **Reduced Number of Configuration Reloads**
Every time the number of pods of services you expose via Ingress changes, the Ingress controller updates the configuration of NGINX to reflect those changes. For open source NGINX software, the configuration file must be changed followed by the configuration reload. For NGINX Plus, we use the [on-the-fly reconfiguration](https://www.nginx.com/products/on-the-fly-reconfiguration/) feature, which allows you to update NGINX Plus on-the-fly without reloading the configuration. This prevents a potential increase of memory usage and overall system overloading, which could occur with frequent configuration reloads.
* **Real-time Statistics**
NGINX Plus provides you with [advanced statistics](https://www.nginx.com/products/live-activity-monitoring/), which you can access either through the API or via the built-in dashboard. This can give you insights into how NGINX Plus and your applications are performing.
* **Session Persistence** When enabled, NGINX Plus makes sure that all the requests from the same client are always passed to the same backend container using the *sticky cookie* method. Refer to the [session persistence examples](examples/session-persistence) to find out how to configure it.

## Advanced load balancing (beyond Ingress)

When your requirements go beyond what Ingress offers, you can use NGINX and
NGINX Plus without the Ingress Controller.

NGINX Plus comes with a [DNS-based dynamic reconfiguration feature](https://www.nginx.com/blog/dns-service-discovery-nginx-plus/), which lets you keep the list of the endpoints of your services in sync with NGINX Plus. Read more about how to setup NGINX Plus this way in [Load Balancing Kubernetes Services with NGINX Plus](https://www.nginx.com/blog/load-balancing-kubernetes-services-nginx-plus/).

## Production status

The status of the NGINX Ingress controllers is currently experimental.

## Contacts

We’d like to hear your feedback! If you have any suggestions or experience issues with our Ingress Controllers, please create an issue or send a pull request on Github.
You can contact us directly via [kubernetes@nginx.com](mailto:kubernetes@nginx.com). Commercial support is available.
