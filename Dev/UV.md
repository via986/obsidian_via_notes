
`uv self update`
`uv init (можно один для всего проекта)`
`uv add fastapi "uvicorn[standard]" pydantic redis httpx`
`uv run python data_pipeline/main.py`
`uv run uvicorn backend.main:app --reload`

**Посмотреть доступные версии**
uv python list

**Скачать нужную версию**
uv python install 3.12

**Закрепить версию для текущего проекта (создаст .python-version)**
uv python pin 3.12

**Чтобы развернуть проект из репозитория с точными версиями из `uv.lock`:**
uv sync

#### CRONTAB

Используйте флаг `--directory` (или `-C`), чтобы `uv` знал, где искать окружение и зависимости:

```
# Запуск каждые 10 минут
*/10 * * * * /home/username/.local/bin/uv run --directory /home/username/my-project python main.py >> /home/username/my-project/cron.log 2>&1
```

- `--directory /путь/к/проекту` — указывает рабочую директорию проекта.
    
- `>> .../cron.log 2>&1` — перенаправляет и стандартный вывод, и ошибки в лог-файл (критично для отладки).

#### Вариант 2

**Создайте bash-скрипт**
Создайте файл, например `run_job.sh`, внутри директории вашего проекта:
Bash
```
nano /home/username/my-project/run_job.sh
```

Вставьте следующий шаблон:
Bash
```
#!/usr/bin/env bash
# Завершать работу при первой же ошибке и неразрешенных переменных
set -euo pipefail

# 1. Явно добавляем путь к uv в PATH (для cron это критично)
export PATH="$HOME/.local/bin:$PATH"

# 2. Переходим в директорию проекта
cd /home/username/my-project

# 3. (Опционально) Подгружаем переменные окружения из .env, если нужно
# export $(grep -v '^#' .env | xargs)

# 4. Запускаем приложение через uv run
uv run python main.py
```