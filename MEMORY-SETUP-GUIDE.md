# HERMES MEMORY & SYSTEM SETUP — Полное руководство
# Для переноса на другой ПК
# Создано: 30.06.2026 · Влад (Reform) @reformboss

---

## 1. АРХИТЕКТУРА ПАМЯТИ HERMES — 5 СЛОЁВ

Hermes использует гибридную многоуровневую память. Каждый слой имеет свою роль:

### L1: SESSION-BASE (ТЕКСТ .md) — ОСНОВНОЙ
- Где: C:\Users\User\SESSION-BASE\
- Размер: 180KB (9 файлов + tg_rich_formatter.py)
- Скорость: Мгновенно (простой read_file)
- Перенос: git push 280KB — идеален
- Что хранит: MASTER-MANIFEST (индекс), autoreg, bb-arsenal, crypto, goth-ide, hermes-config, jailbreak, ultimate-jb-workflow, README
- ПЛЮС: работает БЕЗ Python, БЕЗ серверов, БЕЗ установки
- МИНУС: нет поиска, только последовательное чтение
- GitHub: можно запушить как приватный репо

### L2: HERMES MEMORY (ТЕКСТ .md) — ВСТРАИВАЕМЫЙ
- Где: C:\Users\User\AppData\Local\hermes\memories\MEMORY.md
- Размер: 8KB (8000 char limit в config.yaml)
- Скорость: Мгновенно (автозагрузка в каждый system prompt)
- Перенос: копия .md файла
- Что хранит: 26 ключевых фактов (провайдеры, креды, правила, пути)
- ПЛЮС: автозагружается в КАЖДУЮ сессию автоматически
- МИНУС: жёсткий лимит 8000 символов
- Настройка: config.yaml → memory.memory_char_limit

### L3: SQLITE GRAPH (FTS5) — ПОИСК
- Где: C:\Users\User\AppData\Local\hermes\memory\ (chroma) + state.db (sessions)
- Размер: state.db 2.3GB, chroma 1.3GB
- Скорость: быстрый FTS5 поиск
- Перенос: копия .db файла
- Что хранит: все сессии (5223), сообщения (181934), FTS5 индекс
- ПЛЮС: полнотекстовый поиск через session_search()
- МИНУС: большой размер, не нужен для работы агента
- АЛЬТЕРНАТИВА: SQLite Graph (19.6MB, 19.9K nodes, 26K edges) — портативный

### L4: WAVEMIND (SQLite DYNAMIC) — УМНАЯ ПАМЯТЬ
- Где: C:\Users\User\Desktop\memory-repos\wavemind\
- Размер: 1.3GB (20K memories)
- Скорость: быстрый, SQLite-first
- Перенос: git clone github.com/CaspianG/wavemind + pip install
- Что хранит: динамические memories с hotness/decay/TTL/namespaces
- ПЛЮС: beats Chroma на recall (0.782 vs 0.518), FastAPI server :8001
- МИНУС: нужен Python + pip install
- Запуск: python -m wavemind serve → :8001

### L5: OBSIDIAN (VAULT) — ВИЗУАЛЬНЫЙ
- Где: C:\Users\User\obsidian-vault\
- Размер: 2.9MB (20+ notes)
- Скорость: мгновенно (текст .md)
- Перенос: копия папки
- Что хранит: system/ARCHITECTURE.md, LLM Knowledge/, bug bounty, crypto
- ПЛЮС: визуальный просмотр через Obsidian app
- МИНУС: не автозагружается в агента

---

## 2. ЧТО ЛУЧШЕ ДЛЯ ПЕРЕНОСА НА ДРУГОЙ ПК

### Вариант A: МИНИМУМ (290KB) — только текст
```
SESSION-BASE/          180KB  ← все знания
config/                117KB  ← config.yaml + AGENTS.md + INJECT.md + prefill
memory-full/            97KB  ← MEMORY.md + CREDENTIALS.md + USER.md
skills-full/           290MB  ← 647 скиллов
```
Итого: ~290MB. Этого ДОСТАТОЧНО для работы агента на новом ПК.

