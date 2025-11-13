# Case ID Format & Numbering

**Owner:** Shawn • **Visibility:** Internal

**Format:** `PP-LEX-YYMM-####` (zero‑padded seq).  
**Example:** `PP-LEX-2511-0001`.  
**Sequence:** reset monthly; maintain `seq_state.json`.  
**Optional checksum:** modulo‑10 of digits; append `-X` where X ∈ 0–9.  
**Display:** monospace; large on vouchers.
**Automation:** `data/server` issues IDs and persists the counter; manual edits are blocked in admin portal forms to avoid collisions.
