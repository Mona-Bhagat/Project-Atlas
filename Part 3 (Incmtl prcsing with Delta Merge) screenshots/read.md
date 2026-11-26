## Part 3 — Incremental Processing with Delta MERGE

### Objective
Implement incremental data processing in Microsoft Fabric using PySpark, Delta Lake and timestamp-based watermarks to efficiently process new and updated retail orders.

### Architecture
**Fabric Pipeline → Parameterised PySpark Notebook → Watermark Filtering → Delta MERGE → Watermark Update**

### Implementation
- Created `silver_orders` as a Delta table with an initial baseline of 500 records.
- Implemented `control_watermark` to track successful incremental processing.
- Used `updated_at` to identify eligible changes.
- Implemented Delta MERGE using `order_id` as the business key.
- Applied conditional updates to prevent older records overwriting newer ones.
- Created a parameterised PySpark notebook and integrated it with a Fabric Data Pipeline.

### Testing & Validation
- Processed incremental batches containing inserts and updates.
- Verified that replaying an already-processed batch did not create duplicate orders.
- Confirmed that a no-change batch skipped MERGE successfully.
- Tested automated pipeline execution with one new order and one updated order.
- Validated a final Silver count of **521 orders** and a watermark of **2026-11-12 13:00:00 UTC**.

### Engineering Takeaways
1. Watermarks reduce unnecessary processing by identifying records modified since the last successful load.
2. Delta MERGE supports inserts and updates while preserving business-key uniqueness under validated input assumptions.
3. Idempotent MERGE logic supports safe batch reprocessing.
4. Watermarks must advance only after successful processing.
5. Incremental pipelines require additional controls for late-arriving data, validation, audit logging and failure recovery.

### Next Stage
**Part 4 — Production Readiness:** Implement data-quality validation, execution audit logs, failure handling, monitoring, alerts and recovery controls.
