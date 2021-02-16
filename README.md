# Traefik Local

Setup a local Traefik web proxy with DNS resolution for \*.chatbot-v2.wogra.test domains.

Also setup a local trusted Root CA and create a TLS certificate for using https in local (shout out to [mkcert](https://github.com/FiloSottile/mkcert)).

## 0. Prerequisites

- [Docker](https://docs.docker.com/docker-for-mac/install/)

## 1. Setup resolvers

```sh
# Setup Linux to take into account our local docker resolver
echo "127.0.0.1 traefik.chatbot-v2.wogra.test" | sudo tee -a /etc/hosts > /dev/null
echo "127.0.0.1 local.chatbot-v2.wogra.test" | sudo tee -a /etc/hosts > /dev/null
```

## 2. Setup a local Root CA

```sh
pacman -S nss # only if you use Firefox
pacman -S mkcert

# Setup the local Root CA
mkcert -install

# Local Root CA files are located under ~/Library/Application\ Support/mkcert
# Look at https://github.com/FiloSottile/mkcert is you need instructions to install them on another device
```

## 3. Setup a Traefik container w/ https

```sh
# Clone this repository
git clone https://github.com/KimToehler/traefik-local.git
cd traefik-local/

# Create a local TLS certificate
mkcert -cert-file certs/local.crt -key-file certs/local.key "chatbot-v2.wogra.test" "*.chatbot-v2.wogra.test"

# Start Traefik
docker-compose pull
docker-compose up -d

# Go on https://traefik.chatbot-v2.wogra.test you should have the traefik web dashboard serve over https
```
