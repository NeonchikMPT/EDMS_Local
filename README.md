# 🚀 EDMS - Система управления электронными документами

**EDMS** (Electronic Document Management System) — это Django-приложение для управления электронными документами и профилями пользователей. Проект предназначен для локального использования и идеально подходит для тестирования или обучения.

## ✨ Основные возможности

- 📤 **Загрузка и управление файлами**:
  - Аватарки пользователей (JPG, PNG) хранятся в `media/avatars/`.
  - Документы (PDF, DOCX, TXT) сохраняются в `media/docs/`.
- 👤 **Управление пользователями**:
  - Кастомная модель пользователя с полями `full_name`, `avatar`, `email_notifications`, `role`.
  - Роли: `admin` (администратор) и `staff` (сотрудник).
  - Управление пользователями через интерфейс и админ-панель Django.
- 📊 **Экспорт и импорт данных**:
  - Экспорт пользователей и документов в CSV или SQL.
  - Импорт данных из CSV/SQL с обновлением существующих записей.
- 🔑 **Сброс пароля**:
  - Запрос сброса пароля через email с использованием токена.
- 🔔 **Уведомления**:
  - Звуковые уведомления (например, `notification.mp3`).
  - Email-уведомления для пользователей.

---

## 📋 Содержание

- 🛠 Требования
- ⚙️ Установка
- 📂 Настройка медиа и статических файлов
- 🚀 Запуск
- 🎮 Использование
- 🛡️ Устранение неполадок
- 📁 Структура проекта
- 📜 Лицензия
- 📬 Контакты

---

## 🛠 Требования

| Требование | Версия/Описание |
| --- | --- |
| 🐍 **Python** | 3.11.9 |
| 🗄 **PostgreSQL** | Последняя стабильная версия |
| 📦 **Git** | Для клонирования репозитория |
| 🔧 **pip** | Для установки зависимостей |
| 🌐 **venv** | Для создания виртуальной среды |

---

## ⚙️ Установка

### 1. Клонируйте репозиторий

```bash
git clone https://github.com/NeonchikMPT/EDMS_Local.git
cd EDMS_Local
```

### 2. Создайте виртуальную среду

На **Windows**:

```bash
python -m venv .venv
.venv\Scripts\activate
```

На **macOS/Linux**:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Установите зависимости

```bash
pip install -r requirements.txt
```

Ключевые зависимости:
- `django==5.2.1`
- `psycopg2-binary==2.9.10`
- `pillow==11.2.1`

### 4. Настройте PostgreSQL

1. Убедитесь, что PostgreSQL установлен и запущен.

2. Создайте базу данных:

   ```bash
   psql -U postgres
   ```

   ```sql
   CREATE DATABASE edms_db;
   \q
   ```

3. Обновите настройки базы данных в `edms/settings.py`:

   ```python
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.postgresql',
           'NAME': 'edms_db',
           'USER': 'postgres',
           'PASSWORD': 'your_postgres_password',
           'HOST': 'localhost',
           'PORT': '5432',
       }
   }
   ```

   Замените `'your_postgres_password'` на ваш пароль PostgreSQL.

### 5. Настройте email для сброса пароля

В `edms/settings.py` обновите настройки SMTP, если хотите использовать сброс пароля через email:

```python
EMAIL_HOST = 'smtp.mail.ru'
EMAIL_PORT = 465
EMAIL_USE_SSL = True
EMAIL_HOST_USER = 'your_email@example.com'
EMAIL_HOST_PASSWORD = 'your_email_password'
DEFAULT_FROM_EMAIL = 'your_email@example.com'
```

Замените `your_email@example.com` и `your_email_password` на ваши данные SMTP (например, для Mail.ru или Gmail).

### 6. Сгенерируйте SECRET_KEY

Замените `SECRET_KEY` в `edms/settings.py` на безопасный ключ:

```bash
python -c "import secrets; print(secrets.token_urlsafe(50))"
```

Обновите в `settings.py`:

```python
SECRET_KEY = 'your_new_secure_key'
```

---

## 📂 Настройка медиа и статических файлов

### Медиа-файлы

- **Аватарки** → `media/avatars/`
- **Документы** → `media/docs/`

1. Создайте папки:

   ```bash
   mkdir -p media/avatars media/docs
   ```

2. Убедитесь, что в `edms/settings.py` настроено:

   ```python
   MEDIA_URL = '/media/'
   MEDIA_ROOT = BASE_DIR / 'media'
   ```

3. Для macOS/Linux установите права:

   ```bash
   chmod -R 755 media
   ```

### Статические файлы

- Статические файлы (например, `favicon.ico`, `audio/notification.mp3`) хранятся в `static/`.

