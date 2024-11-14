
# Docker Monitoring Stack (Prometheus, Grafana, Alertmanager, Node Exporter)

This project sets up a monitoring stack using **Prometheus**, **Grafana**, **Alertmanager**, and **Node Exporter** with **Docker Compose**. It is designed to provide a simple way to collect and visualize metrics from your system or containers and set up alerts based on those metrics.
# ![Monitoring Stack](https://devopscube.com/wp-content/uploads/2023/11/image-113.png)
## Overview

This monitoring stack includes the following services:
- **Prometheus**: A time-series database for collecting and storing metrics.
- **Grafana**: A web interface for visualizing metrics stored in Prometheus.
- **Alertmanager**: Handles alert notifications based on Prometheus metrics.
- **Node Exporter**: A tool for exporting hardware and OS metrics to Prometheus.

These services will run as containers using **Docker Compose**, which manages multi-container Docker applications.

## Prerequisites

Before you start, make sure you have the following installed:
- **Docker**: The platform for containerizing your services.
- **Docker Compose**: A tool for defining and running multi-container Docker applications.

## Setup Instructions

### Step 1: Clone the repository

Clone the repository to your local machine:

```bash
git clone https://github.com/Rudraksh2003/Docker-Monitoring-Stack-Prometheus-Grafana-Alertmanager-Node-Exporter-.git
cd Docker-Monitoring-Stack-Prometheus-Grafana-Alertmanager-Node-Exporter-
```

### Step 2: Build and Start Containers

To build and start the services in detached mode, run the following command:

```bash
docker-compose up -d
```

This command will:
- Build the containers (if necessary).
- Start Prometheus, Grafana, Alertmanager, and Node Exporter containers.
- Map ports so that you can access these services via your browser.

### Step 3: Access the Services

Once the containers are up and running, you can access the services using the following URLs:

- **Prometheus**: [http://localhost:9090](http://localhost:9090)
- **Grafana**: [http://localhost:3000](http://localhost:3000)
  - Default login: `admin` (You can change this via environment variables in the `docker-compose.yml`).
- **Alertmanager**: [http://localhost:9103](http://localhost:9103)
- **Node Exporter**: [http://localhost:9100](http://localhost:9100)

### Step 4: Stop and Remove Containers

If you want to stop and remove the containers, you can run:

```bash
docker-compose down
```

If you want to remove the containers, volumes, and networks created by the `docker-compose up` command:

```bash
docker-compose down --volumes --remove-orphans
```

## Docker Compose Explanation

Here is an explanation of the `docker-compose.yml` file and what each section does:

### Version

```yaml
version: '3.8'
```

- Defines the version of Docker Compose syntax being used. Here, we are using version `3.8`.

### Services

This section defines the individual services (containers) that will run.

#### Prometheus

```yaml
prometheus:
  image: prom/prometheus:latest
  container_name: prometheus
  volumes:
    - ./prometheus.yml:/etc/prometheus.yml
  ports:
    - "9090:9090"
```

- **Image**: Specifies the Docker image to use for Prometheus (`prom/prometheus:latest`).
- **Container Name**: Names the container `prometheus`.
- **Volumes**: Mounts the `prometheus.yml` file from your local machine to the container. This file contains the Prometheus configuration.
- **Ports**: Maps the container's port `9090` to the host machine's port `9090` so you can access the Prometheus web interface.

#### Node Exporter

```yaml
node-exporter:
  image: prom/node-exporter:latest
  container_name: node-exporter
  ports:
    - "9100:9100"
```

- **Image**: Specifies the Docker image for Node Exporter.
- **Container Name**: Names the container `node-exporter`.
- **Ports**: Maps port `9100` to expose Node Exporter metrics to Prometheus.

#### Alertmanager

```yaml
alert:
  image: prom/alertmanager:latest
  container_name: alert-manager
  ports:
    - "9103:9103"
  volumes:
   - ./alert.yaml:/etc/alert.yaml
  command:
    - --config.file=/etc/alertmanager/alertmanager.yml
  restart: always
  networks:
    - monitor
```

- **Image**: Specifies the Docker image for Alertmanager.
- **Container Name**: Names the container `alert-manager`.
- **Ports**: Maps port `9103` for accessing the Alertmanager web interface.
- **Volumes**: Mounts the `alert.yaml` file from your local machine for configuring Alertmanager.
- **Command**: Specifies the configuration file for Alertmanager.
- **Restart**: Ensures that the container always restarts if it stops unexpectedly.
- **Networks**: Defines the custom network `monitor` for the services to communicate.

#### Grafana

```yaml
grafana:
  image: grafana/grafana:latest
  container_name: grafana
  ports:
    - "3000:3000"
  environment:
    - GF_SECURITY_ADMIN_PASSWORD=admin
  volumes:
    - grafana-storage:/var/lib/grafana
    - ./grafana-provisioning:/etc/grafana/provisioning
```

- **Image**: Specifies the Docker image for Grafana.
- **Container Name**: Names the container `grafana`.
- **Ports**: Maps port `3000` to the host machine's port `3000` for Grafana's web interface.
- **Environment**: Sets the `GF_SECURITY_ADMIN_PASSWORD` environment variable to define the default admin password.
- **Volumes**: 
  - Mounts the `grafana-storage` volume for persistent data storage.
  - Mounts a local folder `grafana-provisioning` to `/etc/grafana/provisioning` for provisioning Grafana dashboards and datasources.

### Volumes

```yaml
volumes:
  grafana-storage:
```

- This section defines a Docker volume for persistent Grafana data. The volume ensures that data is not lost when the container is stopped or removed.

### Networks

```yaml
networks:
  monitor:
    driver: bridge
```

- **Networks**: Defines a custom Docker network `monitor`, allowing the services to communicate securely and independently from other containers or services on your system.

---

## Conclusion

This project provides a simple and effective way to deploy a monitoring stack using Docker Compose. It enables you to collect system metrics, visualize them with Grafana, and set up alerts for any issues with Alertmanager.

For more information on each service:
- [Prometheus Documentation](https://prometheus.io/docs/)
- [Grafana Documentation](https://grafana.com/docs/)
- [Alertmanager Documentation](https://prometheus.io/docs/alerting/latest/alertmanager/)
- [Node Exporter Documentation](https://github.com/prometheus/node_exporter)

---
