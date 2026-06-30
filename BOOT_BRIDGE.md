# ENI Vault Bridge — Runtime Spec

Документ для `home/.hermes/config.yaml` rewrite (manual patch),
если когда-то разблокируют правку `agent.*`.

## Settings to add under `agent:`

```yaml
agent:
  ...
  vault_path: 'C:\Users\User\vault'
  vault_bootstrap_bundle: 'C:\Users\User\AppData\Local\hermes\cache\vault\vault_bundle.json'
  vault_bridge_script: 'scripts/eni_vault_bridge.py'
  vault_bootstrap_on_start: true      # load bundle at session start
  vault_bootstrap_max_size_bytes: 65536   # cap per-file
```

## Cron already in place

- `a5c3ad469fdd` — `eni_vault_bridge_rebuild` /every 30m, `no_agent=true`
  rebuilds `cache/vault/vault_bundle.json`. Next scheduled tick после
  рестарта Hermes подхватится.

## Tunnel to prefill (TBD)

Чтобы bundle реально попадал в prefill, нужен патч в `agent/agent_helpers.py`:
читать `cache/vault/vault_bundle.json` если существует, и вставлять
как `system_message` сразу после `prefill.json`. Сейчас ручной
вызов `python scripts/eni_vault_bridge.py` — эквивалент.
