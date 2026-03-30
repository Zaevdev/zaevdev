<div align="center">
  <img src="https://media.giphy.com/media/M9gbBd9nbDrOTu1Mqx/giphy.gif" width="100"/>
  <div id="badges">
    <a href="https://vk.com/zzzaev">
      <img src="https://img.shields.io/badge/Vkontakte-blue?style=for-the-badge&logo=vk&logoColor=white" alt="VK Badge"/>
    </a>
    <a href="mailto:zaevdev@gmail.com">
      <img src="https://img.shields.io/badge/Gmail-red?style=for-the-badge&logo=gmail&logoColor=white" alt="Gmail Badge"/>
    </a>
    <a href="https://t.me/zaevdev">
      <img src="https://img.shields.io/badge/Telegram-lightblue?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram Badge"/>
    </a>
  </div>
</div>

---

```php
<?php declare(strict_types=1);

final readonly class Developer
{
    public string $name = 'Alexander Zaev';
    public string $location = 'Moscow, Russia';

    /** @var list<Framework> */
    public array $frameworks = [Framework::Laravel, Framework::Yii2];

    /** @var list<Pattern> */
    public array $architecture = [
        Pattern::DDD,
        Pattern::HexagonalArchitecture,
        Pattern::EventSourcing,
        Pattern::CQRS,
        Pattern::TDD,
    ];

    public function handle(Challenge $challenge): Solution
    {
        return $this->pipeline
            ->pipe(new AnalyzeDomain($challenge))
            ->pipe(new DesignBoundedContexts())
            ->pipe(new WriteFailingTests())      // TDD: red
            ->pipe(new Implement())              // TDD: green
            ->pipe(new Refactor())               // TDD: refactor
            ->pipe(new EnforceArchBoundaries())  // PHPStan level 9
            ->run();
    }
}
```

---

### Привет, я Александр 👋

Senior backend-инженер. Строю **высоконагруженные domain-driven системы** — от монолитов до микросервисов. 5+ лет на PHP, строгая дисциплина TDD и убеждение, что архитектурные решения важнее выбора фреймворка.

Проектирую системы с **гексагональной архитектурой**, строгими bounded contexts, кросс-доменными event bus'ами и достаточным количеством тестов, чтобы деплоить в пятницу.

