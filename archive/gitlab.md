# Gitlab

### Gitlab ###

gitlab.gnulug.org -> git.gnulug.org
* Hosting LUG git repositories that require confidentiality
* Personal repositories for members

### Use ###

1. Request account from admins
2. Log in to Gitlab WUI and set password
3. Add public key to profile `https://git.gnulug.org/profile -> SSH Keys`

#### SSH ####

Add this to ~/.ssh/config on your workstation, replacing `$USER` and `$KEY`
with your username and key

```
Host git.gnulug.org
  User $USER
  IdentityFile ~/.ssh/$KEY
```

Clone repo of choice:
```
git clone git@git.gnulug.org:lug/puppet.git
```

#### HTTPS ####

Disable SSL certificate verfication (self-signed) through one of the following:
```
# Global
git config --global http.sslVerify false
# Local - per repo
git config http.sslVerify "false"
```

Clone repo of choice:
```
git clone https://git.gnulug.org/lug/puppet.git
```

Authenticate with your username and password.
