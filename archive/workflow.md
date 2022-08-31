# Workflow

##Virtual Machines: Development to Production Workflow###

1. Build development VM with Vagrant
2. Create Puppet configuration for VM
3. Verification and review
4. Deploy to production [Xen Project System](xen.md)

###Development: Build VMs with Vagrant###

Our use of Vagrant serves two purposes:
* Test and verify a proper vm configuration before deployment
* Provide resources to utilize a Linux network locally as a test bed (vm labs are hard to come by)

```
git clone https://github.com/ACMLug/vagrant
```

Create new project folder
```
./new test
cd test
```

Edit the Vagrantfile if you need a custom config e.g. port forwarding
```
vim Vagrantfile
```

Bring up and connect to the VM
```
vagrant up
vagrant ssh
```

1. Build your system with the necessary tools and services
2. Automate the build by editing the provision script
3. Commit the new Vagrant project back to the repository

You can run the provisioner manually while the VM is running
```
vagrant provision
```

###Puppet Configuration###

Here we will convert the provisioner script to a Puppet configuration.
We use the puppet configuration to deploy configurations on our Xen host.
```
git clone git@git.gnulug.org:lug/puppet.git
cd puppet
```

Bring up the system (`test.gnulug.org`) with the base configuration from Puppet.
The puppet provisioner is configured in Vagrant so it will apply the `test.gnulug.org` configuration
to the virtual machine. The modules Puppet will apply are listed in `manifests/site.pp`
```
vagrant up
```

Once the base configuration has been applied create a new puppet configuration
to provision the host to your needs as specified in the Vagrant development environment.

New Puppet modules can be created by the helper script
```
./new_module <modulename>
```

Once you are finished editing the puppet configuration, verify it by applying it to the
Vagrant running box. You can use another helper script to perform this task:
```
./vagrant_provision_puppet.sh
```

Finally, after the puppet configuration is applied to the VM without error, verify that all the
services work as planned. Commit the results to the repository. Once received and reviewed the Xen
Project host administrator will build the virtual machine and the puppet configuration will be applied
automatically. You can than utilize the resource as intended by using ssh.
