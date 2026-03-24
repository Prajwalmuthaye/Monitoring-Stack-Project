# Monitoring Stack with Prometheus & Grafana on AWS EC2

## Project Overview

This project demonstrates how to deploy a **basic monitoring stack** using **Prometheus** and **Grafana** on an AWS EC2 instance running Amazon Linux.

The setup collects system metrics using Node Exporter and visualizes them in Grafana dashboards.

---

## Architecture

```
EC2 (Amazon Linux)
   │
   ├── Prometheus (Metrics Collection)
   ├── Node Exporter (System Metrics)
   └── Grafana (Visualization Dashboard)
```

---

## Tech Stack

* AWS EC2 (Amazon Linux 2023)
* Docker & Docker Compose
* Prometheus
* Grafana
* Node Exporter

---

## EC2 Setup

### 1. Launch EC2 Instance

* AMI: Amazon Linux 2023
* Instance Type: t2.micro (Free Tier)
* Security Group Ports:

  * 22 (SSH)
  * 3000 (Grafana)
  * 9090 (Prometheus)
  * 9100 (Node Exporter)

---

### 2. Connect to EC2

```bash
ssh -i monitoring-key.pem ec2-user@<EC2-PUBLIC-IP>
```

---

### 3. Install Docker

```bash
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user
```

Re-login to apply changes.

---

### 4. Install Docker Compose

```bash
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

---

## Project Setup

### 1. Clone Repository

```bash
git clone https://github.com/Prajwalmuthaye/Monitoring-Stack-Project.git
cd Monitoring-Stack-Project
```

---

### 2. Prometheus Configuration

Create `prometheus.yml`

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['node-exporter:9100']
```

---

### 3. Docker Compose File

Create `docker-compose.yml`

```yaml
version: '3'

services:
  prometheus:
    image: prom/prometheus
    container_name: prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana
    container_name: grafana
    ports:
      - "3000:3000"

  node-exporter:
    image: prom/node-exporter
    container_name: node-exporter
    ports:
      - "9100:9100"
```

---

## Run the Stack

```bash
docker-compose up -d
```

---

## Access Services

| Service    | URL                  |
| ---------- | -------------------- |
| Grafana    | http://<EC2-IP>:3000 |
| Prometheus | http://<EC2-IP>:9090 |

---

## Grafana Login

* Username: admin
* Password: admin

---

## Grafana Setup

### Add Prometheus Data Source

* URL: `http://prometheus:9090`

### Import Dashboard

* Dashboard ID: **1860** (Node Exporter Full Dashboard)

---

## Metrics Monitored

* CPU Usage
* Memory Usage
* Disk Usage
* Network Traffic

---

## Screenshots 

<img width="1497" height="236" alt="image" src="https://github.com/user-attachments/assets/35e68b0b-b9a9-40d6-b947-a739e97b74b7" />



Inspired by real-world DevOps monitoring practices.
