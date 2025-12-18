# 🎉 Sber Congratulations Agent

![Python Version](https://img.shields.io/badge/python-3.10%2B-blue)
![FastAPI](https://img.shields.io/badge/FastAPI-0.104-green)
![License](https://img.shields.io/badge/license-MIT-lightgrey)
![Hackathon](https://img.shields.io/badge/hackathon-OPD_2024-orange)
![Status](https://img.shields.io/badge/status-active_development-yellow)

**AI-агент для автоматической генерации и отправки персонализированных поздравлений клиентам Сбера**  
*Реализация кейса №2 "Агент по подготовке поздравительных сообщений клиентам" для хакатона ОПД*

## 🎯 О проекте

Автоматизированная система, которая **регулярно, на основе актуальных данных**:
1. **Идентифицирует поводы** для поздравлений (дни рождения, профессиональные праздники)
2. **Собирает данные о компаниях** (ИНН, ОКВЭД, руководители)
3. **Генерирует искренне персонализированный контент** через нейросети
4. **Доставляет через правильный канал** в нужный момент
5. **Оповещает сотрудников** об отправленных поздравлениях

### ✨ Ключевые особенности
- **🤖 AI-генерация текста** через GigaChat Pro
- **🎨 AI-генерация изображений** через Kandinsky 3.0
- **🏢 Работа с данными компаний** (ИНН, ОКВЭД, руководители)
- **🎓 Профессиональные праздники** с учетом вида деятельности
- **📅 Автоматические триггеры** (ежедневные проверки + ручной запуск)
- **👤 Глубокая персонализация** (сегмент клиента, история, предпочтения)
- **📱 Мультиканальность** (email, Telegram, SMS)
- **📊 Веб-панель управления** для сотрудников Сбера
- **🔔 Оповещение сотрудников** об отправленных поздравлениях

## 🏗️ Архитектура системы

### Визуализация архитектуры

```mermaid
graph TB
    subgraph "Пользовательские интерфейсы"
        WEB[Веб-панель]
        TG[Telegram Bot]
        API[API для интеграций]
    end

    subgraph "Бэкенд"
        FASTAPI[FastAPI приложение]
        CORE[Ядро системы]
        
        subgraph "Модули системы"
            MOD_TRIG[Триггеры событий]
            MOD_GEN[Генерация контента]
            MOD_SEND[Отправка сообщений]
            MOD_ANAL[Аналитика]
            MOD_DATA[Обработка данных компаний]
            MOD_NOTIFY[Уведомления сотрудников]
        end
        
        DB[(База данных)]
    end

    subgraph "Внешние сервисы"
        GIGA[GigaChat API]
        KAND[Kandinsky API]
        SMTP[SMTP сервер]
        TG_API[Telegram API]
        DADATA[DaData API]
        HOLIDAY_API[API праздников]
    end

    WEB --> FASTAPI
    TG --> FASTAPI
    API --> FASTAPI

    FASTAPI --> CORE
    CORE --> MOD_TRIG
    CORE --> MOD_GEN
    CORE --> MOD_SEND
    CORE --> MOD_ANAL
    CORE --> MOD_DATA
    CORE --> MOD_NOTIFY
    CORE --> DB

    MOD_GEN --> GIGA
    MOD_GEN --> KAND
    MOD_SEND --> SMTP
    MOD_SEND --> TG_API
    MOD_DATA --> DADATA
    MOD_TRIG --> HOLIDAY_API
```

### Структура проекта
```
sber-congratulations-agent/
├── docs/                           # 📚 Документация
├── src/                            # 💻 Исходный код
│   ├── core/                       # ⚙️ Ядро (конфиг, БД, модели)
│   ├── api/                        # 🌐 FastAPI приложение + роутеры
│   ├── modules/                    # 🔧 Модули системы:
│   │   ├── trigger/                # ⏰ Триггеры событий
│   │   ├── generator/              # 🎨 Генерация (GigaChat/Kandinsky)
│   │   ├── sender/                 # 📤 Отправка сообщений
│   │   ├── analytics/              # 📊 Аналитика и отчеты
│   │   ├── data_processor/         # 🏢 Обработка данных компаний
│   │   └── notifications/          # 🔔 Уведомления сотрудников
│   ├── frontend/                   # 🎨 Веб-интерфейс
│   ├── scripts/                    # 📜 Вспомогательные скрипты
│   └── utils/                      # 🛠️ Утилиты
├── tests/                          # 🧪 Тесты
├── data/                           # 📂 Данные (JSON, CSV)
└── конфигурационные файлы          # ⚙️ Docker, requirements и т.д.
```

### Технологический стек
- **Backend**: Python 3.10+, FastAPI, SQLAlchemy, Pydantic
- **База данных**: PostgreSQL (продакшен) / SQLite (разработка)
- **AI интеграции**: GigaChat API, Kandinsky 3.0 API
- **Планировщик**: Celery + Redis
- **Фронтенд**: Jinja2, Bootstrap 5, Chart.js
- **Отправка**: SMTP (email), Telegram Bot API, SMS API
- **Деплой**: Docker, Docker Compose
- **Документация**: Swagger/OpenAPI

## 🚀 Быстрый старт

### Предварительные требования
- Python 3.10 или выше
- Docker и Docker Compose (рекомендуется)
- API ключи:
  - [GigaChat API](https://developers.sber.ru/portal/products/gigachat-api)
  - [Kandinsky API](https://fusionbrain.ai/)
  - Telegram Bot Token (через @BotFather)

### Установка и запуск

```bash
# 1. Клонировать репозиторий
git clone https://github.com/your-username/sber-congratulations-agent.git
cd sber-congratulations-agent

# 2. Настроить окружение
cp .env.example .env
# Отредактировать .env, добавив свои API ключи

# 3. Запустить через Docker (рекомендуется)
docker-compose up -d

# ИЛИ установить локально
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt

# 4. Инициализировать базу данных
python src/scripts/seed_database.py

# 5. Запустить сервер
uvicorn src.api.main:app --reload --host 0.0.0.0 --port 8000
```

### Доступ к системе
- **Веб-интерфейс**: http://localhost:8000
- **API документация (Swagger)**: http://localhost:8000/docs
- **API документация (ReDoc)**: http://localhost:8000/redoc
- **Telegram бот**: Начать диалог с @YourBotName

## 📖 Использование

### Для сотрудников Сбера
1. **Ежедневный отчет**: Каждое утро получаете список, кого нужно поздравить
2. **Веб-панель**: Просмотр клиентов, календарь событий, ручная отправка
3. **Контроль качества**: Предпросмотр сгенерированных поздравлений перед отправкой
4. **Аналитика**: Статистика отправок, эффективность, обратная связь

### Автоматический режим
Система работает самостоятельно:
- **08:00**: Проверка событий на день
- **08:30**: Генерация контента через AI
- **09:00**: Отправка поздравлений
- **10:00**: Отчет сотрудникам

### Ручной режим
1. Зайти в веб-панель → "Ручная отправка"
2. Выбрать клиента и тип события
3. Просмотреть сгенерированный контент
4. Нажать "Отправить"

## 🔧 Конфигурация

### Основные настройки (.env)
```bash
# API ключи
GIGACHAT_API_KEY=ваш_ключ_гигачат
KANDINSKY_API_KEY=ваш_ключ_кандинский
TELEGRAM_BOT_TOKEN=токен_бота

# База данных
DATABASE_URL=postgresql://user:pass@localhost:5432/congratulations

# SMTP для отправки email
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=ваш_email@gmail.com
SMTP_PASSWORD=пароль_приложения

# Настройки системы
BIRTHDAY_DAYS_AHEAD=7           # За сколько дней предупреждать о ДР
SEND_TIME=09:00                 # Время отправки утром
DEFAULT_TONE=дружеский          # Тон по умолчанию
```

### Модули промптов
Система использует YAML-файлы для промптов:
- `src/modules/generator/templates/birthday_prompts.yaml` - для дней рождений
- `src/modules/generator/templates/holiday_prompts.yaml` - для праздников
- `src/modules/generator/templates/professional_prompts.yaml` - для профессиональных праздников

## 👥 Команда проекта

| Роль | Участник | Основная ответственность |
|------|----------|--------------------------|
| 🎯 **Архитектор/Тимлид** | Никита Красиков | Общая архитектура, интеграция, GigaChat API |
| 🗄️ **Бэкенд-разработчик** | [Имя] | База данных, CRM, логика триггеров |
| 🤖 **AI-разработчик** | [Имя] | Промпт-инжиниринг, GigaChat, Kandinsky |
| 🎨 **Фронтенд-разработчик** | [Имя] | Веб-интерфейс, UX/UI, визуализация |
| 🔗 **Интеграционный разработчик** | [Имя] | Отправка, внешние API, тестирование |

## 📞 Контакты и поддержка

- **Репозиторий**: [github.com/your-username/sber-congratulations-agent](https://github.com/your-username/sber-congratulations-agent)
- **Вопросы по проекту**: issues в GitHub
- **Команда хакатона**: [Название команды]
- **Для Сбера**: Демо доступно по запросу

## 📝 Лицензия

Этот проект распространяется под лицензией MIT. Подробнее см. в файле [LICENSE](LICENSE).

---

## 🔮 Дорожная карта развития

### Хакатон MVP (текущее)
- [x] Базовая архитектура и структура проекта
- [x] Модели данных и БД
- [ ] Интеграция GigaChat для генерации текста
- [ ] Интеграция Kandinsky для генерации изображений
- [ ] Модуль обработки данных компаний (ИНН, ОКВЭД)
- [ ] Система профессиональных праздников
- [ ] Уведомления сотрудников банка
- [ ] Веб-интерфейс для управления
- [ ] Отправка через email и Telegram
- [ ] Демонстрационный сценарий

### После хакатона
- [ ] Интеграция с реальной CRM Сбера
- [ ] ML-оптимизация промптов на основе обратной связи
- [ ] Мультиязычная поддержка
- [ ] Мобильное приложение для сотрудников
- [ ] Расширенная аналитика и A/B тестирование

---

**Разработано для хакатона ОПД 2025**  
*С использованием технологий Сбера: GigaChat и Kandinsky*
