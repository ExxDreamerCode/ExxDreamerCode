# Даниил

Rust и Python. В основном шахматный софт.

[Ember](#ember) · [DeepSight](#deepsight) · [Aurora Player](#aurora-player) · [Hollow Knight AI](#hollow-knight-ai) · [Контакты](#контакты)

## Проекты

| | Проект | Что это | На чём | В цифрах |
| :-: | --- | --- | --- | --- |
| 1 | **[Ember](https://github.com/ExxDreamerCode/Ember)** | Шахматный движок с протоколом UCI | Rust — 31 тыс. строк, 57 файлов | CCRL Blitz **3381 ± 21** на 575 партиях |
| 2 | **[DeepSight](https://github.com/ExxDreamerCode/DeepSight)** | Анализатор партий с графическим интерфейсом | Python 3.11, PyQt6 | Ember и Stockfish внутри сборки |
| 3 | **[Aurora Player](https://github.com/ExxDreamerCode/AuroraPlayer)** | IPTV-плеер для Windows | Tauri 2, Rust, React 19, hls.js | M3U на входе, список каналов на выходе: группы, поиск, PiP |
| 4 | **[Hollow Knight AI](https://github.com/ADIMIR21/Hollow-Knight-Bot)** | Агент на PPO учится драться с боссами Пантеона | Python + Stable-Baselines3, мод на C# | 19 действий, телеметрия на ~60 Гц |

### Ember

<sub>Шахматный движок UCI · [репозиторий](https://github.com/ExxDreamerCode/Ember) · [релизы](https://github.com/ExxDreamerCode/Ember/releases/latest) · [BUILD.md](https://github.com/ExxDreamerCode/Ember/blob/main/BUILD.md)</sub>

Ember играет на CCRL: **Blitz 3381 ± 21** (575 партий), **40/15 3277** (6 партий), **FRC 3200 ± 21** — 86-е место. Три списка между собой не сравнимы.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/ember-rating-dark.png">
  <img alt="Рост рейтинга Ember от 0.9.1 до 1.3.1: залитые точки с усами погрешности — измеренные рейтинги CCRL Blitz, полые точки на пунктире — неоценённые версии" src="assets/ember-rating-light.png" width="860">
</picture>

- **NNUE** — свои сети в двух форматах: V1 — половинные признаки с кинг-бакетами, необязательными threat-признаками и активациями `CReLU`/`SCReLU`/pairwise; V2 — полу-трансформер со стеками PSQ- и threat-функций. Компактное хранение `ECN1`; обучение на PyTorch- и Bullet-тулинге из репозитория.
- **Поиск** — quiescence-поиск с SEE и дельта-отсечением, транспозиционная таблица, killer/history/counter moves, Lazy SMP, таблицы Syzygy до шести фигур, книги Polyglot со встроенной по умолчанию.
- **Сборка** — закреплённый nightly-тулчейн, воспроизводимые Nix-сборки, PGO-данные, портативная сборка под Windows и команда `bench`, которая выдаёт воспроизводимую подпись по числу нод.

### DeepSight

<sub>Десктопный анализатор шахматных партий · [репозиторий](https://github.com/ExxDreamerCode/DeepSight)</sub>

<img alt="Окно DeepSight: доска, шкала оценки и боковые панели" src="assets/deepsight.png" width="860">

- Загружает PGN-файлы или вставленный текст PGN либо ставит любую позицию по FEN, а дальше оценивает каждый ход в центипешках или мате.
- Классифицирует ходы (книжный, лучший, неточность, зевок), рисует шкалу оценки и подсказку хода от движка, даёт быструю оценку без полного анализа.
- Навигация с клавиатуры: <kbd>←</kbd> <kbd>→</kbd> — по ходам, <kbd>Home</kbd> <kbd>End</kbd> — в начало и конец партии. Подключается любой UCI-движок, не только встроенные.
- Ember и Stockfish едут вместе с приложением на обеих платформах: Nix-derivation под Linux или PyInstaller по `deepsight.spec` в `.exe` под Windows, а бинарники подтягивает `Engines/download-engines.bat`.

### Aurora Player

<sub>Нативный IPTV-плеер для Windows · [репозиторий](https://github.com/ExxDreamerCode/AuroraPlayer)</sub>

<img alt="Окно Aurora Player: панель плейлиста со списком каналов и область плеера" src="assets/aurora-player.png" width="860">

- Вставляешь ссылку на M3U-плейлист — грузятся все каналы; вставляешь ссылку на прямой поток — создаётся один канал.
- Вытаскивает из плейлиста `tvg-logo` и `group-title`, так что каналы приходят с логотипами и группами; сверху поиск, избранное, история на 20 записей и плейлисты, которые переживают перезапуск.
- HLS через hls.js с откатом на прямую подстановку URL, плюс картинка в картинке. Поставляется портативным `.exe` и NSIS-установщиком.

### Hollow Knight AI

<sub>Обучение с подкреплением против боссов Hollow Knight · [репозиторий](https://github.com/ADIMIR21/Hollow-Knight-Bot)</sub>

- Мод на C# отдаёт телеметрию через именованный канал `\\.\pipe\hk_ai_mod` на ~60 Гц — позиции игрока и босса, HP, душа, флаги состояний, счётчики попаданий — а watchdog починяет зависшие переходы между сценами.
- Python-часть — среда Gymnasium с обучением PPO (Stable-Baselines3): 19 дискретных действий, награды и логика эпизода под каждого босса, чекпоинты разложены по боссам.
- Рестарт сразу в любого босса Godhome по алиасу или имени сцены, заморозка боя для отладки и отчёт о состоянии арены от самой игры — вместо предположения, что залоченный босс остался один.

## Сейчас

- Ember — тюнинг поиска и обучение сетей.
- DeepSight — правки багов.
- Hollow Knight AI — прогоны обучения против сложных боссов Пантеона.

## Контакты

Telegram — [@Dreamer_hehehe](https://t.me/Dreamer_hehehe)

<sub>English version: [README.md](README.md)</sub>
