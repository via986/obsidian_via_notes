
103.85.113.225
Server (UDP): xpira.mooo.com:443
Password: TlCNnFiYbR9pDqJFj5iS39_jak4et6OS

Connection URI:
hysteria2://TlCNnFiYbR9pDqJFj5iS39_jak4et6OS@xpira.mooo.com:443?sni=xpira.mooo.com#Hysteria2

Saved files:
  URI:   /root/hysteria-xpira.mooo.com.txt
  Clash: /root/hysteria-xpira.mooo.com.yaml

У меня была проблема с тем, что сертификат не продлился вовремя. Оказалось, что это потому, что certbot не обнараружил работающий веб сервер на 80 порту.

В nginx я теперь добавил блок редиректа.
`server {`
    `listen 80;`
    `server_name xpira.mooo.com;`

    location / {
	     return 301 https://$host$request_uri;
    }
`}`

Также нужно проверить конфликты портов
`sudo ss -tulpn | grep -E '80|443'`

**Обычная правильная схема:**
- **Nginx** занимает **TCP**-порт `80` (для HTTP и проверки Certbot) и/или **TCP**-порт `443` (для веба/HTTPS).    
- **Hysteria** занимает **UDP**-порт `443` (так как Hysteria работает по протоколу QUIC/UDP, она отлично уживается на 443 порту параллельно с TCP-портом Nginx!).

Hysteria у вас берёт сертификат не из внутреннего ACME-кэша, а из стандартной системы **Certbot** по пути `/etc/letsencrypt/live/[xpira.mooo.com/](https://xpira.mooo.com/)`.

Выпуск сертификата без работающего вебсервера на 80 порту
`sudo certbot certonly --standalone -d xpira.mooo.com --force-renewal`
