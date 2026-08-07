# Prometheus & Grafana Installation on Ubuntu EC2 (AWS)

This guide covers installing Prometheus, Grafana, and Node Exporter on Ubuntu EC2.

## Prerequisites

- Ubuntu 22.04/24.04 EC2
- Open ports: 22, 3000, 9090, 9100

```bash
sudo apt update && sudo apt upgrade -y
```

## Install Prometheus

```bash
sudo useradd --no-create-home --shell /bin/false prometheus
sudo mkdir /etc/prometheus /var/lib/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
wget https://github.com/prometheus/prometheus/releases/latest/download/prometheus-3.7.0.linux-amd64.tar.gz
tar -xvf prometheus-*.tar.gz
cd prometheus-*
sudo cp prometheus promtool /usr/local/bin/
sudo cp prometheus.yml /etc/prometheus/
sudo cp -r consoles console_libraries /etc/prometheus/
sudo chown -R prometheus:prometheus /etc/prometheus
```

Create `/etc/systemd/system/prometheus.service`:

```ini
[Unit]
Description=Prometheus
After=network-online.target

[Service]
User=prometheus
Group=prometheus
ExecStart=/usr/local/bin/prometheus --config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path=/var/lib/prometheus --web.console.templates=/etc/prometheus/consoles --web.console.libraries=/etc/prometheus/console_libraries
Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now prometheus
```

Access: http://<EC2-PUBLIC-IP>:9090

## Install Grafana

```bash
sudo apt install -y apt-transport-https software-properties-common wget
wget -q -O - https://apt.grafana.com/gpg.key | sudo gpg --dearmor -o /usr/share/keyrings/grafana.gpg
echo "deb [signed-by=/usr/share/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
sudo apt update
sudo apt install -y grafana
sudo systemctl enable --now grafana-server
```

Access: http://<EC2-PUBLIC-IP>:3000

Default login:
- Username: admin
- Password: admin

Add Prometheus datasource: `http://localhost:9090`

## Install Node Exporter

```bash
sudo useradd --no-create-home --shell /bin/false node_exporter
wget https://github.com/prometheus/node_exporter/releases/latest/download/node_exporter-1.9.1.linux-amd64.tar.gz
tar -xvf node_exporter-*.tar.gz
sudo cp node_exporter-*/node_exporter /usr/local/bin/
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

Create `/etc/systemd/system/node_exporter.service`:

```ini
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
Group=node_exporter
ExecStart=/usr/local/bin/node_exporter
Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now node_exporter
```

Edit `/etc/prometheus/prometheus.yml`:

```yaml
scrape_configs:
  - job_name: node_exporter
    static_configs:
      - targets: ["localhost:9100"]
```

```bash
sudo systemctl restart prometheus
```

Verify:
- Prometheus: http://<EC2-PUBLIC-IP>:9090
- Targets: http://<EC2-PUBLIC-IP>:9090/targets
- Grafana: http://<EC2-PUBLIC-IP>:3000
- Node Exporter: http://<EC2-PUBLIC-IP>:9100/metrics
