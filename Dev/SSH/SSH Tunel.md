Если сеть моргнёт или соединение оборвётся, стандартный `ssh` зависнет в ожидании (пока не сработает долгий таймаут TCP) либо тихо завершится, а порт `1080` перестанет отвечать.

  Чтобы туннель работал непрерывно, используют один из трех подходов.

### Вариант 1. Встроенный Keep-Alive (быстрое решение)

Штатный SSH умеет проверять живость канала с помощью специальных пакетов (`ServerAliveInterval`). Если сервер не отвечает, SSH принудительно завершит сессию, а не будет висеть «мертвым грузом»:

  

Bash

```
ssh -D 1080 -N \
    -o "ServerAliveInterval=15" \
    -o "ServerAliveCountMax=3" \
    -o "ExitOnForwardFailure=yes" \
    user@your-server-ip
```

- `ServerAliveInterval=15`: каждые 15 секунд клиент отправляет пинг серверу внутри туннеля.
    
      
    
- `ServerAliveCountMax=3`: если 3 раза подряд (45 секунд) ответа нет, соединение принудительно разрывается.
    
      
    
- `ExitOnForwardFailure=yes`: если порт `1080` уже занят, ssh сразу завершится с ошибкой, а не запустится вхолостую.
    
      
    

_(Чтобы не писать это каждый раз, параметры можно прописать один раз в файл `~/.ssh/config`)._

  
### Вариант 2. Autossh (автоматический перезапуск туннеля)

Утилита **`autossh`** создана ровно для этой задачи: она запускает SSH и, как только процесс падает или туннель застревает, мгновенно поднимает его заново.

  
1. Установка:
       
    ```
    sudo apt install autossh
    ```
    
2. Запуск в фоне:
        
    ```
    AUTOSSH_GATETIME=0 autossh -M 0 -f -N -D 1080 \
      -o "ServerAliveInterval=15" \
      -o "ServerAliveCountMax=3" \
      user@your-server-ip
    ```
    
    - `AUTOSSH_GATETIME=0`: указывает перезапускать сессию даже при быстром сбое соединения на старте.
        
          
        
    - `-M 0`: отключает устаревший внутренний порт мониторинга autossh, полагаясь на надежный `ServerAliveInterval`.
        
          
        

> **Как проверить:** выполните `kill -9 $(pgrep ssh)` (имитация аварийного падения). Через секунду `autossh` сам создаст новый процесс `ssh`, а локальный порт `1080` останется рабочим.
> 
>   

### Вариант 3. Системный сервис (Systemd) — вариант «настроил и забыл»

Если машина работает постоянно и туннель должен подниматься при загрузке системы и перезапускаться при любых сетевых сбоях, создайте пользовательский systemd-юнит:


1. Создайте файл:
    
    
    ```
    mkdir -p ~/.config/systemd/user
    nano ~/.config/systemd/user/ssh-tunnel.service
    ```
    
2. Вставьте конфигурацию (замените `user@your-server-ip` на свои данные; требуется вход по SSH-ключу):
    
      
    
    Ini, TOML
    
    ```
    [Unit]
    Description=SSH SOCKS5 Tunnel
    After=network.target
    
    [Service]
    Type=simple
    ExecStart=/usr/bin/ssh -NT -D 1080 -o ServerAliveInterval=15 -o ServerAliveCountMax=3 -o ExitOnForwardFailure=yes user@your-server-ip
    Restart=always
    RestartSec=5
    
    [Install]
    WantedBy=default.target
    ```
    
3. Включите и запустите сервис:
    
      
    
    Bash
    
    ```
    systemctl --user daemon-reload
    systemctl --user enable --now ssh-tunnel.service
    ```
    

> **Как проверить:** выполните `systemctl --user status ssh-tunnel.service`. В статусе должно быть зеленое `active (running)`. Systemd будет автоматически перезапускать туннель через 5 секунд при любых обрывах.