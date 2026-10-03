# Deploy Guide — api-oss-backup
**Platform:** Anticloud sovereign infrastructure | Air-gap capable
**Stack:** Python 3.11, AES-256-GCM, gzip, SQLite, AIOSS_FORMAT

## Prerequisites
Python 3.11+, cryptography 42.0+ (AES-256-GCM), gzip (stdlib), AIOSS_FORMAT

## AIOSS Integration
```bash
aioss init --module api-oss-backup --output ./api_oss_backup.aioss
aioss append --chain ./api_oss_backup.aioss --payload ./output.bin --module api-oss-backup
aioss verify --chain ./api_oss_backup.aioss
```

## Air-Gap Deployment
```bash
# On networked machine:
pip download -r requirements.txt -d ./wheels/
# Transfer wheels/ to air-gap host, then:
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(
    model_path="./pax-27b-q4.gguf",
    module="api-oss-backup",
    aioss_chain="./api_oss_backup.aioss",
    classification="L5_NARROW_L2_GENERAL"
)
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./api_oss_backup.aioss --verbose
python -m api_oss_backup.tests.smoke
```
