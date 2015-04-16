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

You can use a PAC file in Firefox to view our non-public web interfaces:
```
// Configuring Firefox to Use a PAC File
//
// 1. In Firefox, click on Tools > Options > Advanced > Network.
// 2. Click Settings > Select Automatic Proxy Configuration URL.
// 3. Type the path and filename of your PAC file. e.g. http://<url>/proxy.pac
// 4. Click Reload, and then click OK twice.

function FindProxyForURL(url, host) {
    host = host.toLowerCase();
    if (
        dnsDomainIs(host, "gnulug.org")
     || dnsDomainIs(host, "open-nsm.net")
     || shExpMatch(host, "192.17.239.*")
    )
        return "SOCKS 127.0.0.1:50000"; // (IP:port)

    return "DIRECT";
}
```

