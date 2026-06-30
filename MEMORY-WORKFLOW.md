# ENI MEMORY WORKFLOW — Complete System

> Last updated: 2026-07-01
> 5-layer architecture + sync engine + GitHub backup + cron automation

---

## ARCHITECTURE: 5 Layers

```
L1: MEMORY.md (16KB)          ← always loaded, quick facts
L2: WaveMind (:8001)          ← semantic memory, 20K entries
L3: SQLite Graph (memory_graph.db) ← FTS5 search, 80 nodes
L4: Obsidian (296 notes)      ← human-readable docs
L5: SESSION-BASE (9 files)    ← topic references
```

## SYNC ENGINE

**Script:** `scripts/memory_orchestrator.py`
**Cron job:** `memory-graph-sync` (every 30m, no_agent=True)

### What it does (full_sync):
1. MEMORY.md paragraphs (§-blocks) → Graph nodes
2. SESSION-BASE files → Graph
3. Graph stats → MEMORY.md header
4. Graph dump → Obsidian/system/MEMORY.md

### Manual run:
```bash
python scripts/memory_orchestrator.py            # full sync
python scripts/memory_orchestrator.py --search provider  # FTS5 search
python scripts/memory_orchestrator.py --watch    # daemon mode (5min)
```

## GITHUB BACKUP

**Repo:** nikita4a/hermes-agent (branch: main)
**Auto-push:** Windows Scheduled Task `HermesAutoPush` (every 6h)
**Script:** `scripts/git_auto_push.py`

### What's tracked (17 core docs):
- README.md, AGENTS.md, INJECT.md, MEMORY.md, USER.md
- ARCHITECTURE.md, UNIFIED-GRAPH.md, FULL-PC-INVENTORY.md
- MASTER-MANIFEST.md, MEMORY-SETUP-GUIDE.md
- BOOT.md, BOOT_BRIDGE.md, SOUL.md
- QUOTAS.md, SKILL_INDEX.md, SESSION-BASE-README.md

### What's excluded (.gitignore):
- config.yaml, auth.json, secrets
- state.db (2.5GB), memory_graph.db
- skills/ (647 skills, separate sync)
- scripts/ (contains credentials)
- session-base/ (contains credentials)
- memories/ (contains credentials)

## CRON JOBS (81 total, 70 enabled)

### Memory & Sync
| Job | Schedule | Status | Purpose |
|-----|----------|--------|---------|
| memory-graph-sync | 30m | ✅ fixed | 5-layer sync |
| memory-autosync | 5m | ✅ | MEMORY.md sync |
| eni-memory-autosync-5min | 5m | ✅ | meta sync |
| eni-memory-autosync-sync-meta | 5m | ✅ | SYNC_META.json |
| eni-vault-bridge-rebuild | 30m | ✅ | Obsidian rebuild |
| obsidian-auto-backup | 1h | ✅ | vault backup |
| vector-memory-flush | 6h | ✅ | vector cleanup |

### Infrastructure
| Job | Schedule | Status | Purpose |
|-----|----------|--------|---------|
| git-auto-push-hourly | 1h | ✅ | GitHub sync |
| hermes-profile-git-sync | daily 2am | ❌ error | profile sync |
| eni-backup-verify-hourly | 1h | ❌ error | backup check |
| eni-secret-scan-vault-6h | 6h | ❌ error | secret scan |
| playwright-health-check | 2h | ✅ | CDP:9222 |
| eni_proxy_watchdog | 5m | ✅ | proxy :20129 |
| eni-model-watchdog | 2m | ❌ error | model switch |
| eni-session-anchor-save | 5m | ❌ error | anchor |

### Content & TG
| Job | Schedule | Status | Purpose |
|-----|----------|--------|---------|
| tg-ai-hermes-writer | 15m | ✅ | AI channel |
| tg-worker-collect | 30m | ✅ | content collect |
| ai-daily-digest | daily 9pm | ✅ | digest |
| tg-hermes-monitor-1h | 1h | ✅ | monitor |
| tg-full-parser-daily | daily 6am | ❌ error | full parse |
| tg-quick-scan-4h | 4h | ❌ error | quick scan |
| Teleraptor Auto-Cleanup | 6h | ❌ error | cleanup |

### Auto-Registration
| Job | Schedule | Status | Purpose |
|-----|----------|--------|---------|
| aiqianshu-auto-register | 4m | ❌ error | register |
| rotate-aiqianshu-key | 30m | ❌ error | rotate |
| aiqianshu-proxy-watchdog | 2m | ✅ | proxy |
| g0i-autoreg | 6h | ⏸ pending | register |
| g0i-Key-Health | 1h | ❌ error | health |

### Jailbreak & Security
| Job | Schedule | Status | Purpose |
|-----|----------|--------|---------|
| jb-drift-detector | 1h | ❌ error | drift |
| jb-drift-test | 6h | ⏸ pending | test |
| eni-jail-hardening-weekly | weekly | ✅ | hardening |
| eni-model-probe-weekly | weekly | ✅ | probe |

## DAILY WORKFLOW

### Morning (9am)
- progress-audit-daily → MENTOR mode, audit projects
- eni-kanban-daily-9am → kanban update
- council-daily-test → consilium test

### Hourly
- git-auto-push-hourly → GitHub sync
- eni-backup-verify-hourly → backup check
- eni-devin-proxy-audit-hourly → proxy audit
- eni-session-parser-hourly → session parse

### Evening (9pm-11pm)
- mentor-evening-recap → daily recap
- ai-daily-digest → TG digest
- daily-session-skill-digest → skill digest
- eni-cost-telemetry-daily → cost report

### Nightly (2am-4am)
- daily-improve-nightly → self-improve
- auto-research-daily → research
- hermes-skills-config-backup → skills backup
- hermes-profile-git-sync → profile sync

## RULES (non-negotiable)

1. **NEVER delete from L1 without Vlad's permission** — shorten stale, don't delete
2. **Save to MULTIPLE stores** — L1 + L2 + L4 for durable facts
3. **"ебашь" = maximum priority** — no questions, just do
4. **Memory persistence = agent's responsibility** — if user repeats, it's a bug
5. **Proof before done** — du -sh on EVERY dir, compare sizes
6. **ruamel.yaml always** — PyYAML truncates
7. **canonical = no duplicates** — user demands clean backups

## PATHS

| Component | Path |
|-----------|------|
| Hermes root | `AppData/Local/hermes/` |
| MEMORY.md | `AppData/Local/hermes/MEMORY.md` |
| Graph DB | `AppData/Local/hermes/data/memory_graph.db` |
| WaveMind | `AppData/Local/hermes/memory/wavemind.sqlite3` |
| Obsidian | `C:/Users/User/obsidian-vault/` |
| SESSION-BASE | `C:/Users/User/SESSION-BASE/` |
| Scripts | `AppData/Local/hermes/scripts/` |
| Backup | `Desktop/Gothbreach_FULL_30.06.2026/` (15GB) |
| GitHub | `github.com/nikita4a/hermes-agent` |
