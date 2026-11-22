## Part 2 — Metadata-Driven Pipeline Orchestration

### Objective
Build a configuration-driven ingestion framework in Microsoft Fabric, allowing datasets to be enabled or disabled without modifying pipeline logic.

### Architecture
**Delta Configuration Table → Lookup → Filter → ForEach → Invoke Pipeline (Legacy) → Child Copy Activity**

### Implementation
- **Delta Configuration Table:** Created `config_ingestion` using PySpark, storing dataset names, source/destination paths, active flags and load types.
- **Lookup:** Retrieves ingestion configurations from the Lakehouse.
- **Filter:** Selects datasets where `is_active = true`.
- **ForEach:** Iterates through enabled datasets.
- **Invoke Pipeline (Legacy):** Calls a reusable child pipeline and passes dataset-specific parameters.
- **Child Pipeline:** Dynamically copies source files into run-specific Bronze folders.

### Testing & Validation
- Successfully processed three enabled datasets while excluding the inactive `order_items` dataset.
- Updated `is_active` using SQL to enable the fourth dataset.
- Reran the master pipeline successfully, processing all four datasets **without changing any pipeline activities**.
- Validated parameter passing and parent-child orchestration.

### Engineering Takeaways
1. Configuration-driven orchestration reduces hard-coded pipeline dependencies.
2. Reusable child pipelines simplify maintenance and dataset onboarding.
3. Delta tables provide centralised configuration management.
4. Run-specific folders improve traceability but require additional controls to avoid redundant storage.

### Next Stage
**Part 3 — Incremental Loading:** Implement watermarks, PySpark transformations and Delta MERGE to support inserts, updates and idempotent processing.
