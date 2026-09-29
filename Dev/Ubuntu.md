###### **check version**
`lsb_release -a`

`sudo apt update`
`sudo apt upgrade`

`adduser sammy`

далее две команды равноценны, добавляют пользователя в группу sudo:

`usermod -aG sudo sammy`
`sudo gpasswd -a fastapi-user sudo`

`cat /etc/passwd`

Посмотреть, в каких группах состоит пользователь (https://askubuntu.com/questions/1366061/when-gpasswd-vs-usermod-deluser):
`id -a username`
The group listed as gid= is the user's primary group. groups= lists all groups the user belongs to (primary group is first, followed by supplementary groups).

`cat /etc/group`

`sudo deluser newuser`

If, instead, you want to delete the user's home directory when the user is deleted, you can issue the following command as root:
`deluser --remove-home newuser`

###### **Check Time**

Далее проверить временную зону и синхронизацию времени (в т.ч. настроить синхронизацию от хоста)

просмотр работающих служб:
`systemctl list-units --type=service --state=running`

Также нужно проверить конфликты портов
`sudo ss -tulpn | grep -E '80|443'`

###### **Add Swapfile**

 Allocate a 4GB swapfile
```bash
# Check existing swap (likely 0 or small)
free -h
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
# Make it permanent across reboots
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
# Verify
free -h
```
###### **.iutf8**

```bash
stty -a | grep -o '.iutf8'
```

Если выведет `-iutf8` (с минусом), флаг выключен. Включите:
```bash
stty iutf8
```

Чтобы флаг включался при каждом входе:
```bash
echo 'stty iutf8 2>/dev/null' >> ~/.bashrc
```