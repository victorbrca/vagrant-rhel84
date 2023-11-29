Vagrant repo for creating a generic rhel84 VM on VirtualBox.

### Box Info

+ **Vagrant Cloud:** [victorbrca/rhel84](https://app.vagrantup.com/victorbrca/boxes/rhel84/)
+ **Vagrant Box Version:** 0.1.0
+ **OS:** RHEL
  + **Release:** 8.4
  + **Kernel:** 4.18.0-305.25.1.el8_4.x86_64
+ **Storage Devices**
  + IDE Controller (IDE)
    + VMDK 128G Dynamic
  + Sata Controller (AHCI)
+ **Guest Additions:** 7.0.12

### Packages Installed

+ ansible 2.9
+ Development Tools

### Built-in credentials

+ **Shell**: vagrant / vagrant

### SSHD Access

    PasswordAuthentication yes
    PermitRootLogin yes

### Setup

You can easily start the box with:

```bash
vagrant init victorbrca/rhel84
vagrant up
```

Or by manually adding to a Vagrantfile:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "victorbrca/rhel84"
end
```

### Example Vagrantfile

```ruby
# Default credentials
#   Shell: vagrant / vagrant

VAGRANTFILE_API_VERSION = "2"

##-- Global config -------------------------------------------------------------
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Don't update the sshkey
  config.ssh.insert_key = false

  # Don't update guest additions
  config.vm.box_check_update = false
  config.vbguest.auto_update = false

  # Private IP Address (eth1)
  # private_ip =

  ##-- Machine -----------------------------------------------------------------
  config.vm.define "rhel84" do |rhel84|
    rhel84.vm.box = "victorbrca/rhel84"
    # rhel84.vm.network "private_network", ip: private_ip

    # Set the synced folder
    rhel84.vm.synced_folder ".", "/vagrant", type: "virtualbox"

    ##-- Provider --------------------------------------------------------------
    rhel84.vm.provider "virtualbox" do |vb|
      # Use internal DNS from host
      vb.customize [ "modifyvm", :id, "--natdnshostresolver1", "on" ]
      vb.name = "rhel84"
      vb.memory = "1024"
    end

  end
end
```