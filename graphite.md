# Graphite

### Collectd/Graphite System ###

graphs.gnulug.org
* Collectd daemon for system stats
* Graphite for graphing stats

Web interface
```
http://graphs.gnulug.org
```

Send collectd data from clients
```
$ cat /etc/collectd/collectd.conf.d/lug.conf
<Plugin "network">
        Server "192.17.239.15" "25826"
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
