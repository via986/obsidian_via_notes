
1. Создайте проброс. Здесь `TARGET_IP` — адрес целевого сервера, а 13389 — любой свободный порт:

```
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=13389 connectaddress=TARGET_IP connectport=3389
```

2. Проверьте, что запущена служба IP Helper, без неё portproxy не работает:

```
Set-Service iphlpsvc -StartupType Automatic
Start-Service iphlpsvc
```

3. Откройте порт в брандмауэре, лучше только для вашего IP:

```
New-NetFirewallRule -DisplayName "RDP relay 13389" -Direction Inbound -Protocol TCP -LocalPort 13389 -RemoteAddress ВАШ_IP -Action Allow
```

4. Проверьте результат:

```
netsh interface portproxy show all
netstat -ano | findstr 13389
```

**Удалить проброс:**

```
netsh interface portproxy delete v4tov4 listenaddress=0.0.0.0 listenport=13389
```

**Посмотреть текущий список firewall:**

```
Get-NetFirewallRule -DisplayName "RDP relay 13389" | Get-NetFirewallAddressFilter
```