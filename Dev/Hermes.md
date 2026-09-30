
`hermes update`
`hermes config check     # покажет недостающие опции`
`hermes config migrate   # добавит их интерактивно`

**Проверенный `~/.hermes/config.yaml`:**

```yaml
model:
  provider: openrouter
  default: ~anthropic/claude-sonnet-latest   # ID сверьте на openrouter.ai/models

terminal:
  backend: docker
  # docker_image не задаём — используется nousresearch/hermes-sandbox:desktop
  docker_forward_env: []          # секреты в контейнер не пробрасываются
  docker_network: true            # false = полностью без сети
  container_cpu: 1
  container_memory: 5120          # МБ
  container_disk: 51200           # МБ, работает только на XFS с pquota
  container_persistent: true      # один общий контейнер
  docker_volumes:
    - "/home/ВАШ_ЮЗЕР/.hermes/cache/documents:/output"
```

`hermes doctor`
`hermes gateway restart`
`docker ps --filter label=hermes-agent=1`

`hermes config set TELEGRAM_ALLOWED_USERS "123456789,987654321"`