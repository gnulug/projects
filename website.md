# Website

### Editing the Website ###

www.gnulug.org -> gnulug.org
* Webserver for official gnulug website
* Jekyll generates static HTML which is deploy to the server

#### Setup ####

1. Install Jekyll on your workstation
```
gem install jekyll
gem install last-modified-at
```

2. Configure [Remote Access](remote_access.md) for ssh to www.gnulug.org
```
$ cat ~/.ssh/config
Host *.gnulug.org
User bob
IdentityFile ~/.ssh/gnulug
```

Run the deploy script to put the changes into production
```
git clone https://github.com/ACMLug/jekyll-website
# edits
./deploy
```

Write access to ACMLug/website repository is limited to Owners and Core groups.
