
`mkdir -p /opt/hysteria && cd /opt/hysteria`
`openssl rand -base64 24   # сгенерируйте пароль`

LMndo6zC7KGlHNtd472c6gtT9UNIwVg3

`sudo usermod -aG docker $USER`

`opt/hysteria/hysteria.yaml`:

yaml

```yaml
listen: :443

tls:
  cert: /etc/letsencrypt/live/xpira.mooo.com/fullchain.pem
  key: /etc/letsencrypt/live/xpira.mooo.com/privkey.pem

auth:
  type: password
  password: ВАШ_ПАРОЛЬ

masquerade:
  type: proxy
  proxy:
    url: https://xpira.mooo.com
    rewriteHost: true
```

### docker-compose.yml

Это пример из документации с одним изменением. Вместо тома `acme` монтируем `/etc/letsencrypt` целиком, потому что в `live/` лежат симлинки на `archive/`:

yaml

```yaml
services:
  hysteria:
    image: tobyxdd/hysteria
    container_name: hysteria
    restart: always
    network_mode: "host"
    volumes:
      - ./hysteria.yaml:/etc/hysteria.yaml:ro
      - /etc/letsencrypt:/etc/letsencrypt:ro
    command: ["server", "-c", "/etc/hysteria.yaml"]
```

`NET_ADMIN` здесь не нужен: он требуется только для port hopping.

bash

```bash
docker compose up -d
docker logs -f hysteria
```

В логах должно быть `server up and running`

**Рестарт Hysteria после обновления сертификатов**

`echo -e '#!/bin/sh\ndocker restart hysteria' | sudo tee /etc/letsencrypt/renewal-hooks/deploy/hysteria.sh > /dev/null`
`sudo chmod +x /etc/letsencrypt/renewal-hooks/deploy/hysteria.sh`

hysteria2://ВАШ_ПАРОЛЬ@xpira.mooo.com:443/?sni=xpira.mooo.com#xpira