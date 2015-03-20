# Proxy - Privoxy/Tor

### Proxy Use ###

proxy.gnulug.org
* Tor listening on localhost:9050
* Privoxy listening on localhost:8118

Use the following SSH config to configure the tunnel.
```
Host tunnel
  User jschipp
  HostName proxy.gnulug.org
  IdentityFile ~/.ssh/lug
  DynamicForward localhost:50000
  LocalForward localhost:60000 localhost:8118
```

Set your HTTP proxy to localhost:6000
```
export http_proxy=127.0.0.1:6000
export https_proxy=127.0.0.1:6000
```

You can also forward anything that speaks SOCKS over localhost:50000
