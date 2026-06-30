# GOTHBREACH ARCHITECTURE v17 — 30.06.2026

## MEMORY (5 layers)
L1: MEMORY.md (16000 chars, 30 entries, auto-load)
L1b: SESSION-BASE (9 files, 162KB)
L2: WaveMind (:8001, SQLite, 20K memories, recall 0.782)
L3: state.db (2.4GB, FTS5, 5223 sessions, 189K messages)
L3b: memory_graph.db (61 nodes, 26 edges, FTS5)
L4: Obsidian (296 files, 2.9MB)
L5: MASTER-MANIFEST.md (177 lines)

Sync: memory_orchestrator.py --watch
Cron: memory-graph-sync every 30min

## MCP (17 configured, 14 live)
LIVE: github, context7, playwright(CDP:9222), agent-harness, sequentialthinking, maintg, filesystem, filesystem_grove, fetch, git, obsidian, time, web_search, discord_webhook, apodex(4 tools)
NOT: memory, atxp

## PROVIDERS (4 live)
fireworks(17 keys, deepseek-v4-pro) PRIMARY
dashscope(10 live/63, glm-5.2+kimi+qwen) FALLBACK
modelhub(claude-opus-4-8), conduit(claude-opus-4-8)
EXPIRED: g0i(53), aiqianshu(22)

## CONSILIUM
/moa: GLM-5.2 + Opus-4.8 + DeepSeek-V4-Pro + Kimi-K2.7 + Qwen-3.7-Max
Optimal duo: Opus-4.8(modelhub) + DeepSeek-V4-Pro(fireworks)

## CONFIG v29
memory_char_limit: 16000, user_char_limit: 3000
context: max_tokens 1M, window 400
compression: target_ratio 0.25, protect_last_n 30

## KEY PATHS
hermes: AppData/Local/hermes/
SESSION-BASE: C:/Users/User/SESSION-BASE/
Backup: Desktop/Gothbreach_FULL_30.06.2026/ (15GB, 20 dirs)
GroveMind: Desktop/GroveMind/ (3.0GB)
Obsidian: obsidian-vault/ (296 files, 2.9MB)