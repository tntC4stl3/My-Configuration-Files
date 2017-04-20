Grafana
=======

The open platform for beautiful analytics and monitoring.

* [Grafana official](https://grafana.com/)  
* [Grafana github](https://github.com/grafana/grafana)
* [Grafana Docker](https://github.com/grafana/grafana-docker)

## Run with custom configuration

```
docker run -d --name=grafana -p 3000:3000 -v /grafana:/etc/grafana grafana/grafana
```

## Enable LDAP authentication

1. Turn on the ldap in main configuration `/etc/grafana/grafana.ini`:
```
[auth.ldap]
enabled = true
config_file = /etc/grafana/ldap.toml
allow_sign_up = true
```
The `allow_sign_up` means if a valid ldap user try to login Grafana, and there is no account for him on Grafana side, Grafana will generate one for him automatically.

2. Change the configuration in `/etc/grafana/ldap.toml` to fit your own requirement.
