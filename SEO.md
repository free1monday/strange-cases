# SEO-чеклист — «Не очень странные дела»

> Сайт в проде: https://strange-cases.vercel.app · Репозиторий: https://github.com/free1monday/strange-cases · Автодеплой: Vercel ↔ GitHub, push в `main` → production

## Контент и метатеги

- [x] `Title`: «Не очень странные дела | Юрист в Екатеринбурге» — без имени, с брендом и городом.
- [x] `Description`: «Решение странных дел и не очень» + три услуги + бесплатные консультации + Telegram.
- [x] Ключевые слова вписаны **органично**, без `<meta name="keywords">` (поисковики его игнорируют):
  - «юрист Екатеринбург» — title, описание, подпись под логотипом, футер, `LegalService.address` / `areaServed`;
  - «банкротство физических лиц» — H3 карточки, description, `OfferCatalog`;
  - «семейный юрист» — подпись карточки «Семейные споры», description;
  - «защита прав потребителей» — H3 карточки, description, `OfferCatalog`.
- [x] Один `H1` — название в две строки «НЕ ОЧЕНЬ / СТРАННЫЕ ДЕЛА»; два `H2` («Специализация», «Консультации бесплатно» в блоке контактов), три `H3` — иерархия без пропусков.
- [x] Скрытый (`sr-only`) текст внутри H1 содержит подзаголовок и ключи для поисковиков, не загромождая дизайн.
- [x] `lang="ru"`, `charset="utf-8"`, `viewport`, `theme-color`, `color-scheme: dark`.
- [x] `canonical` = `https://strange-cases.vercel.app/`.
- [x] Текст лаконичный, без «воды», курсива и рукописных шрифтов.
- [x] Имени и персональных данных нет ни в контенте, ни в разметке (проверено на живом домене).

## Структурированные данные и соцсети

- [x] Schema.org `@graph`: `LegalService` (без `Person`) + `WebSite` со связью `publisher`.
- [x] В разметке: название, `slogan`, город (`areaServed`), адрес, `contactPoint` (Telegram), `sameAs`, `hasOfferCatalog` с тремя услугами, `priceRange` («первая консультация — бесплатно»).
- [x] Open Graph: `title`, `description`, `url`, `image` 1200×630, `image:alt`, `locale=ru_RU`, `site_name` = название бренда.
- [x] Twitter Card `summary_large_image`.
- [x] `og-image.png` перерисована под новое название, весит ~70 КБ, отдаётся с живого домена (200).
- [ ] Проверить сниппеты: [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/), Telegram/WhatsApp превью (нужен ваш аккаунт).
- [ ] Проверить разметку: [Schema Markup Validator](https://validator.schema.org/) и [Rich Results Test](https://search.google.com/test/rich-results).

## Техническое SEO

- [x] `robots.txt`: разрешено всё, указан `Sitemap`.
- [x] `sitemap.xml`: одна страница, `lastmod` 2026-11-03, `changefreq`, `priority`.
- [x] `vercel.json`: `cleanUrls`, `X-Content-Type-Options`, `Referrer-Policy`, `Permissions-Policy`, годовой кэш для `assets/` — заголовки подтверждены ответом сервера.
- [x] Оптимизированные `alt`: у единственного `<img>` осмысленный alt («Красная геометрическая эмблема: весы правосудия в тонком кольце»); у `og:image:alt` — описание карточки с новым названием; декоративные SVG скрыты через `aria-hidden="true"`.
- [x] `aria-labelledby` у секций, `skip-link`, видимый `:focus-visible`, поддержка `prefers-reduced-motion`.
- [x] Адаптив: 3 колонки → 1 колонка на ≤760 px, отдельные правила для длинного названия на ≤380 px, кнопка во всю ширину на мобильном.
- [x] Скорость: нет фреймворков, нет сборки, 1 CSS-файл, inline SVG, ~85 КБ ассетов, аналитика выключена по умолчанию.
- [x] Ссылка на Telegram только внизу, в блоке «Контакты», с `rel="noopener noreferrer"`.
- [x] Домен `https://strange-cases.vercel.app` подставлен во все метатеги, JSON-LD, `robots.txt` и `sitemap.xml` — совпадает с выданным Vercel production-алиасом.
- [x] HTTPS: Vercel выдал сертификат автоматически.
- [x] После деплоя проверены ответы `/`, `/styles.css`, `/robots.txt`, `/sitemap.xml`, `/assets/og-image.png` — все `200`.
- [x] Страница не закрыта от индекса: `noindex` нет, `robots.txt` разрешает.
- [x] Автодеплой включён нативно: проект Vercel привязан к `free1monday/strange-cases`, production-ветка `main`, push = новый деплой. Workflow-файлы и токены в CI не требуются.
- [ ] При подключении своего домена: заменить URL, настроить редирект `www` ↔ без `www`, обновить `lastmod`.
- [ ] Прогнать [PageSpeed Insights](https://pagespeed.web.dev/) (цель: 90+ по мобильным) и Lighthouse Accessibility.

## Индексация и аналитика

- [ ] Добавить сайт в [Google Search Console](https://search.google.com/search-console) и [Яндекс Вебмастер](https://webmaster.yandex.ru/), подтвердить права (через DNS/TXT у Vercel).
- [ ] Отправить `https://strange-cases.vercel.app/sitemap.xml` в обе панели, запросить переобход главной страницы.
- [ ] Подключить Яндекс.Метрику или GA4 (блок в конце `index.html`), создать цель `TELEGRAM_CLICK`.
- [ ] Через 2–4 недели посмотреть запросы и позиции; при необходимости добавить отдельные страницы под услуги (тогда расширить sitemap).
- [ ] Указать ссылку на сайт в профиле Telegram-канала/бота — это даёт живые внешние ссылки.

## Что сознательно не сделано

- `meta keywords` — устаревший тег, не влияет на ранжирование.
- Schema `Person` / имя / фото юриста — по правке «убрать имя», чтобы сайт работал как бренд.
- Блог, раздел «Обо мне», телефон, адрес офиса, формы заявки — по ТЗ «минимум текста, связь только через Telegram».
- GitHub Actions и `.github/workflows/*` — не понадобились: нативная Git-интеграция Vercel деплоит сама и не требует scope `workflow` у токена.
- Тяжёлые библиотеки, шрифтовые файлы в репозитории, JS-фреймворки, cookie-баннеры (аналитика подключается вручную и только по желанию).
