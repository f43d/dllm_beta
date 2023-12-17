---
title: Deploy n8n with Nginx using Portainer
date: 2023-12-07
description: "利用 Portainer 配合 Nginx Proxy Manger 搭建 n8n"
categories: Site部大開發
weight: 0
tags:
  - Oracle
  - n8n
  - Nginx
  - Portainer
cover:
  hidden: false
  relative: false
showToc: false
TocOpen: false
hidemeta: false
comments: false
disableHLJS: false
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: false
ShowBreadCrumbs: false
ShowPostNavLinks: false
ShowWordCount: false
ShowRssButtonInSectionTermList: false
UseHugoToc: false
---

隨着 [OmniFocus4 推出](https://www.omnigroup.com/blog/introducing-omnifocus-4)開始打算 review 一下之前在 [IFTTT](https://ifttt.com/)上建立的 Applet。一段時間沒有用，現在我們這些窮L免費用戶[只能用兩 Applet](https://ifttt.com/plans)。既然而既然如此，於是打算一不做二不休自己搭建[n8n](https://n8n.io/)。

由於 n8n 的[官方 docker compose](https://docs.n8n.io/hosting/installation/server-setups/docker-compose/) 是使用 [Traefik](https://traefik.io/traefik/)。大家看，非常複雜哩。

'
version: "3.7"

services:
  traefik:
    image: "traefik"
    restart: always
    command:
      - "--api=true"
      - "--api.insecure=true"
      - "--providers.docker=true"
      - "--providers.docker.exposedbydefault=false"
      - "--entrypoints.web.address=:80"
      - "--entrypoints.web.http.redirections.entryPoint.to=websecure"
      - "--entrypoints.web.http.redirections.entrypoint.scheme=https"
      - "--entrypoints.websecure.address=:443"
      - "--certificatesresolvers.mytlschallenge.acme.tlschallenge=true"
      - "--certificatesresolvers.mytlschallenge.acme.email=${SSL_EMAIL}"
      - "--certificatesresolvers.mytlschallenge.acme.storage=/letsencrypt/acme.json"
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - traefik_data:/letsencrypt
      - /var/run/docker.sock:/var/run/docker.sock:ro

  n8n:
    image: docker.n8n.io/n8nio/n8n
    restart: always
    ports:
      - "127.0.0.1:5678:5678"
    labels:
      - traefik.enable=true
      - traefik.http.routers.n8n.rule=Host(`${SUBDOMAIN}.${DOMAIN_NAME}`)
      - traefik.http.routers.n8n.tls=true
      - traefik.http.routers.n8n.entrypoints=web,websecure
      - traefik.http.routers.n8n.tls.certresolver=mytlschallenge
      - traefik.http.middlewares.n8n.headers.SSLRedirect=true
      - traefik.http.middlewares.n8n.headers.STSSeconds=315360000
      - traefik.http.middlewares.n8n.headers.browserXSSFilter=true
      - traefik.http.middlewares.n8n.headers.contentTypeNosniff=true
      - traefik.http.middlewares.n8n.headers.forceSTSHeader=true
      - traefik.http.middlewares.n8n.headers.SSLHost=${DOMAIN_NAME}
      - traefik.http.middlewares.n8n.headers.STSIncludeSubdomains=true
      - traefik.http.middlewares.n8n.headers.STSPreload=true
      - traefik.http.routers.n8n.middlewares=n8n@docker
    environment:
      - N8N_HOST=${SUBDOMAIN}.${DOMAIN_NAME}
      - N8N_PORT=5678
      - N8N_PROTOCOL=https
      - NODE_ENV=production
      - WEBHOOK_URL=https://${SUBDOMAIN}.${DOMAIN_NAME}/
      - GENERIC_TIMEZONE=${GENERIC_TIMEZONE}
    volumes:
      - n8n_data:/home/node/.n8n

volumes:
  traefik_data:
    external: true
  n8n_data:
    external: true
'


而小白的我只會最簡單的 Nginx Proxy manager，所以只好利用 Portainer 部署 n8n。

打開Portrainer，首先在 volume 中建立一個名叫 n8n-data 的 volume

{{< figure src="https://co.valent.bond/dllm/n8n-04.png" >}}

加入新 container

`Name` - n8n  
`Image` - n8nio/n8n:latest  
`Always pull the image` - Enabled  
`Manual network port publishing` - Host: 5678 / Container: 5678

{{< figure src="https://co.valent.bond/dllm/n8n-05.png" >}}
然後在下面 `Advanced container settings` 中 start with Volumes 選項 click 

`Map additional volume`.

`container` - /home/node/.n8n  
`volume` - 我們剛剛建立的volume `n8n-data`

{{< figure src="https://co.valent.bond/dllm/n8n-06.png" >}}
 
再設定一些環境參數

在 `Env` 選擇 `Advanced Mode` 貼上以下參數。

{{< figure src="https://co.valent.bond/dllm/n8n-07.png" >}}

```
TZ=Asia/Hong_Kong 
GENERIC_TIMEZONE=Asia/Hong_Kong
WEBHOOK_URL=http://my-ip-address:5678
EXECUTIONS_DATA_SAVE_ON_SUCCESS=none
EXECUTIONS_DATA_PRUNE=true
EXECUTIONS_DATA_MAX_AGE=24
DB_SQLITE_VACUUM_ON_STARTUP=true
```

最後，在 `Restart Policy` 中選擇 `Always` 
我們便可以 `Deploy the container`。

如無意外，可以便可以在 http://my-ip-address:5678 訪問n8n。

如果你和我一樣是使用 Oracle VM 的話，當然要在當然要加一條 Ingress rule 打開 port 5678。

更安全的做法當然當然是用 Nginx Proxy Manage 做反向代理。

在 DNS 中加入一條 A record 'n8n' (或CNAME record)， 指向 my-ipaddress。

{{< figure src="https://co.valent.bond/dllm/n8n-01.png" >}}

在 Nginx Proxy Manager 中 加入 n8n.my-doamin.com， 轉發到 port 5678

{{< figure src="https://co.valent.bond/dllm/n8n-03.png" >}}

加入之前已經申請好的 SSL certificate。

打開 https://n8n.mydomain.com

{{< figure src="https://co.valent.bond/dllm/n8n-02.png" >}}

大功告成！







