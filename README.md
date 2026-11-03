# Не очень странные дела — сайт-визитка

> 🚀 **Задеплоено:** https://strange-cases.vercel.app
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

## Деплой на Vercel

Публикация уже выполнена: проект `strange-cases` создан в Vercel, файлы загружены напрямую (static, framework = `None`, сборка не требуется), production-алиас `strange-cases.vercel.app` активен. GitHub-интеграция к Vercel **не подключена**, поэтому `git push` сам по себе сайт не обновляет.

### Обновление сайта вручную

```bash
git add . && git commit -m "Update site" && git push origin main
npx vercel@latest deploy --prod --yes --project strange-cases --token=<ВАШ_VERCEL_TOKEN>
```

### Если нужен автодеплой при каждом пуше

Два варианта:

1. **Vercel Dashboard** → Project → **Settings → Git** → Connect Git Repository (один раз подтвердить доступ к GitHub). После этого каждый push в `main` будет деплоиться сам.
2. **GitHub Actions**: добавить workflow, который на push выполняет `npx vercel deploy --prod --yes --token=${{ secrets.VERCEL_TOKEN }}`, и записать токен в секреты репозитория. Не требует подключения GitHub к Vercel.

### Как это было сделано (для повторения с нуля)

```bash
# 1. пустой публичный репозиторий на GitHub
# 2. в папке проекта:
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/free1monday/strange-cases.git
git push -u origin main

# 3. деплой (Vercel сам отдаёт static-файлы из корня, vercel.json применяется автоматически)
npx vercel@latest deploy --prod --yes --project strange-cases --create-if-missing --token=<TOKEN>
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

Токены GitHub и Vercel использовались **один раз** для создания репозитория, пуша и деплоя. В файлы проекта и в коммит они не попали (перед коммитом проверено `grep`), `/tmp`-файл с ними лежит вне репозитория. **Обязательно отзовите/перевыпустите оба токена** в настройках GitHub и Vercel: токен, побывавший в переписке, считается скомпрометированным. Для будущих деплоев выпустите новый токен и храните его в секретах CI или в менеджере паролей — никогда не коммитьте `.env`, ключи и токены.

## Чеклист

Полный чеклист по SEO и запуску: [SEO.md](SEO.md).
