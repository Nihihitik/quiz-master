# Стадия сборки
FROM node:20-alpine AS builder

WORKDIR /app

# Копируем файлы зависимостей
COPY package*.json ./

# Устанавливаем зависимости
RUN npm ci

# Копируем остальные файлы
COPY . .

# Собираем проект
RUN npm run build

# Продакшн стадия
FROM node:20-alpine

WORKDIR /app

# Копируем файлы зависимостей
COPY package*.json ./

# Устанавливаем только продакшн зависимости
RUN npm ci --only=production

# Копируем собранные файлы из builder
COPY --from=builder /app/dist ./dist

# Копируем необходимые конфигурационные файлы
COPY drizzle.config.ts ./

# Открываем порт
EXPOSE 3001

# Запускаем приложение
CMD ["node", "dist/src/main.js"]