1. Создайте папку `static/` и добавьте файлы:
   - `favicon.ico` → `static/favicon.ico`
   - `notification.mp3` → `static/audio/notification.mp3`

2. Выполните сбор статических файлов:

   ```bash
   python manage.py collectstatic
   ```

3. Убедитесь, что в `edms/settings.py` настроено:

   ```python
   STATIC_URL = '/static/'
   STATICFILES_DIRS = [BASE_DIR / 'static']
   STATIC_ROOT = BASE_DIR / 'staticfiles'
   ```

4. В шаблонах используйте:

   ```html
   {% load static %}
   <link rel="icon" href="{% static 'favicon.ico' %}">
   <audio src="{% static 'audio/notification.mp3' %}"></audio>
   ```

---

## 🚀 Запуск

1. Примените миграции:

   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

2. Создайте суперпользователя:

   ```bash
   python manage.py createsuperuser
   ```

   Пример:
   - **Username**: `admin`
   - **Email**: `admin@example.com`
   - **Password**: `Admin123!`

3. Запустите сервер:

   ```bash
   python manage.py runserver
   ```

4. Откройте:
   - Главная страница: `http://localhost:8000`
   - Профиль: `http://localhost:8000/users/profile/`
   - Список пользователей (для админов): `http://localhost:8000/users/list/`
   - Админ-панель: `http://localhost:8000/admin`

---

## 🎮 Использование

- **Обновление профиля**:
  1. Перейдите на `/users/profile/`.
  2. Загрузите аватар (JPG, PNG) или обновите данные (ФИО, уведомления).
  3. Нажмите "Сохранить". Аватар сохранится в `media/avatars/`.

- **Управление документами**:
  1. Перейдите на `/docs/`.
  2. Загрузите документ (PDF, DOCX, TXT).
  3. Файл сохранится в `media/docs/`.

- **Администрирование пользователей**:
  1. Перейдите на `/users/list/` (требуется роль `admin`).
  2. Создавайте, редактируйте или удаляйте пользователей.
  3. Управляйте ролями и уведомлениями.

- **Экспорт/импорт данных**:
  1. Перейдите на `/users/export_import/` (требуется роль `admin`).
  2. Выберите экспорт в CSV/SQL или загрузите файл для импорта.

- **Сброс пароля**:
  1. Перейдите на `/users/password_reset/`.
  2. Введите email и следуйте инструкциям в письме.

---

## 🛡️ Устранение неполадок

| Проблема | Решение |
| --- | --- |
| **Ошибка базы данных** | Проверьте `DATABASES` в `settings.py`. Убедитесь, что PostgreSQL запущен и пароль верный. |
| **Медиа-файлы не загружаются** | Проверьте `enctype="multipart/form-data"` в формах. Убедитесь, что папки `media/avatars/` и `media/docs/` существуют. |
| **Изображения/файлы не отображаются** | Убедитесь, что `pillow` установлен (`pip install pillow`). Проверьте `MEDIA_URL`/`MEDIA_ROOT` и `STATIC_URL`/`STATICFILES_DIRS`. |
| **Ошибка favicon (404)** | Поместите `favicon.ico` в `static/` и используйте `<link rel="icon" href="{% static 'favicon.ico' %}">` в шаблонах. |
| **Ошибка сессии после обновления профиля** | Убедитесь, что поля пароля в форме необязательные. Проверьте `users/views.py` (`profile_view`). |
| **Ошибки миграций** | Удалите старые миграции: `find . -path "*/migrations/*.py" -not -name "__init__.py" -delete`, затем `makemigrations` и `migrate`. |

**Дополнительные шаги**:
- Проверьте логи сервера:

  ```bash
  python manage.py runserver
  ```

- Проверьте пути к файлам:

  ```bash
  python manage.py shell
  ```

  ```python
  from users.models import User
  user = User.objects.get(email='your_email')
  print(user.avatar.url)  # Должно вывести /media/avatars/<filename>
  ```

---

## 📁 Структура проекта

```plaintext
EDMS_Local/
├── media/              # Медиа-файлы
│   ├── avatars/        # Аватарки
│   └── docs/           # Документы
├── static/             # Статические файлы (favicon.ico, audio/notification.mp3)
├── users/              # Приложение для пользователей
├── docs/               # Приложение для документов
├── templates/          # HTML-шаблоны
├── edms/               # Настройки проекта
├── manage.py           # Управление проектом
├── requirements.txt    # Зависимости
└── README.md           # Документация
```

---

## 📬 Контакты

👤 **NeonchikMPT**\
🔗 [GitHub](https://github.com/NeonchikMPT)\
📧 dsistema@internet.ru

*Создано с ❤️ для упрощения работы с документами!*
