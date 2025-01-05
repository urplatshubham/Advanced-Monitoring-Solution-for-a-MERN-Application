# Advanced Monitoring Solution for a MERN Application

## Project Overview

This project implements an advanced monitoring solution for a MERN (MongoDB, Express, React, Node.js) application using **Grafana** and **Prometheus**. The monitoring setup includes performance metrics for the backend, frontend, and database, as well as log aggregation and distributed tracing.

The MERN application used for this project is the **Travel Memory Application**, which was cloned from [UnpredictablePrashant's GitHub repository](https://github.com/UnpredictablePrashant/TravelMemory).

## Objectives

- Deploy a MERN application.
- Integrate Prometheus to expose custom metrics for API performance.
- Monitor MongoDB performance using MongoDB Exporter.
- Create advanced Grafana dashboards for detailed monitoring.
- Set up log aggregation using tools like Loki.
- Implement distributed tracing using Jaeger or Zipkin.
- Configure alerting and anomaly detection.

---

## Features

1. **Backend Monitoring**:

   - Custom metrics for API response times, request counts, and error rates.
   - Integration with Prometheus for metric exposure.

2. **Database Monitoring**:

   - Use MongoDB Exporter to track performance metrics such as connections, queries, and cache usage.

3. **Frontend Monitoring**:

   - Visualize application performance in Grafana.

4. **Log Aggregation**:

   - Collect and visualize logs using Loki.

5. **Distributed Tracing**:

   - End-to-end request tracing with Jaeger or Zipkin.

6. **Alerting and Anomaly Detection**:

   - Define Grafana alerts based on application-specific metrics.

---

## Prerequisites

1. **Node.js** (>= 16.x) and npm
2. **MongoDB** (Local or hosted, e.g., MongoDB Atlas)
3. **Docker** (optional but recommended for Prometheus, Grafana, Loki, etc.)
4. **Git**
5. **Prometheus**
6. **Grafana**
7. **Loki**
8. **Jaeger** or **Zipkin**

---

## Steps to Complete the Assignment

### 1. MERN Application Setup

- Clone or use the Travel Memory application repository:
  ```bash
  git clone https://github.com/UnpredictablePrashant/TravelMemory.git
  cd TravelMemory
  ```
- Install dependencies and run the backend and frontend locally:
  ```bash
  cd backend
  npm install
  npm start

  cd ../frontend
  npm install
  npm start
  ```

### 2. Integrate Prometheus

- Add Prometheus client libraries to the backend to expose custom metrics:

  ```bash
  npm install prom-client
  ```

- Example for exposing metrics:

  ```javascript
  const client = require('prom-client');
  const express = require('express');
  const app = express();

  const requestCounter = new client.Counter({
      name: 'http_requests_total',
      help: 'Total number of requests',
  });

  app.use((req, res, next) => {
      requestCounter.inc();
      next();
  });

  app.get('/metrics', (req, res) => {
      res.set('Content-Type', client.register.contentType);
      res.end(client.register.metrics());
  });
  ```

- Run Prometheus in Docker:

  ```bash
  docker run -d --name prometheus -p 9090:9090 prom/prometheus
  ```

### 3. Monitor MongoDB

- Use MongoDB Exporter to monitor database performance:

  ```bash
  docker run -d -p 9216:9216 --name mongodb-exporter -e MONGODB_URI=mongodb://localhost:27017 bitnami/mongodb-exporter:latest
  ```

- Add MongoDB Exporter to Prometheus configuration.

### 4. Create Grafana Dashboards

- Run Grafana in Docker:
  ```bash
  docker run -d -p 3000:3000 --name=grafana grafana/grafana
  ```
- Add Prometheus as a data source in Grafana.
- Create dashboards to display backend performance, MongoDB stats, and frontend performance.

### 5. Log Aggregation with Loki

- Run Loki and Promtail in Docker:
  ```bash
  docker run -d --name=loki -p 3100:3100 grafana/loki:latest
  docker run -d --name=promtail -v /var/log:/var/log -p 9080:9080 grafana/promtail:latest
  ```
- Add Loki as a data source in Grafana.
- Query and visualize logs.

### 6. Distributed Tracing with Jaeger

- Run Jaeger in Docker:
  ```bash
  docker run -d --name=jaeger -p 16686:16686 -p 14268:14268 jaegertracing/all-in-one:latest
  ```
- Integrate Jaeger with the backend application using OpenTelemetry:
  ```bash
  npm install @opentelemetry/api @opentelemetry/node
  ```
- Add Jaeger as a data source in Grafana.

### 7. Alerting and Anomaly Detection

- Configure Grafana alerts for key metrics such as error rates or high response times.
- Set up email, Slack, or webhook notifications.

---

## Deliverables

1. **Documentation**:

   - Step-by-step process for setting up monitoring.
   - Challenges faced and solutions.

2. **Screenshots**:

   - Grafana dashboards showing key metrics, logs, and traces.

3. **Analysis**:

   - Insights from the collected metrics, logs, and traces.

4. **Optional Enhancements**:

   - Auto-scaling based on metrics.
   - Service mesh integration with Istio or Linkerd.

---

## Commands Quick Reference

### Start MERN Application:

```bash
cd backend && npm start
cd frontend && npm start
```

### Run Prometheus:

```bash
docker run -d --name prometheus -p 9090:9090 prom/prometheus
```

### Run Grafana:

```bash
docker run -d -p 3000:3000 --name=grafana grafana/grafana
```

### Run MongoDB Exporter:

```bash
docker run -d -p 9216:9216 --name mongodb-exporter -e MONGODB_URI=mongodb://localhost:27017 bitnami/mongodb-exporter:latest
```

### Run Loki:

```bash
docker run -d --name=loki -p 3100:3100 grafana/loki:latest
```

### Run Jaeger:

```bash
docker run -d --name=jaeger -p 16686:16686 -p 14268:14268 jaegertracing/all-in-one:latest
```

