# Mimir-Project
Created a single instance and a multiple instance of mimir along with visualization use the same for setup below for VM(Ubuntu) also.
# Working with Mimir

Grafana Mimir is a distributed, horizontally scalable, and highly available long term storage for [Prometheus](https://prometheus.io).

In this Project, you'll:

- Run Grafana Mimir with Docker Compose
- Run Prometheus to scrape some machine metrics and container metrics from node exporter and cadvisor and remote write to Grafana Mimir
- Run Grafana to explore Grafana Mimir dashboards
- Add an alert rule in Grafana Mimir

## Grafana Mimir
Grafana Mimir is an open source software project that provides a scalable long-term storage for Prometheus. Some of the core strengths of Grafana Mimir include:
- Easy to install and maintain
- Massive scalability
- High availability
- Cheap, durable metric storage

## Basic Overview of the how things work
1.Prometheus: Prometheus is an open-source monitoring and alerting toolkit. It collects metrics from monitored targets by scraping HTTP endpoints and stores them in a time-series database. Prometheus is highly customizable and supports a wide range of integrations with various systems and applications.

2.cAdvisor: cAdvisor (Container Advisor) provides container-specific metrics. It is an open-source container monitoring tool developed by Google. cAdvisor automatically collects, aggregates, processes, and exports information about running containers on a given host. This information includes resource usage, performance statistics, and more.

3.MinIO: MinIO is an open-source object storage server compatible with Amazon S3 cloud storage. It provides scalable, high-performance storage for unstructured data. MinIO can be used to store logs, metrics, backups, and other types of data generated by Prometheus, cAdvisor, or other monitoring tools.

4.Mimir: Mimir, as mentioned earlier, can represent a data source or repository of information. In this context, Mimir could be used for advanced data analytics, processing, and visualization. It may ingest data from various sources, including Prometheus, cAdvisor, MinIO, or other systems, and perform analytics to derive insights and trends.

5.Grafana: Grafana is an open-source analytics and visualization platform. It allows users to create, explore, and share dashboards and graphs based on data from multiple sources. Grafana supports various data sources, including Prometheus, MinIO, databases, and more. Users can build customized dashboards to monitor system performance, visualize metrics, and analyze data.

6.NGINX: NGINX is a popular open-source web server and reverse proxy server. In this setup, NGINX can be used as a reverse proxy to route HTTP requests to different services, such as Prometheus, Grafana, or Mimir. NGINX can also provide additional features like load balancing, caching, and SSL termination.

In a typical setup, Prometheus collects metrics from various sources, including cAdvisor for container metrics, and pushes them to nginx. Nginx then fowards these metrics to each of the mimir instances configured in nginx.conf, all of these metrics are stored inside the object storage Minio.And once the metrics are stored it is then requested via adding a prometheus data source in grafana dashboard along with url http://load-balancer:9009/prometheus ,also add the custom headers for authentication(X-Scope-OrgID,value=demo).

For single instance of mimir we can also follow the official documentation of grafana mimir(https://grafana.com/docs/mimir/latest/get-started/).

This setup enables users to monitor system performance, analyze metrics, derive insights from data, and visualize trends using a combination of monitoring tools, storage solutions, analytics platforms, and visualization tools.

## SETUP OF PROJECT

# Note: We will be using docker to setup our service.Make sure you have Docker installed and running on your machine.

Start running your local setup with the following Docker command:

```bash
docker-compose up -d
```
Once the docker compose file runs check wether all the targets in prometheus are UP, which indicates that prometheus is able to scrape all the metrics from its targets properly.

### Grafana Alerting

1. **Create Alerts**:Once you have saved the dashboard open the dashboard then click on the alerts section below the visualization.Click to add new alert,Design your alerts in Grafana by setting up thresholds, conditions, and notification channels.
   
   ### CPU Usage Alert

1. **Create Alert Rule**:
   - Navigate to the Grafana dashboard containing the CPU usage metric you want to monitor.
   - Open the panel menu of the CPU metric panel and select "Edit".
   - Go to the "Alert" tab in the panel editor.
   - Click "Add Condition" and configure the condition as follows:
     - Query: Select the appropriate CPU metric query.
     - Operator: Choose "> (above)".
     - Threshold: Set the threshold value to 80.
   - Specify the duration and evaluation interval for the condition.
   - Select the notification channels to receive alerts (e.g., email).
   - Save the alert rule.

### Container Status Alert

1. **Create Alert Rule**:
   - Navigate to the Grafana dashboard containing the container status metric.
   - Open the panel menu of the container status panel and select "Edit".
   - Go to the "Alert" tab in the panel editor.
   - Click "Add Condition" and configure the condition as follows:
     - Query: Select the query that indicates the status of containers (e.g., "container_up").
     - Operator: Choose "=" for equality.
     - Threshold: Set the threshold value to 0 to check if any container is down.
   - Specify the duration and evaluation interval for the condition.
   - Select the notification channels to receive alerts (e.g., email).
   - Save the alert rule.

2. **Configure Notification Channels**: Go to the "Alerting" section in Grafana settings and add notification channels for email and Discord. Example configurations are provided below:

    - **Email Notification Channel:**
        - Name: Enter a name for the channel (e.g., "Email Alerts").
        - Type: Choose "Email" from the dropdown.
        - Include the necessary email to which alerts need to be sent.
        - Test the connection to ensure it's working.

    - **Discord Notification Channel:**
        - Name: Enter a name for the channel (e.g., "Discord Alerts").
        - Type: Choose "Discord" from the dropdown.
        - Paste your Discord Webhook URL.
        - Test the connection to ensure it's working.






