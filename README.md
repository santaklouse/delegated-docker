delegated-docker
=========

![Docker Pulls](https://img.shields.io/docker/pulls/santaklouse/delegated?style=for-the-badge)

[![PlayWithDocker](https://github.com/play-with-docker/stacks/raw/cff22438cb4195ace27f9b15784bbb497047afa7/assets/images/button.png)](https://labs.play-with-docker.com?stack=https://raw.githubusercontent.com/santaklouse/delegated-docker/main/docker-compose.yml)

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/santaklouse/delegated-docker)

[DeleGate][1] is a multipurpose proxy server which relays various application
protocols on TCP/IP or UDP/IP

https://its-more.jp/delegate/ftp/pub/DeleGate/Manual.htm#SRCIF

## docker-compose.yml

```
version: "3.9"
services:
  delegated:
    image: santaklouse/delegated:latest
    command: "-P1080 SERVER=socks SSLTUNNEL=127.1:4444"
    ports:
      - "1080:1080"
    depends_on:
      i2p:
        condition: service_healthy
    restart: always
  i2p:
    image: santaklouse/i2p:latest
    ports:
      - "4444:4444"
      - "4445:4445"
      - "6668:6668"
      - "7657:7657"
    healthcheck:
      test: [ "CMD", "curl", "-f", "http://127.0.0.1:4444" ]
      interval: 1m30s
      timeout: 5s
      retries: 10
      start_period: 10s
```

## up and running

```
# server
$ docker-compose up -d

# client
$ curl -x socks5h://localhost:1080 ifconfig.ovh
```

[1]: http://www.delegate.org/delegate/
