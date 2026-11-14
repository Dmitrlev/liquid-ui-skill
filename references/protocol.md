# Протокол хоста: где лежат файлы и как менять

## Проводка (не трогать)

- `likvid-ui_dev/src/App.tsx` — таб «Тест UI» (`TEST_TAB`, `page === 'test' && <TestSection />`) уже подключён.
- `likvid-ui_dev/src/sections/TestSection.tsx` — рендерит `<Mockup />` из `../mockups/Mockup`, подпись и хост `.test-host`.
- `likvid-ui_dev/src/app.css` — `.test-host { width: 100% }`: хост растянут, ширину и раскладку диктует сам макет.
- Entry хоста (`main.tsx`) подключает `import 'likvid-ui/theme.css'` до проектовых стилей; свои переопределения токенов — отдельный CSS-файл ниже импорта (см. `kit-rules.md`, «Подключение стилей кита»).

Если `TestSection` вдруг ссылается на другой путь — чинить ссылку, а не переносить макет.

## Замена макета до корня

1. `rm -rf /workspace/likvid-ui_dev/src/mockups`
2. `mkdir -p /workspace/likvid-ui_dev/src/mockups`
3. Написать заново ровно три файла:
   - `Mockup.tsx` — корневой `export const Mockup = memo(() => ...)` + `Mockup.displayName = 'Mockup'`. Имя и путь фиксированы — на них ссылается TestSection.
   - `Mockup.css` — импортируется из `Mockup.tsx` (`import './Mockup.css'`).
   - `icons.tsx` — инлайн-SVG-иконки, которые использует макет.
4. Никаких четвёртых файлов и соседних «старых» макетов.

## Конвенции макета

- **Префикс классов** — новый, по теме макета (`gt-` для Google Translate, `sp-` для Spotify и т.д.). Весь CSS-модуль живёт в `Mockup.css` и не выходит за префикс.
- **Корень макета** — `<div className="pfx-screen">`, на нём локальные `--color-main`/`--color-main-dark` под палитру оригинала, обязательное зеркало `--likvid-color-main: var(--color-main)` для компонентов либы и `font-family` при необходимости.
- **Секции макета** — как на входе: header, режимы, панель, футер. Порядок DOM = порядок на скрине.
- **Интерактив** — минимально живой: `useState` для выбранного таба/режима, `maxLength` и счётчик для инпутов, swap-кнопка меняет местами выбор. Заглушки данных — константами на уровне файла.

## Порядок записи

CSS → icons → Mockup. Так компонент пишет уже зная готовые классы, а не переписывается после стилизации.
