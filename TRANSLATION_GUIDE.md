# Руководство по переводу (Translation guide)

Этот репозиторий — русский перевод сайта [The Missing Semester of Your CS Education](https://missing.csail.mit.edu/)
(MIT, выпуск 2026 года). Цель — **точная копия оригинального сайта 1:1**: та же структура файлов,
те же URL, те же видео, тот же смысл, но по-русски. Ниже правила, которых придерживаются все
переводчики (люди и агенты). Изменения глоссария вносятся сюда же.

## 1. Что переводить, что не трогать

**Переводится:**
- весь текст лекций, упражнений, страниц `index.md`, `about.md`, `past.md`, `license.md`, `404.html`;
- `title:` и `description:` во front matter;
- строки интерфейса в `_layouts/`, `_includes/`, `static/js/sidenotes.js`;
- текст ссылок (`[так](...)`), заголовки, подписи, `alt` у картинок;
- текстовые пояснения внутри blockquote-заметок (`> ...`).

**Не переводится и остаётся байт-в-байт:**
- содержимое блоков кода (```` ``` ````) целиком, включая комментарии, вывод команд и промпты;
- inline-код в обратных кавычках (`` `git commit` ``, `` `<file>` ``), кроме случаев, когда это
  очевидно английская фраза-плейсхолдер в тексте (редко; тогда переводится и помечается в PR);
- URL, якоря (`#...`), пути файлов, идентификаторы видео, поля front matter кроме `title`/`description`;
- Liquid-теги `{% ... %}` и `{{ ... }}` (текст между тегами переводится, сами теги — нет);
- имена людей, названия инструментов/компаний/продуктов (Git, GitHub, Vim, Claude Code, Homebrew,
  uv, tmux, ssh, zsh…), названия лицензий, флаги команд;
- HTML-теги и их атрибуты (кроме видимого текста внутри и `alt`).

Структура должна совпадать: тот же порядок и уровни заголовков, то же число абзацев, списков,
элементов списков, blockquote'ов, тот же набор ссылок. Ничего не выбрасывать и не добавлять
«от себя» — ни примечаний переводчика, ни дополнительных примеров.

## 2. Технические ловушки

- **Blockquote → sidenote.** `static/js/sidenotes.js` превращает каждый `> ...` в боковую заметку.
  Если текст blockquote'а **начинается** с `"` или `“`, он становится эпиграфом и делится на
  цитату/автора по **последнему** ` — ` (пробел, длинное тире, пробел). Поэтому:
  - у эпиграфов сохраняем открывающую прямую кавычку `"…"` (не «ёлочки»!) и ровно один ` — ` перед автором;
  - обычная заметка не должна начинаться с кавычки; внутри заметок ` — ` можно использовать свободно.
- **Кавычки в прозе:** русские «ёлочки»; вложенные — „лапки“. В коде — как в оригинале.
- **Тире:** в прозе длинное тире `—` с пробелами (`слово — слово`); дефис `-` только внутри слов.
  Диапазоны: `1–2` (короткое тире) допустимо, но чаще пишем `1-2` как в оригинале, если это код или число.
- **Ссылки-якоря на заголовки** внутри сайта (`/2026/version-control/#anchor`) в оригинале почти не
  используются, но если встретятся — якорь оставить английским: kramdown генерирует id из
  заголовка, поэтому переведённый заголовок ломает якорь. В таком случае добавить явный id:
  `## Заголовок {#original-anchor}`.
- Не менять переносы внутри блоков кода и не «чинить» их. Мягкие переносы строк в прозе
  (оригинал переносит строки на ~72 символах) — переносить примерно так же, чтобы diff'ы были читаемы.
- Markdown-эмфаза `_так_`/`**так**` сохраняется на тех же словах по смыслу.
- Не переводить `[no class]`/`[coming soon]` внутри Liquid-логики index-страниц иначе, чем
  задано ниже (см. глоссарий UI).

## 3. Стиль

- Обращение к читателю — на «вы» (строчная буква), как принято в технической документации.
- Тон — живой, разговорный, как в оригинале, но без просторечия. Юмор и отступления оригинала
  сохраняем, не сглаживаем.
- Предложения не «калькировать»: перестроить фразу по-русски, но не терять ни одной мысли.
- Устоявшиеся англицизмы IT-среды допустимы (коммит, репозиторий, пул-реквест, деплой, линтер),
  но при первом появлении редкого термина в лекции даём английский оригинал в скобках:
  «упаковка (packaging)».
- Названия команд/утилит в тексте — как в оригинале, в обратных кавычках если они там были.
- «Вы» с заглавной не используем. Букву «ё» пишем везде.
- Не переводить междометия в коде/промптах. В примерах промптов к ИИ (внутри блоков кода) — оставить английский.

## 4. Глоссарий терминов (обязателен к соблюдению)

| English | Русский |
|---|---|
| The Missing Semester of Your CS Education | Пропущенный семестр вашего CS-образования |
| Missing Semester (короткое) | Пропущенный семестр (в навигации/шапке остаётся `./missing-semester`) |
| lecture / lecture notes | лекция / конспект лекции |
| exercises | упражнения |
| shell | командная оболочка (в первый раз: «командная оболочка (shell)»), далее допустимо «оболочка» или «shell» |
| terminal / terminal emulator | терминал / эмулятор терминала |
| command line / command-line | командная строка / (прил.) командной строки |
| prompt (shell prompt) | приглашение командной строки |
| prompt (LLM prompt) | промпт |
| command | команда |
| argument / flag / option | аргумент / флаг / опция |
| pipe / pipeline | конвейер (pipe) / конвейер команд |
| redirection / stream / stdin / stdout / stderr | перенаправление / поток / stdin / stdout / stderr |
| environment variable | переменная окружения |
| working directory | рабочий каталог |
| directory / folder | каталог (допустимо «папка», если так в оригинале «folder») |
| file descriptor | файловый дескриптор |
| process / job / background job | процесс / задание / фоновое задание |
| signal | сигнал |
| exit code / exit status | код возврата |
| script / shell script | скрипт / shell-скрипт |
| dotfiles | dotfiles (файлы конфигурации, «дотфайлы» — не использовать) |
| alias | алиас (псевдоним) |
| remote machine / remote | удалённая машина / удалённый (репозиторий) |
| port forwarding | проброс портов |
| package / package manager | пакет / менеджер пакетов |
| packaging | упаковка (packaging) |
| dependency / lock file | зависимость / lock-файл |
| virtual environment | виртуальное окружение |
| build / build system | сборка / система сборки |
| deploy / deployment | развернуть, деплой / развёртывание |
| ship (code) | доставлять (код пользователям), выпускать |
| release / versioning / semantic versioning | релиз / версионирование / семантическое версионирование |
| container / image | контейнер / образ |
| version control / VCS | контроль версий / система контроля версий (VCS) |
| repository | репозиторий |
| commit (n./v.) | коммит / закоммитить (в формальном контексте — «зафиксировать изменения») |
| branch / merge / rebase / merge conflict | ветка / слияние (merge) / rebase / конфликт слияния |
| staging area / to stage | индекс (staging area) / добавить в индекс |
| snapshot | снимок |
| pull request (PR) | пул-реквест (PR) |
| upstream / fork | upstream / форк |
| blame / bisect / stash | git blame / git bisect / stash (не переводить названия подкоманд) |
| debugging / debugger / breakpoint | отладка / отладчик / точка останова |
| profiling / profiler | профилирование / профилировщик |
| tracing / logging / log | трассировка / логирование / лог |
| stack trace | трассировка стека |
| benchmark | бенчмарк |
| flame graph | flame graph (флейм-график) |
| linting / linter / formatter | линтинг / линтер / форматтер |
| test / unit test / integration test | тест / модульный (юнит-)тест / интеграционный тест |
| continuous integration (CI) | непрерывная интеграция (CI) |
| code review | код-ревью (ревью кода) |
| editor / IDE | редактор / IDE |
| language server (LSP) | языковой сервер (LSP) |
| plugin / extension | плагин / расширение |
| autocomplete / completion | автодополнение |
| keybinding / shortcut | сочетание клавиш |
| agentic coding | агентное программирование (agentic coding) |
| coding agent / AI agent | агент для программирования / ИИ-агент |
| LLM / model | LLM / модель |
| context (window) | контекст (контекстное окно) |
| tool call / tool use | вызов инструмента |
| hallucination | галлюцинация |
| open source / maintainer / contributor | open source (открытый исходный код) / мейнтейнер / контрибьютор |
| issue | issue (обращение в трекере) — не переводить как «проблема» |
| documentation / docs / README | документация / README |
| soft skills | «мягкие» навыки (soft skills) |
| etiquette | этикет |
| Q&A | вопросы и ответы |
| homework / assignment | домашнее задание |
| cheat sheet | шпаргалка |
| tab completion | автодополнение по Tab |
| man page | man-страница |
| root / superuser | root / суперпользователь |
| permission(s) | права доступа |
| symlink | символическая ссылка (симлинк) |
| wildcard / globbing | wildcard / подстановка (globbing) |
| regular expression (regex) | регулярное выражение |
| verbose | подробный (режим) |
| boilerplate | шаблонный код |
| workflow | рабочий процесс (в GitHub Actions — workflow) |
| best practices | лучшие практики |
| trade-off | компромисс |
| overhead | накладные расходы |
| production | продакшен |
| edge case | пограничный случай |
| sanity check | проверка на здравый смысл |
| rule of thumb | эмпирическое правило |
| blob / tree (Git objects) | blob / tree («дерево») — латиницей, пояснение при первом упоминании |
| leaky abstraction | «дырявая» абстракция (leaky abstraction) |
| tutorial | туториал |
| session (terminal/tmux) | сессия |
| pinning (versions) | закрепление (pinning) версий |
| hot spot (profiling) | горячая точка (hot spot) |
| binding (language binding) | привязка (binding) |

### Названия лекций 2026 (использовать везде одинаково)

| Файл | English | Русский |
|---|---|---|
| course-shell | Course Overview + Introduction to the Shell | Обзор курса + введение в командную оболочку |
| command-line-environment | Command-line Environment | Среда командной строки |
| development-environment | Development Environment and Tools | Среда разработки и инструменты |
| debugging-profiling | Debugging and Profiling | Отладка и профилирование |
| version-control | Version Control and Git | Контроль версий и Git |
| shipping-code | Packaging and Shipping Code | Упаковка и доставка кода |
| agentic-coding | Agentic Coding | Агентное программирование |
| beyond-code | Beyond the Code | За пределами кода |
| code-quality | Code Quality | Качество кода |

### Названия лекций 2020

| Файл | English | Русский |
|---|---|---|
| course-shell | Course Overview + The Shell | Обзор курса + командная оболочка |
| shell-tools | Shell Tools and Scripting | Инструменты оболочки и скрипты |
| editors | Editors (Vim) | Редакторы (Vim) |
| data-wrangling | Data Wrangling | Обработка данных |
| command-line | Command-line Environment | Среда командной строки |
| version-control | Version Control (Git) | Контроль версий (Git) |
| debugging-profiling | Debugging and Profiling | Отладка и профилирование |
| metaprogramming | Metaprogramming | Метапрограммирование |
| security | Security and Cryptography | Безопасность и криптография |
| potpourri | Potpourri | Попурри |
| qa | Q&A | Вопросы и ответы |

### Названия лекций 2019

| Файл | English | Русский |
|---|---|---|
| course-overview | Course Overview | Обзор курса |
| virtual-machines | Virtual Machines and Containers | Виртуальные машины и контейнеры |
| shell | Shell and Scripting | Командная оболочка и скрипты |
| command-line | Command-line environment | Среда командной строки |
| data-wrangling | Data Wrangling | Обработка данных |
| editors | Editors | Редакторы |
| version-control | Version Control | Контроль версий |
| dotfiles | Dotfiles | Dotfiles |
| backups | Backups | Резервные копии |
| automation | Automation | Автоматизация |
| machine-introspection | Machine Introspection | Интроспекция машины |
| program-introspection | Program Introspection | Интроспекция программ |
| package-management | Package Management and Dependency Management | Управление пакетами и зависимостями |
| os-customization | OS Customization | Настройка ОС |
| remote-machines | Remote Machines | Удалённые машины |
| web | Web and Browsers | Веб и браузеры |
| security | Security and Privacy | Безопасность и приватность |

### Строки интерфейса

| English | Русский |
|---|---|
| lectures (nav) | лекции |
| past (nav) | прошлые годы |
| about (nav) | о курсе |
| Edit this page | Редактировать страницу |
| Licensed under CC BY-NC-SA | Лицензия CC BY-NC-SA |
| Source code | Исходный код |
| [no class] | [занятия нет] |
| [coming soon] | [скоро] |
| Lecture video coming soon! | Видео лекции скоро появится! |
| note N (sidenote label) | заметка N |
| Back to note N | К заметке N |
| Past Offerings | Прошлые выпуски |
| 2026 Lectures | Лекции 2026 |
| Redirecting you to | Перенаправляем на |
| 404: Page not found | 404: страница не найдена |

## 5. Как проверять

`tools/verify_md.py <оригинал> <перевод>` сравнивает структуру: front matter, блоки кода
(должны совпадать байт-в-байт), ссылки, Liquid-теги, заголовки, blockquote'ы, сноски, HTML-теги.
Ошибок быть не должно; предупреждения — просмотреть глазами.
