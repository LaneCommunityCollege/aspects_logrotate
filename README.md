# aspects_logrotate

Create logrotate configuration files in /etc/logrotate.d/.

## Requirements

Set ```hash_behaviour=merge``` in your ansible.cfg file.

## Role Variables
### aspects_logrotate_enabled
Enable or disable this role. Default is False/Disabled. Set to True in order to enable the role.

### aspects_logrotate_configs
A dictionary containing explict rules.

Each rule is saved in ```/etc/logrotate.d/```. The file name is the same as the dictionary key. So, ```aspects_logrotate_configs.apache``` would be saved in ```/etc/logrotate.d/apache```.

That means that you can overwrite any rules in ```/etc/logrotate.d``` simply by setting the key to the filename.

# Example Playbook
Not that it's a good idea to overwrite the default syslog configuration, but it makes a good example.

    - hosts: servers
      roles:
         - aspects_logrotate
      vars:
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