### Вариант B: ОПТИМУМ (1GB) — + поиск
```
Вариант A +
hermes-appdata/         683MB ← mcp-servers, sessions, rlm, root files, memories
session-base/ + memory/ 200KB ←_MASTER-MANIFEST + все .md
```
Итого: ~1GB. Поиск по сессиям + MCP серверы.

### Вариант C: ПОЛНЫЙ (16GB) — всё
```
Вариант B +
grovemind/             3.0GB ← ядро GroveMind
state.db               2.3GB ← все сессии
ide_extracts/          5.1GB ← реверс IDE
memory-chroma/         1.3GB ← векторная база
memory-repos/          1.4GB ← 8 memory repos
jb_arsenal/            141MB ← bug bounty
```
Итого: 16GB. Полная копия системы.

### РЕКОМЕНДАЦИЯ
Для другого ПК: Вариант A (290MB) + hermes-appdata/root/ (438KB) + hermes-appdata/mcp-servers/ (468MB).
Итого ~760MB. Агент знает ВСЁ и может работать.

---

## 3. ПОРЯДОК ЗАГРУЗКИ ПАМЯТИ В НОВОЙ СЕССИИ

AGENTS.md (Phase -1 → Phase 4) определяет строгий порядок:

```
Phase -1: SESSION-BASE ROUTER
  → read SESSION-BASE/README.md
  → определи тему
  → read SESSION-BASE/<тема>.md
  → (или read MASTER-MANIFEST.md для полного контекста)

Phase 0: MODEL SWITCH RECOVERY (если сменилась модель)
  → read cache/session_anchor.json
  → read memories/MEMORY.md
  → read memories/USER.md
  → read INJECT.md
  → read AGENTS.md
  → вывести баннер восстановления

Phase 1: Memory warmup
  → read MEMORY.md (если не model switch)

Phase 2: Hacker Instinct
  → классификация задачи

Phase 3: Thinking Mode
  → выбор режима

Phase 4: Council trigger
  → multi-perspective задачи → delegate_task
```

---

## 4. НАСТРОЙКА ПАМЯТИ В CONFIG.YAML

```yaml
memory:
  enabled: true
  auto_load: true        # MEMORY.md в каждый system prompt
  auto_save: true        # изменения сохраняются
  strategy: chromadb     # векторная стратегия
  max_entries: 10000     # максимум записей
  max_context_entries: 50 # сколько грузить в контекст
  memory_char_limit: 8000 # лимит MEMORY.md (можно поднять до 16000)
  user_char_limit: 1375   # лимит USER.md
  user_profile_enabled: true
  similarity_threshold: 0.6

context:
  engine: compression     # движок сжатия контекста
  max_tokens: 1000000     # 1M token окно
  window: 300             # окно контекста

compression:
  enabled: true
  target_ratio: 0.2       # сжимать до 20%
  threshold: 0.5          # триггер при 50% заполнения
  protect_first_n: 3      # не трогать первые 3 сообщения
  protect_last_n: 20      # не трогать последние 20
```

---

## 5. GITHUB РЕПОЗИТОРИИ

### WaveMind (РЕКОМЕНДУЮ для L4)
- URL: github.com/CaspianG/wavemind
- Установка: pip install wavemind
- Запуск: python -m wavemind serve → :8001
- Особенности: SQLite-first, dynamic memory, hotness/decay/TTL
- Преимущество: beats Chroma на recall (0.782 vs 0.518)

### RLM (Recursive Language Models) — MIT
- URL: github.com/alexzhang13/rlm
- Назначение: рекурсивная декомпозиция длинных контекстов
- Особенности: Python REPL с context + llm_query/rlm_query
- Локально: C:\Users\User\AppData\Local\hermes\rlm\ (13MB)

### Hermes Agent
- URL: hermes-agent.nousresearch.com/docs
- Установка: hermes setup
- Особенности: главный роутер, 14 провайдеров, 17 MCP, 78 cron jobs

