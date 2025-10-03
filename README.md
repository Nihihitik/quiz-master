# Quiz Master

Fullstack приложение на Next.js (frontend) и NestJS (backend) с PostgreSQL.

## Структура проекта

```
quiz-master/
├── frontend/          # Next.js приложение
│   ├── src/
│   ├── Dockerfile
│   └── docker-compose.yml
├── backend/           # NestJS приложение
│   ├── src/
│   ├── Dockerfile
│   └── docker-compose.yml
└── docker-compose.yml # Общий docker-compose
```

## Технологии

### Frontend
- Next.js 15 с TypeScript
- Tailwind CSS
- shadcn/ui компоненты
- Docker support

### Backend
- NestJS с TypeScript
- DrizzleORM
- PostgreSQL
- Docker support

## Быстрый старт

### Запуск всего проекта с Docker

Из корневой директории проекта:

```bash
# Запуск всех сервисов (PostgreSQL + Backend + Frontend)
docker-compose up -d

# Просмотр логов
docker-compose logs -f

# Остановка всех сервисов
docker-compose down
```

После запуска:
- Frontend: http://localhost:3000
- Backend: http://localhost:3001
- PostgreSQL: localhost:5432

### Локальная разработка

#### Backend

```bash
cd backend

# Установка зависимостей
npm install

# Запуск PostgreSQL
docker-compose up postgres -d

# Применение схемы БД
npm run db:push

# Запуск в режиме разработки
npm run start:dev
```

Backend будет доступен на http://localhost:3001

#### Frontend

```bash
cd frontend

# Установка зависимостей
npm install

# Запуск в режиме разработки
npm run dev
```

Frontend будет доступен на http://localhost:3000

## Работа с базой данных

### Drizzle Studio

Drizzle Studio - веб-интерфейс для просмотра и управления базой данных:

```bash
cd backend
npm run db:studio
```

Откроется по адресу: https://local.drizzle.studio

### Команды Drizzle

```bash
cd backend

# Генерация миграций из схемы
npm run db:generate

# Применение миграций
npm run db:migrate

# Push схемы напрямую в БД (для разработки)
npm run db:push
```

## Добавление компонентов shadcn/ui

```bash
cd frontend

# Добавление отдельного компонента
npx shadcn@latest add button
npx shadcn@latest add card
npx shadcn@latest add dialog

# Список доступных компонентов
npx shadcn@latest add
```

## Переменные окружения

### Backend (.env)

```env
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=postgres
DB_NAME=quiz_master
PORT=3001
```

### Frontend (.env.local)

```env
NEXT_PUBLIC_API_URL=http://localhost:3001
```

## Docker команды

### Запуск отдельных сервисов

```bash
# Только PostgreSQL
docker-compose up postgres -d

# Только Backend
docker-compose up backend -d

# Только Frontend
docker-compose up frontend -d
```

### Пересборка контейнеров

```bash
# Пересборка всех сервисов
docker-compose up --build -d

# Пересборка конкретного сервиса
docker-compose up --build backend -d
```

### Просмотр логов

```bash
# Все сервисы
docker-compose logs -f

# Конкретный сервис
docker-compose logs -f backend
docker-compose logs -f frontend
docker-compose logs -f postgres
```

### Очистка

```bash
# Остановка и удаление контейнеров
docker-compose down

# Удаление с volumes (данные БД будут удалены!)
docker-compose down -v
```

## Полезные ссылки

- [Next.js Documentation](https://nextjs.org/docs)
- [NestJS Documentation](https://docs.nestjs.com)
- [DrizzleORM Documentation](https://orm.drizzle.team)
- [shadcn/ui Documentation](https://ui.shadcn.com)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)

## Разработка

### Структура базы данных

Схемы таблиц определены в `backend/src/db/schema.ts`. После изменения схем:

```bash
cd backend
npm run db:push  # для разработки
# или
npm run db:generate && npm run db:migrate  # для продакшн
```

### API интеграция

Frontend может обращаться к Backend API через переменную окружения `NEXT_PUBLIC_API_URL`.

Пример использования:

```typescript
const response = await fetch(`${process.env.NEXT_PUBLIC_API_URL}/api/endpoint`);
```

## Troubleshooting

### Порты заняты

Если порты 3000, 3001 или 5432 уже заняты, измените их в соответствующих docker-compose.yml файлах:

```yaml
ports:
  - '3001:3001'  # измените на '3002:3001' например
```

### Проблемы с подключением к БД

Убедитесь, что PostgreSQL запущен:

```bash
docker-compose ps postgres
```

Проверьте логи:

```bash
docker-compose logs postgres
```

### Очистка и пересборка

Если что-то работает некорректно:

```bash
# Остановите все контейнеры
docker-compose down

# Удалите node_modules и пересоберите
cd backend && rm -rf node_modules && npm install
cd ../frontend && rm -rf node_modules && npm install

# Пересоберите Docker образы
docker-compose up --build -d
```
