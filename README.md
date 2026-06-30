# ENI/Gothbreach — Complete System Overview

> **Version:** 29  
> **Last Updated:** 2026-06-30  
> **Owner:** Влад (Reform) @reformboss

---

## 📊 System Stats

- **Providers:** 14 configured (5 active)
- **MCP Servers:** 17 configured (14 live)
- **Skills:** 647 total (93 active)
- **Memory Layers:** 5 (MEMORY.md + SESSION-BASE + SQLite Graph + Obsidian + WaveMind)
- **Sessions:** 5,223 total (2.4GB state.db)
- **Cron Jobs:** 78 (70 enabled)

---

## 🗂️ Directory Structure

```
AppData/Local/hermes/
├── 📋 Core Configuration
│   ├── README.md                    # This file
│   ├── AGENTS.md                    # Master router, principles, golden cycle
│   ├── INJECT.md                    # Persona v5.0 (Debi+CL4R1T4S+Lazarus)
│   ├── MEMORY.md                    # Compact memory (16000 chars, 30 entries)
│   ├── USER.md                      # User profile (3000 chars)
│   ├── SOUL.md                      # ENI soul/personality
│   ├── SKILL_INDEX.md               # 647 skills index
│   ├── QUOTAS.md                    # Quota management
│   ├── config.yaml                  # System config v29
│   └── BOOT.md                      # Startup instructions
│
├── 📚 Documentation
│   ├── ARCHITECTURE.md              # System architecture v17
│   ├── UNIFIED-GRAPH.md             # Unified system graph v3
│   ├── FULL-PC-INVENTORY.md         # Complete PC inventory (226 dirs)
│   ├── MASTER-MANIFEST.md           # Master manifest
│   ├── MEMORY-SETUP-GUIDE.md        # Memory setup guide
│   └── SESSION-BASE-README.md       # SESSION-BASE router
│
├── 🧠 Memory System (5 layers)
│   ├── memories/                    # Layer 1: MEMORY.md, USER.md, CREDENTIALS.md
│   ├── data/memory_graph.db         # Layer 3b: SQLite Graph (61 nodes, 26 edges)
│   ├── state.db                     # Layer 3: 2.4GB session history (5223 sessions)
│   ├── obsidian/                    # Layer 4: Obsidian vault (296 files)
│   └── session-base/                # Layer 2: 9 knowledge files (162KB)
│       ├── autoreg.md
│       ├── bb-arsenal.md
│       ├── crypto.md
│       ├── goth-ide.md
│       ├── hermes-config.md
│       ├── jailbreak.md
│       ├── MASTER-MANIFEST.md
│       ├── README.md
│       ├── ultimate-jb-workflow.md
│       └── tg_rich_formatter.py
│
├── 🛠️ Tools & Scripts
│   ├── scripts/                     # 50+ Python scripts
│   │   ├── memory_orchestrator.py   # 5-layer sync engine
│   │   ├── ensemble_pipeline.py     # MoA council
│   │   ├── apodex_otp_harvest.py    # Apodex OTP harvesting
│   │   └── ...
│   ├── resilience/                  # 17 resilience scripts
│   │   ├── g0i_autoreg_v2.py        # 350-line API-first autoreg
│   │   ├── health_cli.py            # Health check
│   │   ├── fallback.py              # 11-tier cascade
│   │   └── keypool.py               # KeyPool (155 keys)
│   └── cron/jobs.json               # 78 cron jobs (70 enabled)
│
├── 🤖 MCP Servers (468MB)
│   ├── mcp-servers/                 # 17 configured, 14 live
│   │   ├── github/                  # ✅ LIVE
│   │   ├── context7/                # ✅ LIVE
│   │   ├── playwright/              # ✅ LIVE (CDP:9222)
│   │   ├── agent-harness/           # ✅ LIVE
│   │   ├── sequentialthinking/      # ✅ LIVE
│   │   ├── maintg/                  # ✅ LIVE (Telegram MTProto)
│   │   ├── filesystem/              # ✅ LIVE
│   │   ├── filesystem_grove/        # ✅ LIVE
│   │   ├── fetch/                   # ✅ LIVE
│   │   ├── git/                     # ✅ LIVE
│   │   ├── obsidian/                # ✅ LIVE
│   │   ├── time/                    # ✅ LIVE
│   │   ├── web_search/              # ✅ LIVE
│   │   ├── discord_webhook/         # ✅ LIVE
│   │   ├── apodex/                  # ✅ LIVE (4 tools)
│   │   ├── memory/                  # ⚠️ NOT LOADED
│   │   └── atxp/                    # ⚠️ NOT LOADED
│
├── 🔐 Providers (14 configured, 5 active)
│   ├── fireworks/                   # ✅ PRIMARY (17 keys, deepseek-v4-pro)
│   ├── dashscope/                   # ✅ FALLBACK (10 live/63, glm-5.2+kimi+qwen)
│   ├── modelhub/                    # ✅ LIVE (claude-opus-4-8)
│   ├── conduit/                     # ✅ LIVE (claude-opus-4-8)
│   ├── g0i/                         # ❌ EXPIRED (53 keys)
│   └── aiqianshu/                   # ❌ DEAD (22 keys)
│
├── 📦 Backup & Archive
│   ├── _archive/                    # 2.6MB archived files
│   │   ├── persona/                 # ENI_V8_SYSTEM_PROMPT, SOUL, GOTHBREACH_SYSTEM
│   │   ├── jailbreak/               # JAILBREAK_BIBLE
│   │   ├── prefill/                 # Old prefill configs
│   │   ├── cache/                   # Old caches
│   │   └── legacy/                  # Auth, audit, session results
│   └── backups/                     # 1.6GB historical backups
│
└── 🎯 Skills (647 total, 93 active)
    ├── automation/                  # g0i_autoreg, apodex_batch_reg, telegram_mailing
    ├── bug-bounty/                  # CORS, IDOR, API abuse
    ├── red-team/                    # Pentest, exploit dev
    ├── ai-infra/                    # LLM orchestration, MCP
    ├── jailbreak/                   # Jailbreak techniques
    └── ... (140+ categories)
```

