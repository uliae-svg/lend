# НавесПро — История разработки лендинга

**Живой сайт:** https://lend-tau-sage.vercel.app
**GitHub:** https://github.com/uliae-svg/lend.git
**Файл:** `index.html` (HTML + CSS + JS в одном файле, без фреймворков)

---

## Структура страницы

1. **Hero** — заголовок, статистика (847 проектов, 25 лет гарантия, 1–3 дня монтаж), CTA
2. **Pain** — "Узнаёте себя?" — боли клиентов
3. **UTP** — bento-карточки с преимуществами
4. **Portfolio** — проекты с фильтрами по категориям
5. **Process** — 6 шагов от звонка до готового навеса
6. **Reviews** — отзывы, рейтинг 4.9
7. **Calculator** — пошаговый калькулятор стоимости
8. **Guarantees** — гарантии и команда
9. **Final CTA** — форма + WhatsApp/Telegram
10. **Footer** + фиксированная мобильная нижняя панель

---

## Этапы дизайна

### 1. Earth Tones (шалфей + терракот)
Первая версия. Отзыв: *"не выглядит как премиум"*.

### 2. Charcoal + Gold
Тёмная палитра + золото. Отзыв: *"дизайн тоже не нравится"*.

### 3. True Black + Gold — текущий ✓
Принципы люкс-дизайна:
- Фон `#080808` — настоящий чёрный, не тёмно-серый
- Акцент только золото `#C49830` — никаких других цветов
- Острые углы (2–12px) — скруглённые = масс-маркет
- Шрифт display: **Cormorant Garamond** (элегантная антиква)
- Шрифт body: **Syne** (архитектурный, заменил Inter)
- Заголовок `clamp(3rem, 9vw, 11rem)` — крупная типографика

#### Добавленные премиум-детали
| Деталь | Описание |
|---|---|
| Grain texture | `body::after` с SVG-шумом, `mix-blend-mode: overlay`, opacity 0.04 |
| Кастомный курсор | Золотая точка + кольцо с lag-эффектом (только десктоп) |
| Gold ticker | Бегущая строка с преимуществами между hero и следующим блоком |
| Hero левый | Текст выравнен влево на экранах 1025px+ |
| Акцент в заголовке | "любимым местом" — золотой цвет + подчёркивание, не курсив |

---

## Решённые баги

### Мобильная версия
| Проблема | Причина | Решение |
|---|---|---|
| Блоки не видны (только 1-й и последний) | `.anim { opacity: 0 }` применялось глобально | Перенесли блок анимации внутрь `@media (min-width: 769px)` |
| Кнопки не кликались | `overflow-x: hidden` баг iOS Safari | Заменили на `overflow-x: clip` + `pointer-events: none` на фоновых слоях |
| Трясучка при смене направления скролла | URL-бар iOS показывается/прячется, меняет `100svh` | JS фиксирует `--real-vh = window.innerHeight` один раз при загрузке |
| Скролл не плавный | `scroll-behavior: smooth` глобально ломает momentum scroll | Перенесли в `@media (min-width: 769px)` |
| Кнопка нижней панели — нужно 2 тапа | iOS фокусирует `<button>` на первый тап | Добавили `touchend` listener с `preventDefault()` |
| Зелёный квадрат в нижней панели | Кнопка WhatsApp с `background: #25D366` | Изменили на прозрачную с белой рамкой |

### Десктоп
| Проблема | Причина | Решение |
|---|---|---|
| Статистика перекрывала CTA-кнопку | `hero-stats` на `position: absolute; bottom: 40px` | Рефакторинг: stats стали частью flex-потока, `position: relative` |
| Перекрытие на разных высотах экрана | `margin-bottom: 120px` не всегда хватало | Полный рефакторинг hero: `.hero-content { flex: 1 }`, stats снизу |

---

## Деплой

```
GitHub: uliae-svg/lend (ветка main)
    ↓ автодеплой
Vercel: lend-tau-sage.vercel.app
```

Конфиг `vercel.json`:
```json
{
  "version": 2,
  "builds": [{ "src": "index.html", "use": "@vercel/static" }],
  "routes": [{ "src": "/(.*)", "dest": "/index.html" }]
}
```

---

## Инфраструктура

- **Node.js** — потребовалось изменить политику PowerShell:
  ```
  Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
  ```
- **Скилы Claude Code:** `find-skills`, `frontend-design` (из `anthropics/claude-code`)

---

## CSS переменные

```css
--matte-black: #080808    /* фон тёмных секций */
--charcoal:    #111111
--cream:       #F8F6F2    /* фон светлых секций */
--gold:        #C49830    /* единственный акцент */
--gold-light:  #D4AA4A
--warm-gray:   #6B6560

--font-display: 'Cormorant Garamond', Georgia, serif
--font-body:    'Syne', system-ui, sans-serif

--radius-sm:   2px
--radius-md:   4px
--radius-lg:   8px
--header-h:    72px
```
