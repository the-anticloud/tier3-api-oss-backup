# Developer Cookbook — api-oss-backup
**Stack:** Python 3.11, AES-256-GCM, gzip, SQLite, AIOSS_FORMAT
**Domain:** Sovereign backup and disaster recovery for Anticloud state: AIOSS chains, model weights, KB
**License:** Apache-2.0 | **IP:** USPTO pending 2026, Anticloud FZ LLE

## Core Usage

```python
from api_oss_backup import BackupManager

bm = BackupManager(
    backup_dir="E:/anticloud_backups/",
    encryption_key=vault.retrieve("backup_key"),
    aioss_chain="./backup.aioss"
)

# Full backup
manifest = bm.backup_full(
    sources=["./aioss_chains/", "./kantor_k5.db", "./miirai_memory.db"]
)
print(f"Backup: {manifest.archive_path}, hash: {manifest.chain_hash}")

# Incremental backup (since last)
manifest = bm.backup_incremental()

# Restore and verify
bm.restore(manifest.archive_path, target_dir="./restored/")
verified = bm.verify_restore("./restored/", pax_model="./pax-27b-q4.gguf")
print(f"Restore verified: {verified.aioss_valid}")
```

## AIOSS Chain Append

```python
import hashlib, time

def aioss_append(chain_path, payload: bytes, module_id: str):
    entry_hash = hashlib.sha3_256(payload).digest()
    ts = int(time.time_ns()).to_bytes(8, 'big')
    with open(chain_path, 'rb') as f:
        f.seek(-32, 2); prev_hash = f.read(32)
    new_hash = hashlib.sha3_256(prev_hash + entry_hash + ts).digest()
    with open(chain_path, 'ab') as f:
        f.write(ts + entry_hash + new_hash)
    return new_hash.hex()

# After every api-oss-backup output:
chain_hash = aioss_append("./api_oss_backup.aioss",
                           result_bytes, "api-oss-backup")
```

## Performance & Integration

AES-256-GCM + gzip: ~3:1 compression for AIOSS chains. Schedule nightly backups via SOVEREIGN_OS cron. Incremental backups use file modification timestamps. Integration: backs up outputs of AIOSS_FORMAT, KANTOR_K5, MIIRAI_CHAT, PAX_STORAGE (T2).
