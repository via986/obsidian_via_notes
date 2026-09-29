создание ключа
`ssh-keygen -t ed25519 -f "$env:USERPROFILE\.ssh\id_ed25519_my_new_server"`

Команда `ssh-keygen -t ed25519` создаёт ключ с использованием современного алгоритма Ed25519 (на основе эллиптических кривых), в то время как (`-t rsa -b 4096`) использует классический алгоритм RSA с длиной ключа 4096 бит.

Внутри самого SSH-ключа логин (имя пользователя) не зашивается.
SSH-ключ — это просто криптографическая пара (случайный набор символов и математических алгоритмов), которая служит только для проверки вашей цифровой подписи. Сам по себе ключ никак не привязан к конкретному имени пользователя ни на вашем компьютере, ни на сервере.

Откройте файл конфигурации в блокноте

`notepad "$env:USERPROFILE\.ssh\config"`

Добавьте туда настройки для вашего сервера:
`Host my-server`
    `HostName IP_адрес_вашего_VDS`
    `User root`
    `IdentityFile ~/.ssh/id_rsa_my_new_server`

Добавление SSH ключа на сервер

`Get-Content "$env:USERPROFILE\.ssh\id_ed25519_my_new_server.pub" | ssh логин@IP_адрес_сервера "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys"`


`sudo nano /etc/ssh/sshd_config`
`PermitRootLogin no`
`PasswordAuthentication no` 
`ChallengeResponseAuthentication no` 
`KbdInteractiveAuthentication no`

**CHECK /etc/ssh/sshd_config.d/**


`sudo systemctl restart ssh`