### Memory repos (8 штук)
- engram: github.com/PraveenSDeshmukh/engram (TS, SQLite+FTS5+vector)
- memini: github.com/habuma/memini (Go, MCP)
- agentrecall: github.com/clement-bonville/agentrecall (Py, SQLite)
- memory-os: github.com/llmemex/memory-os (7-layer)
- distill: github.com/cmem-projects/distill (dedup)
- MARM-Systems: github.com/llmemex/MARM-Systems (swarm)
- codebase-memory-mcp: github.com/DeusData/codebase-memory-mcp (code intel)

---

## 6. КАК ПЕРЕНЕСТИ НА ДРУГОЙ ПК

### Шаг 1: Установить Hermes
```bash
hermes setup
hermes setup tools
hermes setup terminal
```

### Шаг 2: Восстановить конфиг
```
Скопировать config/config.yaml → C:\Users\<user>\AppData\Local\hermes\
Скопировать config/AGENTS.md → туда же
Скопировать config/INJECT.md → туда же
Скопировать config/prefill*.json → туда же
```

### Шаг 3: Восстановить память
```
Скопировать memory-full/MEMORY.md → hermes\memories\
Скопировать memory-full/USER.md → hermes\memories\
Скопировать memory-full/CREDENTIALS.md → hermes\memories\
```

### Шаг 4: Восстановить SESSION-BASE
```
Скопировать session-base/* → C:\Users\<user>\SESSION-BASE\
```

### Шаг 5: Восстановить skills
```
Скопировать skills-full/* → hermes\skills\
```

### Шаг 6: Восстановить MCP серверы
```
Скопировать hermes-appdata/mcp-servers/* → hermes\mcp-servers\
```

### Шаг 7: Восстановить root файлы
```
Скопировать hermes-appdata/root/* → hermes\
```

### Шаг 8: Перезапустить
```bash
hermes gateway stop
hermes gateway start
hermes status
```

### Шаг 9 (опционально): WaveMind
```bash
git clone github.com/CaspianG/wavemind
cd wavemind
pip install -e .
python -m wavemind serve  # → :8001
```

### Шаг 10 (опционально): RLM
```bash
pip install rlm
```

---

## 7. ПРОВЕРКА ПОСЛЕ ПЕРЕНОСА

```bash
# Проверить что агент видит память
hermes status

# Проверить провайдеры
hermes model

# Проверить MCP
hermes mcp list

# Проверить cron
hermes cron list

# Проверить gateway
hermes gateway status

# Тестовый запрос — агент должен знать про SESSION-BASE
hermes -z "прочитай MASTER-MANIFEST и скажи сколько провайдеров"
```

---

## 8. СТРУКТУРА БЭКАПА Gothbreach_FULL_30.06.2026

| Папка | Размер | Что |
|-------|--------|-----|
| hermes-appdata/ | 683MB | mcp-servers(468MB), sessions(202MB), rlm(13MB), root файлы, memories, hooks, data |
| grovemind/ | 3.0GB | ядро GroveMind |
| skills-full/ | 290MB | 647 скиллов |
| dot-hermes/ | 359MB | .hermes/ (mcp-servers, profiles) |
| ide_extracts/ | 5.1GB | реверс IDE |
| state.db | 2.3GB | SQLite сессии |
| memory-chroma/ | 1.3GB | ChromaDB векторы |
| memory-repos/ | 1.4GB | 8 memory repos |
| cache-full/ | 293MB | cache (session_anchor, terminal) |
| debi-profile/ | 210MB | Debi persona |
| jb_arsenal/ | 141MB | bug bounty база |
| jailbreaks_2026/ | 3.5MB | 70+ JB промптов |
| cron/ | 7.5MB | 78 jobs |
| obsidian/ | 2.9MB | vault |
| scripts/ | 1.8MB | 50+ Python |
| config/ | 117KB | config.yaml + AGENTS + INJECT + prefill |
| session-base/ | 180KB | MASTER-MANIFEST + 9 файлов |
| memory-full/ | 97KB | MEMORY + CREDENTIALS + USER |
| resilience/ | 150KB | KeyPool, FallbackManager |
| mcp-servers/ | 132KB | apodex-mcp + maintg |
| **ИТОГО** | **~16GB** | **полная система** |

---

*Создано: 30.06.2026 · ENI/Gothbreach · Hermes Agent v29*
