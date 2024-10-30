# Kittygram: Социальная сеть для любителей котиков

Kittygram – это социальная сеть для обмена фотографиями ваших любимых питомцев. Этот проект демонстрирует использование Docker, Docker Compose, Nginx и GitHub Actions для непрерывной интеграции и доставки (CI/CD).

## Запуск проекта локально

### Backend

1. Клонируйте репозиторий:

```bash
git clone https://github.com/yandex-praktikum/kittygram_backend.git
cd kittygram_backend
```

2. Создайте и активируйте виртуальное окружение:

```bash
python3 -m venv env
source env/bin/activate  # Linux/macOS
source env/scripts/activate  # Windows
```

3. Обновите pip и установите зависимости:

```bash
python3 -m pip install --upgrade pip
pip install -r requirements.txt
```

4. Выполните миграции:

```bash
python3 manage.py migrate
```

5. Запустите проект:

```bash
python3 manage.py runserver
```

### Frontend

1. Клонируйте репозиторий:

```bash
git clone https://github.com/yandex-praktikum/kittygram_frontend.git
cd kittygram_frontend
```

2. Установите зависимости:

```bash
npm i
```

3. Запустите проект:

```bash
npm run start
```

## Запуск проекта в Docker

1. Клонируйте репозиторий `kittygram_final`:

```bash
git clone <ваш репозиторий kittygram_final>
cd kittygram_final
```

2. Запустите проект с помощью Docker Compose:

```bash
docker-compose up -d
```

## CI/CD с помощью GitHub Actions

Workflow для CI/CD настроен в файле `.github/workflows/main.yml`.  Он выполняет следующие действия при пуше в ветку `main`:

* Проверка кода backend на соответствие PEP8.
* Запуск тестов frontend и backend.
* Сборка и отправка образов на Docker Hub:
    * `<ваш_логин_на_докерхабе>/kittygram_backend`
    * `<ваш_логин_на_докерхабе>/kittygram_frontend`
    * `<ваш_логин_на_докерхабе>/kittygram_gateway`
* Обновление образов на сервере и перезапуск приложения с помощью Docker Compose.
* Сборка статики, миграции и перенос статики в volume.
* Отправка уведомления об успешном деплое в Telegram.

## Настройка сервера для деплоя

1. Очистите сервер от лишних данных:

```bash
npm cache clean --force
sudo apt clean
sudo journalctl --vacuum-time=1d
sudo docker system prune -af
```

2. Удалите Gunicorn-сервис Kittygram (если существует).

3. Создайте директорию `kittygram/` в домашней директории сервера.

4. Настройте Nginx для проксирования запросов на порт 9000 Docker.

## Файл `tests.yml`

Для проверки работы проекта создайте файл `tests.yml` в корне репозитория со следующим содержимым:

```yaml
repo_owner: <ваш_логин_на_гитхабе>
kittygram_domain: <полная ссылка (https://доменное_имя) на ваш проект Kittygram>
taski_domain: <полная ссылка (https://доменное_имя) на ваш проект Taski>
dockerhub_username: <ваш_логин_на_докерхабе>
```

## Чек-лист перед отправкой

* Проект Taski доступен по доменному имени, указанному в `tests.yml`.
* Проект Kittygram доступен по доменному имени, указанному в `tests.yml`.
* Пуш в ветку `main` запускает тестирование и деплой Kittygram, после чего вы получаете уведомление в Telegram.
* В корне проекта есть файл `kittygram_workflow.yml` (скопируйте содержимое `.github/workflows/main.yml` в этот файл).
