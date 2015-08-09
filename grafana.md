# Grafana/InfluxDB

### Grafana/InfluxDB System ###

{influxdb,grafana}.gnulug.org -> docker1.gnulug.org
* Collectd sends data to InfluxDB
* Grafana retrieves and graphs data from InfluxDB

Services are running from containers.

### Examples ###

![Grafana Screenshot](http://jonschipp.com/lug/grafana.png)

### Use ###

Web interfaces:
```
http://influxdb.gnulug.org:8083 (Admin: Use proxy)
http://influxdb.gnulug.org:8080 (API)
http://grafana.gnulug.org (Admin)
```

API:
```
curl -G 'http://influxdb.gnulug.org:8080/db/collectd/series?u=user&p=pass&pretty=true' --data-urlencode 'q=select * from "hammy/memory/memory-used" limit 1'
curl -G 'http://influxdb.gnulug.org:8080/db/collectd/series?u=user&p=pass' --data-urlencode 'q=select * from "docker1/cpu-0/cpu-idle" limit 100'
```

### Configuration ###

InfluxDB:
```
$ git clone https://github.com/tutumcloud/tutum-docker-influxdb
$ cd tutum-docker-influxdb
$ grep -A 1 api config.toml
  # Configure the http api
  [api]
  port     = 8080
$ docker build -t tutum/influxdb .
$ mkdir /opt/influxdb
$ docker run -d --name influxdb -e PRE_CREATE_DB="collectd" -p 8083:8083 -p 8080:8080 -p 2003:2003 -p 25826:25826/udp --expose 8090 --expose 8099 -v /opt/influxdb/:/data  tutum/influxdb
```

Grafana:
```
mkdir /opt/grafana
docker run -d -i -p 80:80 --name grafana2 -e "GF_SERVER_ROOT_URL=http://grafana.gnulug.org" -e "GF_SERVER_HTTP_PORT=80" -e "GF_SERVER_DOMAIN=grafana.gnulug.org" -e "GF_SECURITY_ADMIN_USER=user" -e "GF_SECURITY_ADMIN_PASSWORD=pass" -v /opt/grafana/:/opt/grafana/data grafana/grafana:develop
```
