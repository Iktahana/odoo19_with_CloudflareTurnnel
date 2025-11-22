# Installing Odoo 19 with Cloudflare Tunnel

## Create a Cloudflare Tunnel

At the first, You need to create a Cloudflare Tunnel And note the `token`.

Here's the
document: https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/get-started/create-remote-tunnel/

![cloudflare-0-1.png](screenshots/cloudflare-0-1.png)
![cloudflare-0-2.png](screenshots/cloudflare-0-2.png)
![cloudflare-0-3.png](screenshots/cloudflare-0-3.png)

## Quick Installation

We use Ubuntu 25.10 x64

### Install Docker & Git

Install the Docker and Git to your server.

https://docs.docker.com/engine/install/

``` bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update

# install docker
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# install git
sudo apt install git
```

### Clone code

```bash
cd /opt/
git clone https://github.com/Iktahana/odoo19_with_CloudflareTurnnel.git
mv ./odoo19_with_CloudflareTurnnel odoo
```

### Set configs (.env)

```bash
cd /opt/odoo
cp .env.example .env
nano .env
```

```dotenv
# Using Cloudflare Turnnel is a safe and effective way
CLOUDFLARED_TOKEN=<cloudflare_turnnel_token>

# According to security practices, 
# it is recommended that you mount the hard disk to an external SSD. 
# (Most cloud services provide this service)
PGDB_VOLUME=/mnt/volume_xxx_xx/odoo/postgresql/

# Tips: Use `openssl rand -hex 32` to generate a security password.
POSTGRES_PASSWORD=
```

### Start the Docker Compose

``` sh
docker-compose up -d
```

## Setup tunnel

Finally, set up the rules on the Cloudflare Dashboard.

![cloudflare-1.png](screenshots/cloudflare-1.png)
![cloudflare-2.png](screenshots/cloudflare-2.png)

來源URL是在容器內部，所以你必須填寫`http://nginx`.

保存之後，你就可以使用你的域名在外網訪問odoo了。

![odoo-19-welcome-screenshot.jpg](screenshots/odoo-19-welcome-screenshot.jpg)

enjoy it.
