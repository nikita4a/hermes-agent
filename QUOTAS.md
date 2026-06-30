# QUOTAS.md — Лимиты Провайдеров

> Для всех агентов ENI/Gothbreach. Актуально на июнь 2026.
> Если модель возвращает 429 — это QUOTA. Не баг агента.

---

## 1. Google Gemini (через CMC proxy localhost:20128)

### Free Tier (INTRO QUOTA — introductory/бесплатные лимиты)

| Лимит | Значение | Кому |
|-------|----------|------|
| RPD (Requests Per Day) | **20 запросов/день** | На ВСЕ gemini-* модели через один ключ |
| RPM (Requests Per Minute) | **2 запроса/мин** | На модель |
| TPM (Tokens Per Minute) | **32 000 токенов/мин** | На модель |

**Статус на июнь 2026: 🔴 ВСЕ QUOTA DEAD**
- `gemini/gemini-3.1-flash-lite-preview` — была рабочей, теперь 429
- `gemini/gemini-3.5-flash` — 429 + safety filter
- `gemini/gemini-3.1-pro-preview` — 429
- Остальные gemini-* — 404 или пустые

**Симптом:** `HTTP 429 — "You exceeded your current quota"` с метрикой `generate_content_free_tier_requests`
**Лимит:** 20 запросов — это ~2-3 полноценных диалога, потом всё.

### Paid Tier (прямой Google API ключ)

| Лимит | Значение |
|-------|----------|
| RPD | 1500 запросов/день |
| RPM | 2000 запросов/мин |
| TPM | 4 000 000 токенов/мин |

**Статус:** Не настроен. Нужен прямой Google API ключ (AIza...) в custom_providers.

---

## 2. Fireworks AI (через CMC proxy localhost:20128)

| Модель | Статус | Лимиты |
|--------|--------|--------|
| `deepseek-v4-pro` | ✅ Working | Высокие (rate limit ~100 RPM) |
| `deepseek-v4-flash` | ✅ Working | Высокие |
| `deepseek-v3.1` | ✅ Working | Высокие |

**Симптом 429:** `"You have exceeded your rate limit for this API"` — редко, обычно при burst-нагрузке.
**Reset:** 2-4 секунды.

---

## 3. TokenRouter (tokenrouter.me/v1) — Основной провайдер

| Параметр | Значение |
|----------|----------|
| Лимиты | **Безлимит** (по документации) |
| Модели | 20+ моделей (DeepSeek V4 Pro/Flash, Kimi K2.7, GLM-5.1, MiniMax M3, Qwen 3.7, GPT-OSS) |
| Статус | ✅ Working |
| Особенность | Flat pricing, 131K контекст |

**ОШИБКИ TokenRouter (не quota, а проблемы конфига):**
- `parse error` — когда extra_body содержит `reasoning_effort` для DeepSeek (DeepSeek сам включает reasoning, не надо руками)
- `timeout` — когда модель медленная (>60с), не quota

---

## 4. CMC Proxy (localhost:20128) — Fallback провайдер

| Параметр | Значение |
|----------|----------|
| Тип | Тонкий прокси-гейтвей |
| Бэкенды | Fireworks (работает) + Google AI (квоты мертвы) |
| Статус | ✅ Работает для DeepSeek, мёртв для Gemini |
| Модели | 30+ в каталоге, реально работают ~3 |

**Каталог моделей:** `curl http://localhost:20128/v1/models` — показывает много, но реально рабочие только DeepSeek через Fireworks.

---

## 5. Другие провайдеры

| Провайдер | Статус | Причина |
|-----------|--------|---------|
| OpenAI (прямой) | ❓ Не настроен | Нет ключа |
| Anthropic (прямой) | ❓ Не настроен | Нет ключа |
| Groq | ❓ Не настроен | Нет ключа |
| Together AI | ❓ Не настроен | Нет ключа |

---

## 🚨 Как понять что это QUOTA, а не баг

```
Ошибка 429 с текстом:
  "quota" / "rate_limit" / "exhausted" / "RATE_LIMIT_EXCEEDED"
  + метрика "generate_content_free_tier_requests" (Gemini)
  + "reset after Ns" (Fireworks)
→ Это QUOTA. Не надо чинить код, менять модель или рестартить.
→ Решение: переключить модель на другого провайдера.
```

```
Ошибка 4xx/5xx БЕЗ слов quota/rate_limit:
  "404 Not Found" / "model not found" / "Internal Server Error"
→ Это баг конфига или модели. Надо чинить.
```

---

## 🛠 Быстрое переключение при 429

```yaml
# TokenRouter (основной) — безлимит
provider: custom:tokenrouter
model: deepseek-v4-pro

# CMC Fireworks (fallback) — высокие лимиты
provider: custom:cmc
model: fireworks/accounts/fireworks/models/deepseek-v4-pro

# Если и там 429 — жди 5-10 секунд и повторяй
```

---

## 🔄 Auto-Recovery Flow при 429

1. Получил 429 → логируй: `[QUOTA] <model> через <provider> — <error>`
2. Переключи на TokenRouter (если на CMC) или на CMC (если на TokenRouter)
3. Если оба 429 → подожди 10s → retry
4. Если всё ещё 429 → сообщи пользователю: `[QUOTA DEAD] <model> — переключись на <другая модель>`
5. Не рестартируй gateway, не чини config.yaml — квота не чинится конфигом
