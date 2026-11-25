# KRD HANDY — лендинг

Статическая одностраничная страница без сборки. Источник — `index.html`, общие
стили — `style.css`. Сайт опубликован на GitHub Pages:
https://glebulllek.github.io/krdhandy_site/

## Состав

| Файл | Назначение |
|---|---|
| `index.html` | главная: hero, демо, конструктор, калькулятор, цены, безопасность, FAQ, документы |
| `style.css` | общие стили главной и юридических страниц, адаптив и доступность |
| `offer.html`, `rules.html`, `refund.html`, `privacy.html` | согласованные тексты из `docs_text.py` |
| `og-image.png` | обложка 1200×630 для `og:image` |
| `robots.txt`, `sitemap.xml` | SEO под Краснодар |

Юридические страницы генерируются, а не правятся руками:

```bash
cd ~/krdhandy_bot/krdhandy_bot
python3 scripts/generate_legal_pages.py
python3 scripts/generate_og_image.py
```

После правок скопируйте содержимое `landing/` в клон `glebulllek/krdhandy_site`
(сейчас `/data/data/com.termux/files/usr/tmp/opencode/krdhandy_site`) и запушьте
в `main`. Проверка совпадения:
`diff -rq ~/krdhandy_bot/krdhandy_bot/landing /data/data/com.termux/files/usr/tmp/opencode/krdhandy_site`
(игнорируйте только `.git/`).

## Воронки

| Кнопка | Deep link | Событие в боте |
|---|---|---|
| Создать заявку | `?start=src_site_customer` | `landing_click`, `landing_customer_click` |
| Найти работу | `?start=src_site_worker` | `landing_click`, `landing_worker_click` |
| Демо-карточки | `?start=demo_loader` и др. | `demo_open` |
| Черновик из конструктора | `?start=dr_<base32hex>` | `site_draft_start` + предзаполнение FSM |
| Лента/футер | `@krdhandy_orders`, `@krdhandy` | — |

Конструктор кодирует категорию, число работников, бюджет (uint24) и короткое
описание (до 28 байт UTF-8) в base32hex. Длина `start` не превышает 64 символов.
Телефон, адрес и дата в ссылку не попадают — их запрашивает бот. Декодер в
`bot.py:decode_draft_arg`.

## Важно

Лендинг не заменяет бота: вся сделка, чат, подтверждение и оплата комиссии
остаются в Telegram. Страница объясняет ценность и приводит в бота.
Этап 2 (лента заявок и профили через API) не реализован: в базе пока только
тестовые данные, показывать их как реальные запрещено.
