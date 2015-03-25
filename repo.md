# Repo

### Repo/Apt-Cacher System ###

repo.gnulug.org
* Custom repository for packages we build
* Caching repository via Apt-Cacher

### Use ###

Client settings for the repo services.

Retrieve packages from our cache:
```
$ cat /etc/apt/apt.conf.d/01proxy
Acquire::http::Proxy "http://repo.gnulug.org:8080";
```

Retrieve packages from our repository:
```
$ cat /etc/apt/sources.list.d/lug.list
deb http://repo.gnulug.org/packages/ amd64/
deb http://repo.gnulug.org/packages/ i386/
```

Add packages to our repository:
```
cp mypackage_amd64.deb /var/www/html/packages/amd64/
cp mypackage_i386.deb /var/www/html/packages/i386/
/var/www/html/packages/update_repo.sh
```
