# Blog

## frp

### frps

#### frps.toml

```toml
# frps.toml

[common]
bind_port = 7000
token = "Your_Token"

dashboard_port = 7500
dashboard_user = "admin"
dashboard_pwd = "admin"

vhost_http_port = 8080
vhost_https_port = 8443

log_file = "./frps.log"
log_level = "info"
log_max_days = 7
```

#### frps.service

```ini
# frps.service

[Unit]
Description = frp server
After = network.target syslog.target
Wants = network.target

[Service]
Type = simple
ExecStart = /usr/local/bin/frps -c /etc/frp/frps.toml

[Install]
WantedBy = multi-user.target
```

### frpc

#### frpc.toml

```toml
# frpc.toml

[common]
server_addr = "Your_Server_Address"
server_port = 7000
token = "Your_Token"

[[proxies]]
name = "ssh"
type = "tcp"
local_ip = "127.0.0.1"
local_port = 22
remote_port = 8022

[[proxies]]
name = "web"
type = "tcp"
local_ip = "127.0.0.1"
local_port = 8080
remote_port = 8080

[[proxies]]
name = "mysql"
type = "tcp"
local_ip = "127.0.0.1"
local_port = 3306
remote_port = 8306

[[proxies]]
name = "web_http"
type = "http"
local_ip = "127.0.0.1"
local_port = 8080
custom_domains = ["Your_Domain"]

[[proxies]]
name = "web_https"
type = "https"
local_ip = "127.0.0.1"
local_port = 8443
custom_domains = ["Your_Domain"]

[[proxies]]
name = "dns"
type = "udp"
local_ip = "127.0.0.1"
local_port = 53
remote_port = 8053
```

#### frpc.service

```ini
# frpc.service

[Unit]
Description = frp client
After = network.target syslog.target
Wants = network.target

[Service]
Type = simple
ExecStart = /usr/local/bin/frpc -c /etc/frp/frpc.toml

[Install]
WantedBy = multi-user.target
```

### configuration

#### frps

`cd frp`

`sudo mkdir /etc/frp`

`sudo cp frps /usr/local/bin/`

`sudo cp frps.toml /etc/frp/`

`sudo cp frps.service /etc/systemd/system/`

`sudo systemctl daemon-reload`

`sudo systemctl enable frps`

`sudo systemctl start frps`

#### frpc

`cd frp`

`sudo mkdir /etc/frp`

`sudo cp frpc /usr/local/bin/`

`sudo cp frpc.toml /etc/frp/`

`sudo cp frpc.service /etc/systemd/system/`

`sudo systemctl daemon-reload`

`sudo systemctl enable frpc`

`sudo systemctl start frpc`
