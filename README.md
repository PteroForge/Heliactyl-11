# Heliactyl 11

Tired of dashactyl? Why not try Heliactyl
Features on top of dashactyl:
- Join for Resources
- Anti Discord Ratelimit
- Gift Resources
- Improved API
- Corona Admin Theme

# Support

Discord Server: https://dc.heliactyl.xyz

# How to install

1. Upload the file above onto a Pterodactyl NodeJS server [Download the egg from Parkervcp's GitHub Repository](https://github.com/parkervcp/eggs/tree/master/bots/discord/discord.js)
2. Unarchive the file and set the server to use NodeJS 12
3. Configure settings.json (specifically panel domain/apikey and discord auth settings for it to work)
4. Start the server (Ignore the 2 strange errors that might come up)

# Configure reverse proxy

1. Login to your DNS manager, point the domain you want your dashboard to be hosted on to your VPS's Ip address. (Example: dash.witchly.host 192.168.0.1)
2. Run `apt install nginx && apt install certbot` on the vps
3. Run `ufw allow 80` and `ufw allow 443` on the vps
4. Run `nano /etc/nginx/sites-enabled/heliactyl.conf`
5. Paste the configuration at the bottom of this and replace <IP> with the IP of the pterodactyl server including the port and <domain> with the domain you want your dashboard to be hosted on.
6. Run `certbot certonly -d <DOMAIN>` then do 1 and put your email
7. Run `systemctl restart nginx` and try open your domain
```
server {
listen 80;
server_name <DOMAIN>;
return 301 https://$host$request_uri;
}
server {
listen 443;
server_name <DOMAIN> ;
ssl_certificate /etc/letsencrypt/live/<DOMAIN>/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/<DOMAIN>/privkey.pem;
ssl on;
ssl_session_cache builtin:1000 shared:SSL:10m;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_ciphers HIGH:!aNULL:!eNULL:!EXPORT:!CAMELLIA:!DES:!MD5:!PSK:!RC4;
ssl_prefer_server_ciphers on;
access_log /var/log/nginx/access.log;
location /afkwspath {
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "upgrade";
  proxy_pass "http://<IP>/afkwspath";
}
location / {
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_pass http://<IP>;
proxy_read_timeout 90;
proxy_redirect http://<IP> https://<DOMAIN>;
  }
}
