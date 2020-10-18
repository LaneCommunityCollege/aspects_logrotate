# aspects_logrotate
Manage logrotate configuration in `/etc/logrotate.conf` and `/etc/logrotate.d`.

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

See [defaults/main.yml](defaults/main.yml) for examples. You can set a default value, or per
distro and major version.

# Example Playbook
Not that it's a good idea to overwrite the default syslog configuration, but it makes a good example.
```yaml
- hosts: servers
  roles:
     - aspects_logrotate
  vars:
    aspects_logrotate_enabled: True
    aspects_logrotate_conf_file:  
      001suusergroup:
        enabled: True
        Ubuntu:
          1604: "su root syslog"
          1804: "su root syslog"
          2004: "su root adm"
        CentOS:
          7: "su root loggroup"
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
```

# License

MIT
