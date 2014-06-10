#aspects_logrotate

Create logrotate configuration files in /etc/logrotate.d/.

##Requirements

Set ```hash_behaviour=merge``` in your ansible.cfg file.

##Role Variables
###aspects_logrotate_configs
A dictionary containing explict rules.

#Example Playbook
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

#License

MIT