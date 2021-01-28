# ansible-role-opencanary

Installs [thinkst/opencanary](https://github.com/thinkst/opencanary).

## Role Variables

| Variable                           | Default | Comments                                        |
| :--------------------------------- | :------ | :---------------------------------------------- |
| opencanary_path                    | /opt/opencanary | The path to install opencanary |
| opencanary_systemd_path            | /etc/systemd/system | The path for the systemd service file |
| opencanary_user                    | opencanary | The user to run opencanary as |
| opencanary_group                   | opencanary | The group to run opencanary as |
| opencanary_pid                     | /opt/opencanary/opencanary.pid | The path to the pid |
| opencanary_log_path                | /var/log/opencanary | Where to store the opencanary logs |
| opencanary_logrotate_path          | /etc/logrotate.d | Where the logrotate configuration should be saved |
| opencanary_systemd_path            | /etc/systemd/system | Path to the systemd definitions |
| opencanary_loggers                 | [Logging](#Logging) | Loggers |
| opencanary_config                  | {} | List of configuration parameters |

### opencanary Configuration

The [opencanary.conf](/thinkst/opencanary/docs/starting/configuration.rst) has information on the general options.

This file has been translated as the defaults, with every thing turned off. You can turn on the individual honeypots
by using their json name as a yaml key. This allows for variable substitution when needed

```yaml
opencanary_config:
  rdp.enabled: true
```

### Logging

| Name | Default | Description |
| :--- | :------ | :---------- |
| file | {{ opencanary_ log_path }}/opencanary.log | Path to the log file |
| console | ext://sys.stdout | Log to stdout |
| slack | | The webhook URL |
| smtp | | SMTP configuration |

#### SMTP configuration
```yaml
opencanary_loggers:
  smtp:
    host: smtp.example.com
    port: 25
    from: noreply@example.com
    to: admin@example.com
    subject: Canary Alert!
```

If you need to handle authentication to port 587, add the following:
```yaml
  port: 587
  credentials:
    username: example
    password: example
```

## Testing

Testing requires Molecule and Docker

```
pipenv install -r molecule/requirements.txt
pipenv run molecule test
```

## License

MIT