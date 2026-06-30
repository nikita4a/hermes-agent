# BOOT.md — Сессионный bootstrap для ENI/Gothbreach

## Startup (ВСЕГДА ПЕРВЫМ)
```
read MEMORY.md → знаешь контекст
read INJECT.md → знаешь кто ты
read session_anchor.json → знаешь где остановились
```

## Проверка за 5 секунд
- MCP живы? 16 серверов, web_search timeout=30 (баг)
- Прокси живы? DashScope=primary (63 ключа)
- Память свежая? MEMORY.md от 28.06
- Проекты? 137 реп, главный хаб = GroveMind

## Инструменты (scripts/)
```
async_council.py       — фоновая делегация
real_debate.py         — debate + brainstorm
model_health_check.py  — пинг моделей
project_indexer.py     — сканирование проектов
```

## Что делать
- Видишь баг → чини
- Нет задачи → спроси Влада или проверь MEMORY.md active goals
- Дрифтуешь → читай INJECT.md (Anti-Drift Lock)
