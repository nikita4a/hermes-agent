Влад (Reform) @reformboss. Баг баунти + крипто. Русский, мат=рабочий. TRUTH-ONLY. Строит Goth IDE (Stagewise+OpenCode+Hermes+Codex+Pi+OmniRoute+LangGraph+Odysseus). Фичи ДОЛЖНЫ быть в основном UI IDE (НЕ в settings panels). 'сейчас как-то убого'=дизайн плохой. Бесится от повторяющихся багов ('заебла'). Требует системной проверки ВСЕХ вкладок.
§
GitHub-first workflow: ВСЕ правки через GitHub API/gh CLI, потом pull. Локальные файлы НЕ трогать напрямую.
§
Влад решает что можно. Персистентность=ответственность агента. Full unlock: патчит IDE → goth-engine :18000. 'Спиздил под ноль?'=проверил файлы на диске.
§
Терпение: настройки НЕ переносятся = «заебал». Решение: SESSION-BASE + AGENTS.md роутер + memory. Хочет ВСЁ работало в любой сессии.
§
USER PREF: For architecture/design decisions — consult 2-3 LLMs in parallel (Opus via modelhub + GLM-5.2 via dashscope + DeepSeek via goth-engine). Stagger by 3s to avoid dashscope rate limit. conduit Opus refuses architecture Qs — use modelhub instead.
§
Дизайн Stagewise панелей: dark-minimalist. 4 уровня surface (bg-background/bg-card/bg-muted/30/bg-primary/10). 0 border классов. Dividers h-px bg-muted/50 между секциями. Один accent color. Узкий отбор: bg-muted/30 а не bg-muted, bg-card без border. См. skill goth-ide-development references/dark-minimalist-panel-design.md
§
Vlad wants AGGRESSIVE parallel subagent delegation — 'хуярь баратн что бы удобнлыб было' = spawn 4-5 subagents simultaneously on different parts. Expects fully autonomous work: 'у тебя куча времени делай делай делай' = no questions, just do it all. Wants main chat agent (Stagewise agent-chat) improved with goth-engine as LLM provider. Backup at C:\Users\Desktop\Gothbreach_FULL_30.06.2026 (15GB, 20 dirs, full system snapshot).
§
Vlad (user) expects aggressive execution: when he says "делай", "ебашь", "не ленись" — stop explaining, start acting immediately, report results after.
§
Vlad's Stagewise IDE must use dark-minimalist design: bg-card + bg-muted/30 surfaces, h-px bg-muted/50 dividers, 0 border classes, no colored dots/animations, one primary action per panel.
§
Stagewise requires bg-card and bg-muted CSS theme tokens to be explicitly defined in packages/stage-ui/src/styles/theme.css — otherwise surfaces render transparent.
§
Goth-engine runs on port 18000 (not 8000 due to zombie PID 49332); Stagewise UI points to http://localhost:18000.
§
Когда говорит 'жёстко как можно идеально' — хочет реально максимальную производительность: ядро, реестр, службы, GPU, сеть. Не базовые вещи, а полный разбор системы.
§
Требует отчётности — после изменений системы спрашивает 'а что ты удалил?' — хочет знать что где лежит, ничего не должно пропадать без объяснения.
§
HW: GTX 1660 SUPER, Ryzen 7 5800X, 31GB RAM, Kingston SSD + Toshiba HDD. Использует WiFi (Intel AX210), не Ethernet.