## Part 1 — Dynamic File Ingestion with Microsoft Fabric

### Objective
Build a reusable ingestion pipeline capable of discovering and processing multiple source files dynamically without creating separate copy activities for each dataset.

### Architecture
**Get Metadata → ForEach → If Condition → Copy Activity**

### Implementation
- **Get Metadata:** Discovers source files from the Lakehouse landing folder using `childItems`.
- **ForEach:** Iterates through the discovered files using dynamic expressions.
- **If Condition:** Validates that each item is a CSV file.
- **Copy Activity:** Copies eligible files into Bronze storage using a dynamically resolved filename.
- **Run-specific folders:** Uses `pipeline().RunId` to isolate files by execution and preserve ingestion history.

### Testing and Validation
- Successfully ingested four synthetic retail datasets using one Copy Activity definition.
- Re-ran the pipeline to investigate overwrite behaviour and idempotency.
- Introduced run-specific Bronze folders to retain files from individual executions.
- Simulated missing source files to examine failure propagation through nested activities.
- Observed that individual Copy Activities failed and the parent ForEach ultimately reported failure.

### Engineering Takeaways
1. Dynamic file discovery reduces hard-coded pipeline dependencies.
2. Run-level isolation improves traceability but can create redundant storage.
3. Preventing duplicate file storage does not automatically prevent duplicate downstream processing.
4. Reliable production pipelines require configuration-driven controls, audit logging and intelligent retry mechanisms.

### Next Stage
Part 2 introduces metadata-driven orchestration using configuration tables, Lookup activities, pipeline parameters and reusable child pipelines.
