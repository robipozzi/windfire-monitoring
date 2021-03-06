# Windfire Monitoring
- [Introduction](#introduction)
- [Before you start](#before-you-start)
- [Setup and run the stack](#setup-and-run-the-stack)
- [Nginx](#nginx)
- [Windfire Restaurants](#windfire-restaurants)
- [Grafana](#grafana)
- [Prometheus](#prometheus)
- [cAdvisor](#cadvisor)
- [Node Exporter](#node-exporter)
- [Portainer](#portainer)

## Introduction
This repository contains the scripts needed to configure and run an opinionated monitoring stack for containerized applications.

All the tools are configured to run behind a Reverse Proxy, implemented with Nginx (https://www.nginx.com/), which shields and concentrates the access to several applications, as seen in the architecture diagram below.

![](images/architecture.png)

Nginx runs as a container and is configured as a Reverse Proxy, with several *proxy_pass* directives that redirect to the following components running behind the proxy (see [Nginx](#nginx) paragraph for more details):
- [Windfire Restaurants](#windfire-restaurants)
- [Grafana](#grafana)
- [Prometheus](#prometheus)
- [cAdvisor](#cadvisor)
- [Portainer](#portainer)

Node Exporter does not have a relevant UI and for this reason it runs in the backend and is not exposed through Nginx (see [Node Exporter](#node-exporter) paragraph for more details).

## Before you start
Before starting to experiment with the samples provided in this repository, you just need to have Docker and Docker Compose properly installed in your environment.

Refer to the following:
- https://docs.docker.com/engine/install/ for Docker engine installation
- https://docs.docker.com/compose/install/ for Docker Compose installation

Once done with the prerequisites installation, clone this repository to **$HOME/dev/windfire-monitoring** directory.

To setup and run the whole stack, I provided the Docker Compose file **[docker-compose.yaml](docker-compose.yaml)** that defines all the configurations needed. Since some of the containers in the stack need to read configuration files from filesystem and Volumes to persist data, before running the stack you will need to adjust the directories according to your specific environment, i.e.:

- Prometheus
    - the volume mapping *$HOME/dev/windfire-monitoring/prometheus/prometheus-proxied.yml:/etc/prometheus/prometheus.yml* tells the container to read Prometheus configuration file from a specific directory on the filesystem: change *$HOME/dev/windfire-monitoring/prometheus* according to your environment.
- Grafana 
    - the volume mapping *$HOME/dev/windfire-monitoring/grafana/conf:/usr/share/grafana/conf* tells the container to load configurations from a specific directory on the filesystem: change *$HOME/dev/windfire-monitoring/grafana/conf* according to your environment.
    - the volume mapping *$HOME/temp/monitoring/grafana:/var/lib/grafana* tells the container where to persist data generated during its execution: change *$HOME/temp/monitoring/grafana* according to your environment
- Portainer
    - change directory *$HOME/temp/monitoring/portainer* according to where you want Portainer container to store data generated during its execution.
- Nginx
    - the volume mapping *$HOME/dev/windfire-monitoring/nginx/default.conf:/etc/nginx/conf.d/default.conf* tells the container to read Nginx configuration file from a specific directory on the filesystem: change *$HOME/dev/windfire-monitoring/nginx* according to your environment.

## Setup and run the stack
To start the whole stack just run **[run-stack.sh](run-stack.sh)** script which uses **[docker-compose.yaml](docker-compose.yaml)**.

## Nginx
[Nginx](https://www.nginx.com/) is a free, open source HTTP web server, mail proxy server, reverse proxy and load balancer for HTTP, TCP and UDP traffic.

Nginx behavior is defined in a specific configuration file named **[default.conf](nginx/default.conf)**, that is loaded at container startup via a volume mapping.

## Windfire Restaurants 
Windfire Restaurants is a custom microservice I develop that simply exposes the */restaurants* endpoint to return a list of restaurants.

## Grafana
[Grafana](https://grafana.com/) is an open-source platform for data visualization, monitoring and analysis that has become quite popular as a tool for Observability, allowing to query, visualize, alert on and understand environment metrics no matter where they are stored.

## Prometheus
[Prometheus](https://prometheus.io/) is an open-source system monitoring and alerting toolkit. It is configured via **[prometheus.yml](prometheus/prometheus.yml)** specific configuration file that is that is loaded at container startup via a volume mapping.

## cAdvisor
[cAdvisor](https://github.com/google/cadvisor) provides container users an understanding of the resource usage and performance characteristics of their running containers. It is a running daemon that collects, aggregates, processes, and exports information about running containers. Specifically, for each container it keeps resource isolation parameters, historical resource usage, histograms of complete historical resource usage and network statistics.

## Node Exporter
[Node Exporter](https://github.com/prometheus/node_exporter) is a Prometheus exporter for hardware and OS metrics with pluggable metric collectors.

## Portainer
[Portainer](https://www.portainer.io/) is an open source tool for managing container based applications in Kubernetes, Docker, Docker Swarm, Azure ACI and edge environments. It can be used to simplify and streamline all the tasks needed to manage containers effectively, such as starting, stopping, opening a terminal into the running container, accessing the logs and much more.