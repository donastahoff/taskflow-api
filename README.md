# TaskFlow API

**REST API для управления задачами с JWT-авторизацией.**

Данный проект демонстрирует создание защищенного бэкенда на Java + Spring Boot. Включает регистрацию, логин, CRUD для задач и документацию через Swagger.

---

## 🛠️ Стек технологий

- **Java 21**
- **Spring Boot 3**
- **Spring Security (JWT)**
- **Spring Data JPA (Hibernate)**
- **PostgreSQL**
- **Swagger (OpenAPI 3)**
- **Maven**
- **Lombok**

---

## 📂 Архитектура

Проект построен по принципу **многослойной архитектуры**:

| Слой | Компонент | Ответственность |
|------|-----------|-----------------|
| **Controller** | `UserController`, `TaskController`, `AuthController` | Прием HTTP-запросов |
| **Service** | `UserService`, `TaskService`, `CustomUserDetailsService` | Бизнес-логика |
| **Repository** | `UserRepository`, `TaskRepository` | Работа с базой данных |
| **Security** | `JwtAuthenticationFilter`, `JwtTokenProvider`, `SecurityConfig` | JWT-аутентификация и авторизация |
| **DTO** | `AuthRequest`, `AuthResponse`, `TaskRequest`, `TaskResponse` | Обмен данными с клиентом |

---

## ⚡ Функционал

- ✅ Регистрация пользователя (`POST /api/users/register`)
- ✅ Логин и выдача JWT-токена (`POST /api/auth/login`)
- ✅ Создание задачи (`POST /api/tasks`) — только с токеном
- ✅ Получение всех задач (`GET /api/tasks`)
- ✅ Получение задачи по ID (`GET /api/tasks/{id}`)
- ✅ Обновление задачи (`PUT /api/tasks/{id}`)
- ✅ Удаление задачи (`DELETE /api/tasks/{id}`)
- ✅ Swagger-документация (`/swagger-ui.html`)

---

## 🚀 Запуск проекта

### Требования
- Java 21
- Maven 3.8+
- PostgreSQL (локально или через Docker)

### Шаги

1. **Клонируй репозиторий:**
   ```bash
   git clone https://github.com/donastahoff/taskflow-api.git
   cd taskflow-api
   ```

2. **Настрой базу данных PostgreSQL:**
   - Создай БД `taskflow_db`
   - Или используй Docker:
     ```bash
     docker run --name taskflow-postgres -e POSTGRES_PASSWORD=12345 -e POSTGRES_DB=taskflow_db -p 5432:5432 -d postgres:15
     ```

3. **Настрой `application.yml`:**
   - Создай файл `src/main/resources/application.yml`
   - Укажи свои параметры (пример ниже)

4. **Запусти приложение:**
   ```bash
   mvn clean install
   mvn spring-boot:run
   ```

5. **Открой Swagger:**  
   [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)

---

## 📄 Пример `application.yml`

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/taskflow_db
    username: postgres
    password: 12345
    driver-class-name: org.postgresql.Driver

  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        format_sql: true

jwt:
  secret: mySuperSecretKey12345678901234567890  # минимум 32 символа!
  expiration: 3600000                           # 1 час
```

---

## 📮 Примеры запросов (Postman)

### 1. Регистрация
**POST** `/api/users/register`
```json
{
  "email": "test@mail.com",
  "password": "12345",
  "firstName": "Иван",
  "lastName": "Иванов"
}
```

### 2. Логин (получение токена)
**POST** `/api/auth/login`
```json
{
  "email": "test@mail.com",
  "password": "12345"
}
```

**Ответ:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9...",
  "email": "test@mail.com",
  "firstName": "Иван",
  "role": "USER"
}
```

### 3. Создание задачи (с токеном)
**POST** `/api/tasks`  
**Header:** `Authorization: Bearer <токен>`
```json
{
  "title": "Моя первая задача",
  "description": "Описание",
  "status": "TODO"
}
```

---

## 📂 Структура проекта

```
src/main/java/com/example/demo/
├── config/
│   ├── JwtAuthenticationFilter.java
│   ├── JwtTokenProvider.java
│   └── SecurityConfig.java
├── controller/
│   ├── AuthController.java
│   ├── TaskController.java
│   └── UserController.java
├── dto/
│   ├── AuthRequest.java
│   ├── AuthResponse.java
│   ├── TaskRequest.java
│   └── TaskResponse.java
├── model/
│   ├── Task.java
│   ├── TaskStatus.java
│   └── User.java
├── repository/
│   ├── TaskRepository.java
│   └── UserRepository.java
├── service/
│   ├── CustomUserDetailsService.java
│   └── TaskService.java
└── DemoApplication.java
```

---

## 📌 Планы по развитию

- [ ] Добавить пагинацию и сортировку
- [ ] Добавить роли (ADMIN, USER)
- [ ] Добавить комментарии к задачам
- [ ] Интеграция с Telegram-уведомлениями
- [ ] Деплой на сервер
