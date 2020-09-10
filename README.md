# Monitoring

This Repo is for Container Monitoring using CAdvisor, Prometheus and Grafana.

1) Run CAdvisor using:

sudo docker run -d \
--volume=/:/rootfs:ro \
--volume=/var/run:/var/run:ro \
--volume=/sys:/sys:ro \
--volume=/var/lib/docker/:/var/lib/docker:ro \
--volume=/dev/disk/:/dev/disk:ro \
--publish=8080:8080 \
--name=cadvisor \
google/cadvisor:latest


Check your container resources Information at http://localhost:8080


2) Copy Prometheus config file at /tmp/prometheus.yml

cp prometheus.yml /tmp/prometheus.yml

3) append your host IP under /tmp/prometheus.yml file under below two sections:

    static_configs:

      - targets: ['docker.for.mac.host.internal:8080']

    static_configs:

      - targets: ['docker.for.mac.host.internal:9100']


4) Run prometheus container
$ docker run -d --name prom -p 9090:9090 -v /tmp/prometheus.yml:/etc/prometheus/prometheus.yml  prom/prometheus:v2.14.0
Check promethus at localhost port 9090
 http://localhost:9090
 
 
5) Run Grafana container
docker run -d --name prom-dashboard -p 3000:3000 grafana/grafana:6.4.4
Check grafana at localhost port 3000
 http://localhots:3000
 use credentials admin/admin to login.


6) Run Node-Exporter container (for node metrics)
docker run -d --name node-exporter -p 9100:9100 prom/node-exporter
Check Node Exporter at below address:
http://localhost:9100

7) Check traget section in Prometheus, both targets (Container Advisor and Node Exporter) should be up and running.

8) Add Prometheus as data source in Grafana, using Server IP address and port 9090. Save the config.

9) Create a new dashboard using Import method using json file - "cadvisor-grafana-dashboard.json". Save/Publish the dashboard.

10) Monitor your resources against various metrics.


#Clean up:

To Delete containers:
$ docker container rm -f prom prom-dashboard node-exporter cadvisor

To Delete images
$ docker image rm prom/prometheus:v2.14.0 grafana/grafana:6.4.4 prom/node-exporter google/cadvisor





