**Собрать образ и запустить в фоновом режиме (-d)**
docker compose up -d --build

**Посмотреть статус контейнеров**
docker compose ps

**Посмотреть логи бэкенда в реальном времени**
docker compose logs -f beta-backend

**Остановить контейнеры**
docker compose down