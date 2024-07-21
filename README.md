# Monitoring System with Grafana and Prometheus

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