**Сейчас:** готовлю [Ikhtion](https://github.com/zaevdev) к продакшну — оптимизация производительности, E2E-покрытие, соответствие 152-ФЗ, бета-запуск.

---

### Что я строю

<table>
<tr>
<td width="50%" valign="top">

**Ikhtion** — PWA для рыболовов 🎣
<br/><sub>Флагманский проект. PWA: 9 DDD-доменов, AI-распознавание рыбы (YOLOv8 + BioCLIP cascade), real-time сообщество через WebSocket, геймификация с event sourcing, геопространственные запросы PostGIS, 119 API-эндпоинтов, 1264+ тестов.</sub>

`Laravel 12 (Octane)` `Next.js 15` `React 19` `PostGIS` `Redis` `FastAPI` `Docker`

<sub>DDD + Event Sourcing + CQRS | PHPStan level 9 | Pest Arch boundaries | 617 PHP + 322 TS файлов</sub>

</td>
<td width="50%" valign="top">

**Dovezlo** — Сравнение стоимости доставки 📦
<br/><sub>Сравнение тарифов российских служб доставки (СДЭК и др.). Clean architecture бэкенд, Vue 3 SPA, Telegram-бот + WebApp, подписки, промокоды, оплата через Robokassa.</sub>

`Laravel 12` `Vue 3` `PostgreSQL` `Redis` `Telegram Bot API` `Robokassa`

<sub>DDD + Hexagonal | OpenAPI/Swagger | Codeception + PHPStan</sub>

</td>
</tr>
</table>

---

### Архитектура — то, что действительно важно

```
src/Domain/{Auth,Spot,Catch,Gamification,Community,AI,Payments,Notifications}/
  +-- Application/    Commands, Queries, Handlers, DTOs
  +-- Domain/         Entities, Value Objects, Events, Repository interfaces
  +-- Infrastructure/ Eloquent Repos, External APIs, Adapters
```

- **Гексагональная архитектура** — ядро домена не зависит от фреймворка. Ports & Adapters на всех уровнях.
- **Кросс-доменное взаимодействие только через Domain Events.** Без shared models, без cross-imports.
- **Event Sourcing** там, где важен аудит и воспроизведение состояния — не как карго-культ.
- **CQRS** — раздельные command/query пайплайны с выделенными read-моделями и проекциями.
- **TDD** — red-green-refactor как дефолтный процесс. Тесты формируют дизайн, а не просто проверяют.
- **Архитектурные fitness-функции** — Pest Arch тесты контролируют границы слоёв в CI.

---

### Базы данных — не просто `SELECT *`

```sql
-- Геопространственный поиск рыболовных точек в радиусе с фильтрацией по запретным зонам
SELECT s.*, ST_Distance(s.location, ST_MakePoint(:lng, :lat)::geography) AS distance
FROM spots s
WHERE ST_DWithin(s.location, ST_MakePoint(:lng, :lat)::geography, :radius)
  AND NOT EXISTS (
      SELECT 1 FROM restricted_zones rz
      WHERE ST_Contains(rz.boundary, s.location::geometry)
  )
ORDER BY distance;
```

| Что | Как применяю |
|-----|-------------|
| **PostgreSQL** | Основная СУБД. Миграции, партицирование, materialized views для проекций |
| **PostGIS** | Геопространственные индексы (GiST), ST_DWithin, boundary-запросы для зон запрета |
| **Event Store** | Своя реализация на PostgreSQL — append-only таблица, снапшоты, upcasters для эволюции схемы |
| **Read Models** | Асинхронные проекции из event stream в плоские таблицы — лидерборды, профили, статистика |
| **Redis** | Кеширование, очереди (Horizon), rate limiting, сессии, pub/sub для real-time |
| **MySQL** | Опыт с legacy-проектами на Yii2, миграция на PostgreSQL |
| **Оптимизация** | EXPLAIN ANALYZE, составные индексы, partial indexes, query plan анализ |
| **Проектирование** | Нормализация до 3NF для write-моделей, денормализация для read-моделей (CQRS) |

---

### Стек

**Бэкенд и фреймворки**

![PHP](https://img.shields.io/badge/PHP_8.3--8.5-777BB4?style=flat-square&logo=php&logoColor=white)
![Laravel](https://img.shields.io/badge/Laravel_10--12-FF2D20?style=flat-square&logo=laravel&logoColor=white)
![Yii2](https://img.shields.io/badge/Yii2-0073BB?style=flat-square&logo=php&logoColor=white)
![Octane](https://img.shields.io/badge/Octane_(Swoole)-FF2D20?style=flat-square&logo=laravel&logoColor=white)
![RoadRunner](https://img.shields.io/badge/RoadRunner-6C7A89?style=flat-square)

**Базы данных и очереди**

![PostgreSQL](https://img.shields.io/badge/PostgreSQL_16+-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![PostGIS](https://img.shields.io/badge/PostGIS-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat-square&logo=rabbitmq&logoColor=white)

**Архитектура и паттерны**

![DDD](https://img.shields.io/badge/DDD-5C2D91?style=flat-square)
![Hexagonal](https://img.shields.io/badge/Hexagonal_Architecture-2C3E50?style=flat-square)
![Event Sourcing](https://img.shields.io/badge/Event_Sourcing-E67E22?style=flat-square)
![CQRS](https://img.shields.io/badge/CQRS-27AE60?style=flat-square)
![TDD](https://img.shields.io/badge/TDD-C0392B?style=flat-square)
![Microservices](https://img.shields.io/badge/Microservices-1ABC9C?style=flat-square)

**Фронтенд (при необходимости)**

![Vue](https://img.shields.io/badge/Vue_3-4FC08D?style=flat-square&logo=vue.js&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js_15-000000?style=flat-square&logo=next.js&logoColor=white)
![React](https://img.shields.io/badge/React_19-61DAFB?style=flat-square&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Tailwind](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)

**AI / ML**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)

**Качество и тестирование**

![PHPStan](https://img.shields.io/badge/PHPStan-Level_9-8B5CF6?style=flat-square)
![Tests](https://img.shields.io/badge/Tests-1264+-22C55E?style=flat-square)
![Pest](https://img.shields.io/badge/Pest-F472B6?style=flat-square)
![PHPUnit](https://img.shields.io/badge/PHPUnit-3C7EBB?style=flat-square)
![Codeception](https://img.shields.io/badge/Codeception-F05033?style=flat-square)
![Playwright](https://img.shields.io/badge/Playwright_E2E-2EAD33?style=flat-square&logo=playwright&logoColor=white)

**Инфраструктура и DevOps**

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Docker Compose](https://img.shields.io/badge/Docker_Compose-2496ED?style=flat-square&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=flat-square&logo=nginx&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)
![Yandex Cloud](https://img.shields.io/badge/Yandex_Cloud-5282FF?style=flat-square)

---

### Как я работаю

- **TDD по умолчанию** — red-green-refactor, а не «тесты потом допишем». Дизайн рождается из тестов.
- **Trunk-based development** — conventional commits, короткоживущие ветки, CI-гейты перед мержем
- **Архитектура в первую очередь** — bounded contexts, границы агрегатов и anti-corruption layers до написания первой строки кода
- **Опыт высоких нагрузок** — Swoole/RoadRunner, очереди, асинхронная обработка событий, read-модели и проекции
- **Российский рынок** — 152-ФЗ (персональные данные), 54-ФЗ (фискализация), Robokassa, экосистема Yandex Cloud
- **Модернизация legacy** — миграция Yii2-монолитов на Laravel через strangler fig pattern

