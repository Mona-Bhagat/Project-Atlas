# Project-Atlas
Metadata-Driven Data Engineering with Microsoft Fabric

A four-stage portfolio project covering dynamic file ingestion, configuration-driven orchestration, incremental Delta Lake processing, and production-style validation/monitoring.

## Architecture
Synthetic retail CSV → Lakehouse Files/landing → Fabric pipeline → Files/bronze → PySpark → Delta Tables/silver → Delta Tables/gold. Configuration, watermarks and audit tables coordinate operations.


# Build sequence
1. **Part 1:** Get Metadata → ForEach → If Condition → Copy (initial CSVs).
2. **Part 2:** Lookup configuration → parameterised child pipeline.
3. **Part 3:** Incremental batches, watermarks, deduplication and Delta MERGE.
4. **Part 4:** Validation, audit trail, alerts, simulated failures and recovery.
