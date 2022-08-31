# New Users

## Adding New Users
1. Create user account on VMs
2. Configure SSH Key for Access to VMs

**Note:** Verify the new puppet configuration in Vagrant before comitting it!

### New User Accounts

Creating a new Linux user account will give you access to all our VMs. This is done by cron each hour.

```
git clone git@git.gnulug.org:lug/puppet.git
cd puppet
vim modules/common/manifests/users.pp
```

1. Create a class for the new user
2. Increment the UID by 1 from the previous highest UID

```
class users::lug::bob {
  users::lug { "bob":
    fullname => "Bob Sanders",
    uid => 2005,
    extra_groups => [ 'lug' ],
    }
  }
```

Other possible options:

3. Set an optional password using the crypt() format, result of: `mkpasswd -m sha-512`
4. Decide whether user needs sudo access
5. Set shell to something other than bash

```
class users::lug::bob {
  users::lug { "bob":
    ...
    password     => '$6$ffjsjzsdfadw$ujz0asdaxwllsdfdf8vvaqedtnn6utrpoq5.vyuk9diyxk0q/lv9rx7if0turmw21ubt0x6wspbfwkgtfsq/e1.8',
    extra_groups => [ 'sudo', 'lug' ],
    shell        => '/bin/zsh',
    }
  }
```

Now add new user class to lug members to apply it
```
class users::lug::members {
  ...
  include users::lug::bob
}
```

An admin can apply the configuration immediately on the host via
```
cd /etc/puppet && git pull && ./puppet_apply.sh
```

### SSH Key Configuration

SSH keys are used to access our VMs which provides better security than passwords.
Each new user will need to generate a key pair and add the key to puppet so it is configured on all the VMs.

```
git clone git@git.gnulug.org:lug/puppet.git
cd puppet
```

Place the public key in the authorized_keys directory with the filename set to the name of the new user
```
vim modules/ssh/files/authorized_keys/bob
```

Open `modules/ssh/manifests/client.pp` and add the new user to the users array
```
$users = [ .., 'bob' ]
```

Finally, add the user to the list of AllowUsers and then commit changes and push.
```
$ grep AllowUsers modules/ssh/templates/sshd_config.erb
AllowUsers alice bob
```

An admin can apply the configuration immediately on the host via
```
cd /etc/puppet && git pull && ./puppet_apply.sh
```
