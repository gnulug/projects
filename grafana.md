# Grafana/InfluxDB

### Grafana/InfluxDB System ###

{influxdb,grafana}.gnulug.org -> docker1.gnulug.org
* Collectd sends data to InfluxDB
* Grafana retrieves and graphs data from InfluxDB

Services are running from containers.

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
$ docker built -t tutum/influxdb .
$ mkdir /opt/influxdb
$ docker run -d --name influxdb -e PRE_CREATE_DB="collectd" -p 8083:8083 -p 8080:8080 -p 2003:2003 -p 25826:25826/udp --expose 8090 --expose 8099 -v /opt/influxdb/:/data  tutum/influxdb
```

Grafana:
```
$ git clone https://github.com/tutumcloud/tutum-docker-grafana
$ cd tutum-docker-influxdb
$ docker built -t tutum/grafana .
$ docker run -d --name grafana -p 80:80 -e INFLUXDB_HOST=influxdb.gnulug.org -e INFLUXDB_PORT=8080 -e INFLUXDB_NAME=collectd -e INFLUXDB_USER=user -e INFLUXDB_PASS=pass -e INFLUXDB_IS_GRAFANADB=true -e HTTP_USER=user -e HTTP_PASS=pass tutum/grafana
```
