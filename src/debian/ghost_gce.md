# Debian Based

## Configure a Ghost Blog at GCE

### Node + Yarn + Requisites

DBUS and SETCAP are not installed by default on GCE Debian

```sh
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -

echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo apt-get update && sudo apt-get install -y yarn nodejs dbus libcap2-bin wget
```

#### Update Practices

```sh
sudo apt update && sudo apt upgrade
```
weekly


### Ghost Engine

```sh
echo "PATH=$PATH:$HOME/.yarn/bin" >> ~/.bashrc
source ~/.bashrc
yarn global add ghost-cli@latest
sudo rm -rf /var/www/ghost
sudo mkdir -p /var/www/ghost
sudo chown $(whoami): /var/www/ghost

ghost install --url=https://MYSITE.COM --admin-url=https://ADMIN.MYSITE.COM --db=sqlite3 --mail=SMTP --mailservice=Mailgun --mailuser=postmaster@MYSITE.COM --mailpass=MAILGUN_SMTP_PW --no-stack --no-setup-ssl --no-prompt -d /var/www/ghost
```

#### Update Practices

```sh
ghost update
```
whenever a new version comes

### Caddy Webserver

Auto HTTPS using Let's Encrypt

#### Caddyfile

First we need to create a `Caddyfile`, that is a configuration file for Caddy HTTPS Server. `nano Caddyfile` or vim Caddyfile and paste this:

```
MYSITE.COM {
  status 404 /ghost
  proxy / http://127.0.0.1:2368 {
    transparent
    fail_timeout 300s
    header_upstream X-Forwarded-Ssl on
  }

  tls ADMINEMAIL@MYSITE.COM
  gzip
}

ADMIN.MYSITE.COM {
  status 404 //
  proxy / http://127.0.0.1:2368/ {
    transparent
    fail_timeout 300s
    header_upstream X-Forwarded-Ssl on
  }
  tls ADMINEMAIL@MYSITE.COM
  gzip
}

www.MYSITE.COM {
  redir https://MYSITE.COM
  tls ADMINEMAIL@MYSITE.COM
}
```

Obviously modify to your site settings.

I separated the admin site for hardening access. It has stricter rules at Cloudflare Edge. The ADMIN is some long string (kind of phrase password). It can make life harder for bots. Plus with stricter rules at my Cloudflare Edge for these bots (for instance, mandatory Captcha thoughter rate limits), it will not eat Network Fees from Google Cloud (or whatever you are using).

#### Installation

Now we will install and start the service:

```sh
curl https://getcaddy.com | bash -s personal
sudo setcap 'cap_net_bind_service=+ep' /usr/local/bin/caddy
sudo mkdir /etc/caddy
sudo chown -R root:www-data /etc/caddy
sudo mkdir /etc/ssl/caddy
sudo chown -R root:www-data /etc/ssl/caddy
sudo chmod 0770 /etc/ssl/caddy
sudo cp ~/Caddyfile /etc/caddy/
sudo chown www-data:www-data /etc/caddy/Caddyfile
sudo chmod 444 /etc/caddy/Caddyfile
sudo chown www-data:www-data /var/www
sudo chmod 555 /var/www
wget https://raw.githubusercontent.com/mholt/caddy/master/dist/init/linux-systemd/caddy.service
sudo cp caddy.service /etc/systemd/system/
sudo chown root:root /etc/systemd/system/caddy.service
sudo chmod 644 /etc/systemd/system/caddy.service
sudo systemctl daemon-reload
sudo systemctl start caddy.service
sudo systemctl enable caddy.service
```

#### Update practices

Whenever a new caddy version comes, or weekly:

```sh
curl https://getcaddy.com | bash -s personal
sudo pkill -USR2 caddy
```
Automating this is out of this doc scope (tip: CRON)

### Logs

To follow connection logs on Caddy:

```sh
journalctl -f -u caddy.service
```

Or requests at node server:

```sh
journalctl -f -u ghost_MYSITE.service
```