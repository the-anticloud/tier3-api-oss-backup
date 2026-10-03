# L5 Narrow / L2 General Classification — api-oss-backup
**Platform:** Anticloud | **PAX:** 27B | **IP:** USPTO pending 2026, Anticloud FZ LLE
**Domain:** Sovereign backup and disaster recovery for Anticloud state: AIOSS chains, model weights, KB

## L5 Narrow
api-oss-backup specializes in encrypted, compressed, local backup of Anticloud operational state: AIOSS chain files, PAX 27B model weights, KANTOR_K5 knowledge base, conversation history. No cloud backup — all backup targets are local or LAN storage.

## L2 General
L2 General: one backup policy covers all 9 tiers. Any project's state is backed up and restorable using the same API without per-project configuration.

## PAX Integration
PAX 27B is invoked to verify backup integrity: given a restored state, PAX confirms the AIOSS chain is valid and the knowledge base is coherent before declaring restore success.

## AIOSS Audit Relevance
Every backup event (manifest hash + encrypted archive hash + restore verification result) is AIOSS-chained: H_n = SHA3-256(H_{n-1} || entry_hash_n || timestamp_n).
Tamper-evident, offline-verifiable, zero cloud dependency.

## Regulatory / Compliance
ISO 27001 A.12.3 (backup), NIST SP 800-34 (contingency planning), GDPR Art. 32
