# Graphite

### Collectd/Graphite System ###

graphs.gnulug.org
* Collectd daemon for system stats
* Graphite for graphing stats

### Examples ###

![Graphite Screenshot](http://jonschipp.com/lug/graphite.png)

### Use ###

Web interface:
```
http://grapite.gnulug.org
```

Send collectd data from clients:
```
$ cat /etc/collectd/collectd.conf.d/lug.conf
<Plugin "network">
        Server "collectd.gnulug.org" "25826"
        Server "influxdb.gnulug.org" "25826"
        </Plugin>

        LoadPlugin syslog
        LoadPlugin battery
        LoadPlugin cgroups
        LoadPlugin conntrack
        LoadPlugin contextswitch
        LoadPlugin cpu
        LoadPlugin cpufreq
        LoadPlugin df
        LoadPlugin disk
        LoadPlugin entropy
        LoadPlugin ethstat
        LoadPlugin exec
        LoadPlugin filecount
        LoadPlugin interface
        LoadPlugin iptables
        LoadPlugin irq
        LoadPlugin load
        LoadPlugin lvm
        LoadPlugin memory
        LoadPlugin netlink
        LoadPlugin network
        LoadPlugin processes
        LoadPlugin protocols
        LoadPlugin swap
        LoadPlugin tcpconns
        LoadPlugin unixsock
        LoadPlugin uptime
        LoadPlugin users
        LoadPlugin vmem
$ service collectd restart
```

Collectd server configuration:
```
LoadPlugin write_graphite

<Plugin "network">
        Listen "0.0.0.0" "25826"
</Plugin>

<Plugin write_graphite>
        <Node "collectd">
                Host "localhost"
                Port "2003"
                Protocol "tcp"
                LogSendErrors true
                StoreRates true
                AlwaysAppendDS false
                EscapeCharacter "_"
        </Node>
</Plugin>
```
