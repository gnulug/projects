# Remote Access

### Accessing Systems Remotely ###

user -> *.gnulug.org
* Command-line access to gnulug servers
* 

Specify private key, username, and server to connect to:
```
ssh -i .ssh/gnulug bob@server.gnulug.org
```

A more convenient method is to add the following to your client ssh config file: `.ssh/config`
```
Host *.gnulug.org
    User bob
    IdentityFile ~/.ssh/gnulug

Host 192.17.239.*
    User bob
    IdentityFile ~/.ssh/gnulug
```
Now you only need to specify the server or IP: `ssh server.gnulug.org`