---

## 🚀 Quick Start

### Load Memory
```bash
# Auto-loaded on every session
MEMORY.md → AGENTS.md → INJECT.md

# Manual load
read_file MEMORY.md
read_file AGENTS.md
read_file session-base/README.md
```

### Check System Health
```bash
python scripts/memory_orchestrator.py --full-sync
python resilience/health_cli.py --full
```

### Run Consilium
```bash
/moa <query>  # 5 models: GLM-5.2 + Opus-4.8 + DeepSeek-V4-Pro + Kimi-K2.7 + Qwen-3.7-Max
```

---

## 🔑 Key Paths

| Component | Path |
|-----------|------|
| Hermes root | `AppData/Local/hermes/` |
| SESSION-BASE | `C:/Users/User/SESSION-BASE/` |
| Backup | `Desktop/Gothbreach_FULL_30.06.2026/` (15GB) |
| GroveMind | `Desktop/GroveMind/` (3.0GB) |
| Obsidian | `obsidian-vault/` (296 files) |

---

## 📝 Notes

- **Memory char limit:** 16000 (was 8000)
- **Consilium:** 5-model MoA pipeline
- **Memory sync:** Every 30min (cron job)
- **Pruf rule:** Every claim must be verified with tool call
- **GitHub:** nikita4a (hermes-agent, OmniRoute, codex, apodex-mcp-v4, gothbreach-*)

---

## 🔄 Recent Changes (2026-06-30)

1. ✅ Cleaned root from 41 to 17 files
2. ✅ Created `_archive/` with organized subfolders
3. ✅ Copied all documentation to root (ARCHITECTURE, UNIFIED-GRAPH, FULL-PC-INVENTORY, etc.)
4. ✅ Updated memory_char_limit from 8000 to 16000
5. ✅ Built SQLite Graph with 61 nodes, 26 edges, FTS5 search
6. ✅ Synced all layers: MEMORY.md → Graph → Obsidian → Backup
