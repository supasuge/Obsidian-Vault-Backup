## Table of Contents

- [Limitations/Requirements](#limitations/requirements)
          - [Clone Repo to server](#Clone\Repo\to\server)
  - [Setting up NGINX and the reverse Proxy](#Setting\up\NGINX\and\the\reverse\Proxy)
- [Allow SSH, HTTP, HTTPS through the firewall:](#allow\ssh,\http,\https\through\the\firewall:)
- [Enable the firewall](#enable\the\firewall)
    - [Setting up HTTPS (SSL/TLS) Certificates](#Setting\up\HTTPS\(SSL/TLS)\Certificates)

# Limitations/Requirements
- Rate limiting requests on the server
- A firewall to only allow connections on some ports
- SSL Certificates (HTTPS) for the server
- Logging Requests correctly to trace activity

- separating each system component in docker containers use the `docker-compose.yml` file's present on the CTFd repo to conveniently deploy and migrate if needed.
[Install Docker](https://docs.docker.com/engine/install/)
```bash
sudo apt-get remove docker docker-engine docker.io containerd runc

sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose
```
###### Clone Repo to server
```bash
git clone https://github.com/...../ctfd

cd CTFd
```
To setup CTFTime authentication add the following environment variables in the `docker-compose.yml` file:
- `SECRET_KEY`
- `OAUTH_PROVIDER`
- `OAUTH_CALLBACK`
- `OAUTH_CLIENT_ID`
- `OAUTH_CLIENT_SECRET`

## Setting up NGINX and the reverse Proxy
```bash
sudo apt update
sudo apt upgrade -y 
sudo apt install nginx ufw
# Allow SSH, HTTP, HTTPS through the firewall:
sudo ufw allow 'Nginx Full'
sudo ufw allow 'OpenSSH'
# Enable the firewall
sudo ufw enable
```
- Now, visiting your domain should show the default Nginx page, let's reconfigure it to show CTFd instead:
Create a file at: 
`/etc/nginx/site-available/DOMAINHERE.com` (replace domain)
```bash
limit_req_zone  $binary_remote_addr zone=mylimit:10m rate=10r/s;
limit_conn_zone $binary_remote_addr zone=addr:10m;
server {
	server_name mydomain.com;
	limit_req zone=mylimit burst=15;
	limit_conn addr 10;
	limit_req_status 429;
	client_max_body_size 8M;
	location / {
    		proxy_pass http://localhost:8000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
  }
}
```
- This sets up rate-limiting at 10 requests per second, and a max of 10 sumltaneous connections per IP address at a time, and also tells Nginx to route requests to `DOMAINHERE.com` at port 8000

Create a symlink to the file that was previously created in `/etc/nginx/sites-enabled` and reload nginx

```bash
sudo ln -s /etc/nginx/sites-available/DOMAINHERE.com /etc/nginx/sites-enabled/DOMAINHERE.com

sudo nginx -s reload
```

### Setting up HTTPS (SSL/TLS) Certificates
```bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get innstall software-properties-common
sudo add-apt-repository universe
sudo apt-get update
sudo apt-get install certbot python3-certbot-nginx
```
- Once Installed run:
```bash
sudo certbot --nginx
```
- This will prompt you to pick which domains you want to setup nginx for. Select the domain we setup with nginx before for CTFd, and follow the series of questions certbot asks. Make sure you tell `certbot` to **always redirect to HTTPS**, as we don't want anyone accessing the website over HTTP.