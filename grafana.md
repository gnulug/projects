# Grafana/InfluxDB

### Grafana/InfluxDB System ###

{influxdb,grafana}.gnulug.org -> docker1.gnulug.org
* Collectd sends data to InfluxDB
* Grafana retrieves and graphs data from InfluxDB

Services are running from containers.

Web interfaces:
```
http://influxdb.gnulug.org:8083 (Use proxy)
http://grafana.gnulug.org
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
$ docker run -d  -p 8083:8083 -p 8080:8080 -p 2003:2003 -p 25826:25826/udp --expose 8090 --expose 8099 -v /opt/influxdb/:/data  tutum/influxdb
```

Grafana:
```
$ git clone https://github.com/tutumcloud/tutum-docker-grafana
$ cd tutum-docker-influxdb
$ grep -A 14 datasources config.js
    datasources: {
      collectd: {
        type: 'influxdb',
        url: "<--PROTO-->://<--ADDR-->:<--PORT-->/db/<--DB_NAME-->",
        username: "<--USER-->",
        password: "<--PASS-->",
      },
      grafana: {
         type: 'influxdb',
         url: "<--PROTO-->://<--ADDR-->:<--PORT-->/db/<--DB_NAME-->",
         username: "<--USER-->",
         password: "<--PASS-->",
         grafanaDB: true
       },
    },
$ docker built -t tutum/grafana .
$ docker run -d -p 80:80 -e INFLUXDB_HOST=influxdb.gnulug.org -e INFLUXDB_PORT=8080 -e INFLUXDB_NAME=collectd -e INFLUXDB_USER=user -e INFLUXDB_PASS=pass -e INFLUXDB_IS_GRAFANADB=true -e HTTP_USER=user -e HTTP_PASS=pass tutum/grafana
```
