<p align="center">  
    <img src="https://user-images.githubusercontent.com/76937659/153705486-44e6c1b2-74fa-4d44-be1c-36c8fdb83331.gif"/>  
  </p>  
<p align="center">
<img src="https://readme-typing-svg.herokuapp.com?color=%2336BCF7&center=true&vCenter=true&lines=FN+PROJECT+Autoscript" />
</p>
<p align="center">  
    <img src="https://user-images.githubusercontent.com/76937659/153705486-44e6c1b2-74fa-4d44-be1c-36c8fdb83331.gif"/>  
  </p>  

![Rerechan02 card name](https://cardivo.vercel.app/api?name=Rerechan02『𝐅𝐍』&description=Hi,%20everyone!%20and%20Nice%20to%20meet%20you%20%F0%9F%91%8B&image=https://raw.githubusercontent.com/Rerechan02/simple-xray/main/funny1.jpg?v=4&backgroundColor=%23ecf0f1&telegram=/&github=Rerechan02&pattern=leaf&colorPattern=%23eaeaea)

# NOTES
`this is example fultiport with sslh for linux server or tunneling by FN Project`

# Haproxy:
```
global
       tune.ssl.default-dh-param 2048
       tune.h2.initial-window-size 2147483647

defaults
    log global
    mode tcp
    option tcplog
    option forwardfor
    timeout connect 5000
    timeout client 24h
    timeout server 24h
    errorfile 400 /etc/haproxy/errors/400.http
    errorfile 403 /etc/haproxy/errors/403.http
    errorfile 408 /etc/haproxy/errors/408.http
    errorfile 500 /etc/haproxy/errors/500.http
    errorfile 502 /etc/haproxy/errors/502.http
    errorfile 503 /etc/haproxy/errors/503.http
    errorfile 504 /etc/haproxy/errors/504.http

frontend ssh-ssl
    bind *:777 ssl crt /etc/xray/funny.pem
    mode tcp
    option tcplog
    use_backend openresty_http if HTTP
    default_backend ssh-backend

backend ssh-backend
    mode tcp
    option tcplog
    server ssh-server 127.0.0.1:22

backend openresty_http
    mode http
    server http1 127.0.0.1:18020 send-proxy check
```

# SSLH
```
RUN=yes
DAEMON=/usr/sbin/sslh
DAEMON_OPTS="--listen 0.0.0.0:443 --tls 127.0.0.1:777 --ssh 127.0.0.1:3303 --anyprot 127.0.0.1:443 --pidfile /var/run/sslh/sslh.pid -n"
```
# Nginx
```
server {
             listen 18020 proxy_protocol so_keepalive=on reuseport;
             listen [::]:18020 proxy_protocol so_keepalive=on reuseport;
    ssl_certificate /path/to/cert.crt;
    ssl_certificate_key /path/to/cert.key;
    ssl_ciphers EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+ECDSA+AES128:EECDH+aRSA+AES128:RSA+AES128:EECDH+ECDSA+AES256:EECDH+aRSA+AES256:RSA+AES256:EECDH+ECDSA+3DES:EECDH+aRSA+3DES:RSA+3DES:!MD5;
    ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
        server_name 127.0.0.1 localhost;
        root /var/www/html;

location / {
if ($http_upgrade != "Upgrade") {
rewrite /(.*) / break;
}
proxy_redirect off;
proxy_pass http://127.0.0.1:700;
proxy_http_version 1.1;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
proxy_set_header Host $http_host;
}
```

# Detail
```
Server akan menerima koneksi pada port 443 dalam banyak protokol sekaligus seperti websocket, openssh dan ssl/sni/stunnel/tls dalam 1 port.
Server akan membaca koneksi yang diminta jika stunnel maka akan diteruskan kedalam haproxy lalu akan dibaca jika koneksi menuju sni maka 
``
