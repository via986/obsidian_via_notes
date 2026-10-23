

`sudo nano /etc/systemd/system/mm_monitor.service`

`[Unit]`
`Description=Viking MM Monitor`
`After=network-online.target`
`Wants=network-online.target`

`[Service]`
`Type=simple`
`User=cqg`
`WorkingDirectory=/home/cqg/projects/viking_mm_monitor`
`ExecStart=/home/cqg/.local/bin/uv run /home/cqg/projects/viking_mm_monitor/main.py`
`Restart=always`
`RestartSec=5`
`# Чтобы логи (print/logging) сразу шли в journald без буферизации:`
`Environment=PYTHONUNBUFFERED=1`

`[Install]`
`WantedBy=multi-user.target`

`sudo systemctl daemon-reload`
`sudo systemctl enable --now mm_monitor.service`
`sudo systemctl status mm_monitor.service`

