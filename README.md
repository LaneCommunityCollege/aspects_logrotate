# aspects_logrotate

Create logrotate configuration files in /etc/logrotate.d/.

## Requirements
`logrotate` needs to be installed on your node.

Set ```hash_behaviour=merge``` in your ansible.cfg file.

## Role Variables
### aspects_logrotate_enabled
Enable or disable this role. Default is False/Disabled. Set to True in order to enable the role.

### aspects_logrotate_configs
A dictionary containing explict rules.

Each rule is saved in ```/etc/logrotate.d/```. The file name is the same as the dictionary key. So, ```aspects_logrotate_configs.apache``` would be saved in ```/etc/logrotate.d/apache```.

That means that you can overwrite any rules in ```/etc/logrotate.d``` simply by setting the key to the filename.

### aspects_logrotate_conf_file
A dictionary of values to place in the ```/etc/logrotate.conf``` file.

Dictionary keys should be prefixed with a number to allow proper sorting.

Each item can have a per ```ansible_os_family``` override. For example, if you want to use the dateext option on CentOS 7 but not on Ubuntu, you can do something like:

```
aspects_logrotate_conf_file:
  004dateext:
    enabled: True
    value: ""
    Debian:
      enabled: False
    RedHat:
      enabled: True
      value: "dateext"
```

If there is no ```ansible_os_family``` override, the default ```aspects_logrotate_conf_file.<key>.value``` will be used.

See defaults/main.yml for examples and the defaults set. The defaults were created by comparing CentOS 7, Ubuntu Trusty and Xenial, and Debian Stretch Vagrant vm's default ```/etc/logroate.conf``` files.

The Vagrantfile had vm.box set to bento/ubuntu-16.04, ubuntu/trusty64, centos/7, and debian/stretch64.

# Example Playbook
Not that it's a good idea to overwrite the default syslog configuration, but it makes a good example.

    - hosts: servers
      roles:
         - aspects_logrotate
      vars:
        aspects_logrotate_enabled: True
        aspects_logrotate_configs:
          syslog: |
            /var/log/syslog
            {
                rotate 7
                daily
                missingok
                notifempty
                delaycompress
                compress
                postrotate
                    reload rsyslog >/dev/null 2>&1 || true
                endscript
            }

# License

MIT
