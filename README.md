
# 🚀 Monitoring Stack with Docker: Prometheus + Grafana + Node Exporter

This project sets up a full-fledged containerized system monitoring solution using **Prometheus**, **Grafana**, and **Node Exporter** via Docker Compose.

---

## 🛠️ Stack Overview

| Component     | Role                                  |
|---------------|---------------------------------------|
| Prometheus    | Collects and stores time-series metrics |
| Node Exporter | Exposes hardware and OS metrics         |
| Grafana       | Visualizes metrics via dashboards       |

---

## 📂 Folder Structure

```
Container/
├── compose.yml
├── prometheus.yml
└── README.md
```

---

## ⚙️ Setup Instructions (VS Code + Docker Desktop)

### 1. Clone or Create Project Directory

```bash
mkdir Container && cd Container
```

### 2. Add `compose.yml`

```yaml
version: "3.8"

services:
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml

  node-exporter:
    image: prom/node-exporter
    ports:
      - "9100:9100"

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin
```

### 3. Add `prometheus.yml`

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "node"
    static_configs:
      - targets: ["node-exporter:9100"]
```

### 4. Start the Stack

```bash
docker-compose -f compose.yml up --build
```

---

## 🔍 Access Services

- **Prometheus:** [http://localhost:9090](http://localhost:9090)
- **Grafana:** [http://localhost:3000](http://localhost:3000)
- **Node Exporter:** [http://localhost:9100](http://localhost:9100/metrics)

---

## 🧪 Grafana Login

Use default credentials:

```
Username: admin
Password: admin
```

---

## 📊 Add Prometheus as Data Source in Grafana

1. Go to **Grafana > Settings > Data Sources**
2. Choose **Prometheus**
3. Set URL: `http://prometheus:9090`
4. Click **Save & Test**

---

## 📥 Import Monitoring Dashboard

1. Go to **+ > Import**
2. Use Dashboard ID `1860` (Node Exporter Full)
3. Select Prometheus as data source
4. Click **Import**

---

## 🖼️ Screenshots

### ✔️ Docker Containers Running
![Containers](./screenshots/docker-containers.png)

### ✔️ Grafana Login
![Grafana Login](./screenshots/grafana-login.png)

### ✔️ Dashboard Example
![Dashboard](./screenshots/grafana-dashboard.png)

---
![image](https://github.com/user-attachments/assets/1249f1f9-2dc2-445a-81e5-0bdd96e47405)

## 🧠 Observations

- **Grafana authentication failed** initially due to incorrect credentials.
- Reset was successful by passing credentials in `compose.yml`.
- Prometheus scraped metrics from `node-exporter` successfully.
- Imported dashboard shows CPU, RAM, disk usage live.

---

## 🛠️ Common Issues

| Problem                                | Fix                                                             |
|----------------------------------------|------------------------------------------------------------------|
| Grafana "user not found"               | Use correct credentials or reset using environment variables     |
| Prometheus not scraping metrics        | Ensure target container name `node-exporter:9100` is correct     |
| Docker not starting containers         | Check Docker Desktop is running with Linux engine                |
| Version warning in Compose             | Remove the `version` field from `compose.yml`                    |

---

## 📌 Comments

- This project demonstrates a clean setup of observability tools.
- Easily extendable to include `alertmanager`, `cadvisor`, or custom exporters.
- Great for learning DevOps practices and system introspection.

---

## 🧾 Author

> Created by Ishika Verma 
> Date: April 2025  
> Tools: Docker, VS Code, Prometheus, Grafana, Node Exporter
