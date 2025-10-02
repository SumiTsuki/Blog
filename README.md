# Blog

## frp

### frps

#### frps.toml

```toml
# frps.toml

bindPort = 7000

[auth]
method = "token"
token = "your_secure_token_here"
allowPorts = [
        { single = 8022 },
        { single = 8053 },
        { single = 8080 },
        { single = 8306 },
        { single = 8443 }
]

[webServer]
addr = "0.0.0.0"
port = 7500
user = "admin"
password = "your_admin_password"

[log]
to = "./frps.log"
level = "info"
maxDays = 7
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
[common]
serverAddr = "your_server_ip"
serverPort = 7000
auth.token = "your_secure_token_here"

[[proxies]]
name = "ssh"
type = "tcp"
localIP = "127.0.0.1"
localPort = 22
remotePort = 8022

[[proxies]]
name = "web"
type = "tcp"
localIP = "127.0.0.1"
localPort = 8080
remotePort = 8080

[[proxies]]
name = "mysql"
type = "tcp"
localIP = "127.0.0.1"
localPort = 3306
remotePort = 8306

[[proxies]]
name = "web_http"
type = "http"
localIP = "127.0.0.1"
localPort = 8080
customDomains = ["your-domain.com"]

[[proxies]]
name = "web_https"
type = "https"
localIP = "127.0.0.1"
localPort = 8443
customDomains = ["your-domain.com"]

[[proxies]]
name = "dns"
type = "udp"
localIP = "127.0.0.1"
localPort = 53
remotePort = 8053
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

`sudo cp frps /usr/local/bin/`

`sudo cp frps.toml /etc/frp/`

`sudo cp frps.service /etc/systemd/system/`

`sudo systemctl daemon-reload`

`sudo systemctl enable frps`

`sudo systemctl start frps`

#### frpc

`cd frp`

`sudo cp frpc /usr/local/bin/`

`sudo cp frpc.toml /etc/frp/`

`sudo cp frpc.service /etc/systemd/system/`

`sudo systemctl daemon-reload`

`sudo systemctl enable frpc`

`sudo systemctl start frpc`
