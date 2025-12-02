## <a id = "конфигурация">⚙️ Конфигурация </a>

## 📋 Содержание

- [Основные файлы конфигураци](#основные-файлы-конфигурации)
- [Профиль разработки (dev)](#разработка-dev-профиль)
- [Продакшен профиль (prod)](#продакшен-prod-профиль)
- [Конфигурация ML сервиса](#конфигурация-ml-сервиса)

<a id="Файлы конфигурации Spring Boot">Основные файлы конфигурации</a>

### Файлы конфигурации Spring Boot

**application.yml** (основная конфигурация):

```yaml
spring:
  application:
    name: spring-boot-kotlin-STT
  datasource:
    url: ${DATABASE_URL:jdbc:postgresql://localhost:5432/student_themes}
    username: ${DATABASE_USERNAME:postgres}
    password: ${POSTGRES_PASSWORD}
    driver-class-name: org.postgresql.Driver
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
  jpa:
    open-in-view: false
    properties:
      hibernate:
        jdbc.batch_size: 20
        order_inserts: true
        order_updates: true

logging:
  level:
    com.StudentsToThemes.spring_boot_kotlin_STT: DEBUG
    org.springframework.web: INFO
    org.hibernate: WARN
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss} - %logger{36} - %msg%n"
  file:
    name: "logs/application.log"

server:
  port: ${PORT:8080}
  servlet:
    context-path: /api

management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics
```

<a id="разработка-dev-профиль">Разработка (dev профиль)</a>
**application-dev.yml** (разработка):

```yaml
spring:
  application:
    version: 1.0.0-dev
  jpa:
    hibernate:
      ddl-auto: update
    properties:
      hibernate:
        show_sql: true
        format_sql: true
        use_sql_comments: true
  # В DEV отключаем Flyway - пусть Hibernate управляет схемой
  flyway:
    enabled: false

logging:
  level:
    org.hibernate.SQL: DEBUG
    org.hibernate.type.descriptor.sql.BasicBinder: TRACE
```

<a id="продакшен-prod-профиль">Продакшен (prod)</a>
**application-prod.yml** (продакшен):

```yaml
spring:
  application:
    version: 1.0.0-prod
  jpa:
    hibernate:
      ddl-auto: validate
    properties:
      hibernate:
        show_sql: false
        format_sql: false
  # В PROD включаем Flyway для управления миграциями
  flyway:
    enabled: true
    locations: classpath:db/migration
    baseline-on-migrate: true

logging:
  level:
    com.StudentsToThemes.spring_boot_kotlin_STT: INFO
    org.springframework: WARN
```

<a id="конфигурация-ml-сервиса">Конфигурация ML сервиса</a>

### Конфигурация ML сервиса

ML сервис можно кастомизировать через параметры:

```python
# В файле main.py можно изменить:
matcher = CSVStudentTopicMatcher(
    model_name='sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2'  # Модель по умолчанию
)

# Доступные модели:
# - 'sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2' (рекомендуется)
# - 'sentence-transformers/paraphrase-multilingual-mpnet-base-v2' (больше точность, больше памяти)
# - 'sentence-transformers/all-MiniLM-L6-v2' (быстрее, меньше памяти)
```
