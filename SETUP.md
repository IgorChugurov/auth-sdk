# Инструкция по настройке нового репозитория

## ✅ Что уже сделано

1. ✅ Все файлы скопированы из `packages/auth-sdk/` в новый репозиторий
2. ✅ Зависимости настроены правильно (все в peerDependencies)
3. ✅ Созданы `.gitignore` и `.npmignore`
4. ✅ Добавлен `LICENSE` (MIT)
5. ✅ Создан `DEVELOPMENT.md` с инструкциями по отладке

## 📋 Следующие шаги

### 1. Инициализация Git репозитория

```bash
cd /Users/igorchugurov/Documents/GitHub/OUR-pack/auth-sdk
git init
git add .
git commit -m "Initial commit: Auth SDK extracted from monorepo"
```

### 2. Создание GitHub репозитория

1. Создайте новый репозиторий на GitHub (например, `auth-sdk`)
2. Обновите `package.json` с правильным URL репозитория:

```json
{
  "repository": {
    "type": "git",
    "url": "https://github.com/your-org/auth-sdk.git"
  },
  "bugs": {
    "url": "https://github.com/your-org/auth-sdk/issues"
  },
  "homepage": "https://github.com/your-org/auth-sdk#readme"
}
```

3. Подключите remote:

```bash
git remote add origin https://github.com/your-org/auth-sdk.git
git branch -M main
git push -u origin main
```

### 3. Установка зависимостей

```bash
cd /Users/igorchugurov/Documents/GitHub/OUR-pack/auth-sdk
pnpm install
```

### 4. Первая сборка

```bash
pnpm build
```

Проверьте, что папка `dist/` создана и содержит все необходимые файлы.

### 5. Подключение к основному проекту

#### Вариант A: npm link (для разработки)

**В SDK репозитории:**
```bash
cd /Users/igorchugurov/Documents/GitHub/OUR-pack/auth-sdk
pnpm build
pnpm link --global
```

**В основном проекте:**
```bash
cd /Users/igorchugurov/Documents/GitHub/OUR-pack/axon-dashboard
pnpm link --global @axon-dashboard/auth-sdk
```

#### Вариант B: Local path (альтернатива)

В `package.json` основного проекта:
```json
{
  "dependencies": {
    "@axon-dashboard/auth-sdk": "file:../auth-sdk"
  }
}
```

Затем:
```bash
pnpm install
```

#### Вариант C: Публикация в npm (для production)

1. Войдите в npm:
```bash
npm login
```

2. Опубликуйте:
```bash
cd /Users/igorchugurov/Documents/GitHub/OUR-pack/auth-sdk
pnpm build
npm publish --access public
```

3. В основном проекте:
```bash
pnpm add @axon-dashboard/auth-sdk
```

### 6. Обновление импортов в основном проекте

После подключения SDK через npm/link, обновите импорты в основном проекте:

**Было:**
```typescript
import { ... } from "@/packages/auth-sdk/src/client";
```

**Стало:**
```typescript
import { ... } from "@axon-dashboard/auth-sdk/client";
```

## 🔧 Отладка

См. подробные инструкции в `DEVELOPMENT.md`:

- Отладка в монорепо (workspace links)
- Отладка в отдельном репозитории (npm link, local path)
- Troubleshooting

## 📦 Структура проекта

```
auth-sdk/
├── src/
│   ├── client/          # Клиентский модуль
│   ├── server/          # Серверный модуль
│   ├── components/      # UI компоненты
│   ├── utils/          # Утилиты
│   ├── types.ts        # Типы
│   └── errors.ts       # Классы ошибок
├── dist/               # Собранные файлы (генерируется)
├── package.json
├── tsconfig.json
├── tsup.config.ts
├── README.md
├── DEVELOPMENT.md
└── LICENSE
```

## ⚠️ Важно

1. **Все зависимости в peerDependencies** - они должны быть установлены в проекте, который использует SDK
2. **SDK требует Next.js** - использует Next.js специфичные функции (middleware, useRouter, useSearchParams)
3. **SDK требует Tailwind CSS** - компоненты используют Tailwind классы
4. **После изменений нужно пересобирать**: `pnpm build` или `pnpm dev` (watch mode)

## 🚀 Готово к использованию!

SDK готов к использованию. Следуйте инструкциям выше для подключения к основному проекту.

