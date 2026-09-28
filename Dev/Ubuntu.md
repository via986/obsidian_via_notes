
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