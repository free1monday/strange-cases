# Не очень странные дела — сайт-визитка

> 🚀 **Задеплоено:** https://strange-cases.vercel.app
> 🔁 **Автодеплой:** Vercel ↔ GitHub — каждый push в `main` публикует продакшн
> **Репозиторий:** https://github.com/free1monday/strange-cases · `main` · 2026-11-03

Одностраничный лендинг юридической практики: чёрный фон, красные акценты, геометрический sans-serif, атмосфера «странных дел».
Название «Не очень странные дела» + подзаголовок «Решение странных дел и не очень», специализации, консультации бесплатно и ссылка на Telegram в нижнем блоке контактов. Имени и персональных данных на сайте нет.

Чистый HTML + CSS, один крошечный inline-скрипт для аналитики, никаких фреймворков и сборщиков.

## Структура

```
strange-cases/
├── index.html          # разметка, метатеги, OG, Schema.org (LegalService + WebSite), место под аналитику
├── styles.css          # все стили, адаптив, анимации, prefers-reduced-motion
├── robots.txt
├── sitemap.xml
├── vercel.json         # cleanUrls + заголовки безопасности и кэширования ассетов
├── assets/
│   ├── mark.svg        # красная эмблема «весы в кольце» (иконка сайта)
│   ├── favicon.png     # apple-touch-icon, 180×180
│   └── og-image.png    # превью для соцсетей, 1200×630, с новым названием
├── SEO.md              # чеклист по SEO
└── README.md           # этот файл
```

## Домен

Сайт доступен по адресу **https://strange-cases.vercel.app**, он же прописан в `canonical`, `og:url`, `og:image`, `twitter:image`, JSON-LD, `robots.txt` и `sitemap.xml`.

При подключении своего домена замените его везде:

```bash
grep -rn "strange-cases.vercel.app" .
```

Затем в Vercel: **Settings → Domains** → добавить домен и прописать DNS-записи, которые покажет панель. Не забудьте обновить `lastmod` в `sitemap.xml`.

## Локальный просмотр

Файл `index.html` можно просто открыть в браузере. Для корректной работы путей и заголовков лучше поднять локальный сервер:

```bash
cd strange-cases
python3 -m http.server 8080
# http://localhost:8080
```

Шрифты Montserrat и Orbitron подгружаются с Google Fonts. Без интернета сайт остаётся полностью рабочим благодаря системному fallback (`Segoe UI / Arial`). Orbitron не поддерживает кириллицу, поэтому он используется только для цифр и латинских подписей; весь кириллический текст — Montserrat.

## Деплой и автодеплой

Публикация выполнена: проект `strange-cases` создан в Vercel (static, framework = `None`, сборка не требуется), production-алиас `strange-cases.vercel.app` активен.

**Автодеплой включён нативно.** Проект подключён к репозиторию `free1monday/strange-cases`, production-ветка `main`. Каждый `push` в `main` сам создаёт новый деплой в Vercel и переключает алиас. Не нужны ни workflow-файлы, ни GitHub Actions, ни токен в CI: Vercel читает код напрямую через установленное на аккаунте GitHub-приложение (те же права уже использовались вашими проектами `lyukinfo`, `tiex-alfa`, `dark-forest-terra-incognita`).

### Обновление сайта

```bash
git add . && git commit -m "Update site" && git push origin main
```

Дальше всё сделает Vercel. История деплоев: https://vercel.com/moo26/strange-cases

### Ручной деплой (fallback, если Git-интеграция недоступна)

```bash
npx vercel@latest deploy --prod --yes --project strange-cases --token=<VERCEL_TOKEN>
```

Токен для этого сохранён в зашифрованном секрете репозитория `VERCEL_TOKEN` (Settings → Secrets and variables → Actions) — при нативном автодеплое он не используется, но оставлен по вашей просьбе как запасной вариант.

### Первоначальная настройка (для повторения с нуля)

```bash
# 1. публичный репозиторий на GitHub + папка проекта
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/free1monday/strange-cases.git
git push -u origin main

# 2. первый деплой прямой загрузкой файлов
npx vercel@latest deploy --prod --yes --project strange-cases --create-if-missing --token=<TOKEN>

# 3. подключить Git (в дашборде: Settings → Git → Connect Git Repository, либо через Vercel API)
```

Настройки проекта в Vercel: **Framework Preset** `Other`, **Build Command** пусто, **Output Directory** по умолчанию, переменные окружения не нужны.

## Аналитика

В конце `index.html` есть закомментированный блок с **Яндекс.Метрикой** и **GA4**. Включите нужный вариант, подставьте свой ID (`YM_COUNTER_ID` / `G-XXXXXXXXXX`) и раскомментируйте.

Клик по ссылке Telegram в блоке контактов уже обвешан хуками и не требует дополнительного кода:

- `window.dataLayer.push({ event: 'telegram_click' })`;
- `gtag('event', 'telegram_click', …)` — событие GA4;
- `ym(ID, 'reachGoal', 'TELEGRAM_CLICK')` — в Метрике создайте цель типа **JavaScript-событие** с идентификатором `TELEGRAM_CLICK`.

Пока счётчик не подключён, скрипт ничего не отправляет и не делает внешних запросов.

## Производительность

- 1 HTML + 1 CSS + 3 маленьких ассета (SVG + 2 PNG, суммарно ~85 КБ);
- иконки услуг — inline SVG, без дополнительных запросов;
- свечение, сетка и «пульс» — чистый CSS, без изображений и JS-анимаций;
- Google Fonts с `preconnect` и `display=swap`, системный fallback;
- внешние скрипты (аналитика) по умолчанию выключены;
- `vercel.json` кэширует `assets/` на год, заголовки `nosniff` и `Referrer-Policy` подтверждены на живом домене.

## Безопасность

Токены сохранены и используются по вашему указанию. Vercel-токен лежит в **зашифрованном секрете** `VERCEL_TOKEN` репозитория как запасной вариант для ручного деплоя; рабочий файл с обоими токенами находится в `/tmp` вне репозитория. В файлах проекта, в коммитах и в `git config` токенов нет — перед каждым коммитом это проверяется `grep`. При нативном автодеплое токены в CI вообще не участвуют. Если когда-либо решите перевыпустить токены, достаточно обновить этот секрет. Никогда не коммитьте `.env`, ключи и токены.

## Чеклист

Полный чеклист по SEO и запуску: [SEO.md](SEO.md).
