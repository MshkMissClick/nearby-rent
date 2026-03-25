# Nearby Rent 🏠🔧

> **Микросервисная платформа для аренды вещей между соседями**  
> Бери дрель на час, палатку на выходные или проектор на вечер — без переплат и сложной логистики.

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Java](https://img.shields.io/badge/Java-17%2B-orange)](https://java.com)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-brightgreen)](https://spring.io/projects/spring-boot)
[![React](https://img.shields.io/badge/React-18.x-blue)](https://reactjs.org)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15.x-blue)](https://postgresql.org)
[![Docker](https://img.shields.io/badge/Docker-24.x-blue)](https://docker.com)

**Nearby Rent** — это C2C-платформа, соединяющая людей в пределах района.  
Владельцы сдают вещи, арендаторы берут их **разово** или по **подписке**.

Ключевая идея: минимальные цены, личная встреча, доверие между соседями, никакой почты и курьеров.

---

## 🏗 Архитектура

### Высокоуровневая схема

<img width="1280" height="461" alt="image" src="https://github.com/user-attachments/assets/ba47b94f-5663-456e-80a3-7a1c17742e73" />


### Ключевые архитектурные решения

| Решение | Обоснование |
|---------|-------------|
| **Микросервисная архитектура** | Независимое развёртывание, масштабирование и развитие сервисов |
| **API Gateway (Spring Cloud Gateway)** | Единая точка входа, маршрутизация, первичная проверка JWT |
| **Database-per-Service** | Каждый микросервис имеет собственную схему/БД PostgreSQL — слабая связанность |
| **Асинхронное взаимодействие** | Kafka для событий между сервисами (например, после оплаты → уведомление) |
| **Stateless сервисы** | Горизонтальное масштабирование без потери сессии |
| **Кеширование** | Redis для часто запрашиваемых данных |

---

## 🧩 Микросервисы

| Сервис | Ответственность | Технологии |
|--------|-----------------|------------|
| **Auth Service** | Регистрация, аутентификация, выдача JWT/Refresh токенов | Spring Security, JWT, BCrypt |
| **User Service** | Управление профилями, рейтинги, контактные данные | Spring Data JPA, PostgreSQL |
| **Order Service** | Каталог вещей, бронирование, жизненный цикл сделки | Spring Data JPA, PostgreSQL, Kafka |
| **Payment Service** | Взаимодействие с платежными шлюзами, фиксация транзакций | Spring Boot, Kafka |

---

## 📊 Показатели качества

| Показатель | Целевое значение / Реализация |
|------------|-------------------------------|
| **Функциональность** | CRUD для профилей и объявлений, валидация через DTO, взаимодействие микросервисов |
| **Производительность** | Время отклика < 200 мс для основных операций, HikariCP для пула соединений |
| **Масштабируемость** | Горизонтальное масштабирование stateless сервисов, балансировка нагрузки |
| **Безопасность** | JWT + Refresh Token, BCrypt для паролей, @PreAuthorize на методах |
| **Надёжность** | Глобальная обработка ошибок (@ControllerAdvice), информативные JSON-ответы |
| **Документирование** | Swagger UI (springdoc-openapi) по адресу `/swagger-ui.html` |

---

## Требования
| Инструмент | Минимальная версия | Проверка установки |
|------------|--------------------|--------------------|
|Java JDK | 17 |java -version |
|Node.js | 18.x | node -v |
|Docker | 20.10+ |docker --version| 
|Docker Compose | 2.x (V2) |docker-compose --version |

## 🚀 Быстрый старт

```bash
# 1. Клонировать репозиторий
git clone https://github.com/your-org/nearby-rent.git
cd nearby-rent

# 2. Настройка окружения 
Скопируйте пример файла конфигурации и при необходимости отредактируйте его (пароли БД, ключи API):
cp .env.example .env

# 3. Запустить все сервисы
docker-compose up --build
```
🔗 Доступ к сервисам
После успешного запуска проект будет доступен по следующим адресам:
| Компонент | Адрес |
|-----------|-------|
| Frontend (UI) | http://localhost:3000 |
| Backend API | http://localhost:8080 |
| API Documentation (Swagger) | http://localhost:8080/swagger-ui.html |
| Database (PostgreSQL) | localhost:5432 |
