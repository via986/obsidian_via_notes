
1. Добавьте псевдонимы в `C:\Windows\System32\drivers\etc\hosts`:

```
   10.0.0.5   srv-admin
   10.0.0.5   srv-user1
   10.0.0.5   srv-user2
```

2. Сохраните учётные данные для каждого имени:

```
   cmdkey /generic:TERMSRV/srv-admin /user:DOMAIN\admin /pass:Пароль1
   cmdkey /generic:TERMSRV/srv-user1 /user:DOMAIN\user1 /pass:Пароль2
```

3. Подключайтесь командой `mstsc /v:srv-admin` или через ярлык или .rdp-файл для каждого пользователя. В .rdp-файле можно указать строку `username:s:DOMAIN\user1`

**Порт указывается в настройках соединения, не в hosts!!!**