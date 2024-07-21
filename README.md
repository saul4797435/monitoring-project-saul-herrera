# Monitoring System with Grafana and Prometheus

## Description
This program sets up a monitoring system using Grafana and Prometheus to monitor a Node.js application. The monitoring system collects and visualizes metrics to help you understand the performance and health of your Node.js application.

## Purpose
The purpose of this monitoring system is to provide a robust solution for tracking various performance metrics of a Node.js application, including CPU usage, memory usage, and other custom metrics. By using Prometheus to collect metrics and Grafana to visualize them, you can gain insights into your application's behavior and performance in real-time.

## How It Works
1. **Prometheus**: Prometheus is a powerful monitoring and alerting toolkit that scrapes metrics from monitored targets. In this setup, it collects metrics from a Node.js application and its own metrics endpoint.
   
2. **Grafana**: Grafana is an open-source platform for monitoring and observability. It provides a rich set of dashboards to visualize the metrics collected by Prometheus.

3. **Node.js Application**: The Node.js application is instrumented with the `prom-client` library to expose custom metrics to Prometheus. These metrics are available at the `/metrics` endpoint of the application.

## Key Features
- **Real-time Monitoring**: Collects and visualizes metrics in real-time to provide immediate insights into the application's performance.
- **Custom Metrics**: Allows for the collection of custom metrics from the Node.js application, tailored to specific monitoring needs.
- **User-friendly Dashboards**: Grafana provides easy-to-use and highly customizable dashboards to visualize the data collected by Prometheus.

## Benefits
- **Improved Performance**: By monitoring key metrics, you can identify and address performance bottlenecks.
- **Proactive Issue Detection**: Alerts can be set up in Prometheus to notify you of potential issues before they impact end users.
- **Data-Driven Insights**: Visualizing metrics in Grafana helps in making informed decisions based on real data.

## Objective
Implement a monitoring system using Grafana and Prometheus to monitor a Node.js application.

## Steps to Set Up the Project

1. **Create and Edit `docker-compose.yml`**:
   - Create a file named `docker-compose.yml` and add the following content:
     ```yaml
     version: '3.7'
     services:
       prometheus:
         image: prom/prometheus
         volumes:
           - ./prometheus.yml:/etc/prometheus/prometheus.yml
         ports:
           - "9090:9090"
       grafana:
         image: grafana/grafana
         ports:
           - "3000:3000"
     ```

2. **Create and Edit `prometheus.yml`**:
   - Create a file named `prometheus.yml` and add the following content:
     ```yaml
     global:
       scrape_interval: 15s
     scrape_configs:
       - job_name: 'prometheus'
         static_configs:
           - targets: ['localhost:9090']
     ```

3. **Set Up the Node.js Application**:
   - Create a file named `server.js` and add the following content:
     ```javascript
     const express = require('express');
     const client = require('prom-client');

     const app = express();
     const collectDefaultMetrics = client.collectDefaultMetrics;

     collectDefaultMetrics();

     app.get('/metrics', async (req, res) => {
       res.set('Content-Type', client.register.contentType);
       res.end(await client.register.metrics());
     });

     app.listen(3000, () => {
       console.log('Server is running on port 3000');
     });
     ```

4. **Install `prom-client`**:
   - Run the following command to install `prom-client`:
     ```sh
     npm install prom-client
     ```

5. **Run the Services with Docker Compose**:
   - Execute the following command to start the services:
     ```sh
     docker-compose up
     ```

## Configuration and Access

- **Access Grafana**:
  - Open your browser and go to `http://localhost:3000`
  - Use the default credentials (username: `admin`, password: `admin`) to log in
  - Change the password upon first login

- **Access Prometheus**:
  - Open your browser and go to `http://localhost:9090`

- **Access Node.js Application Metrics**:
  - Open your browser and go to `http://localhost:3000/metrics`

## Repository Structure
- `docker-compose.yml`: Configuration file for Docker Compose.
- `prometheus.yml`: Configuration file for Prometheus.
- `server.js`: Sample Node.js application exposing metrics.
- `README.md`: This file, containing setup and usage instructions.

## Conclusion
Following these steps, you will have a fully functional monitoring system using Grafana and Prometheus to monitor a Node.js application.

