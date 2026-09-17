> **Download source project:** The `outputs/`, `work/`, `docs/` folders and `requirements.txt` described below are inside [Data_Cloud_Operations_GitHub_Portfolio.zip](Data_Cloud_Operations_GitHub_Portfolio.zip). Extract the ZIP first, then run the reproduction commands from the extracted directory. The ready-to-open TWBX is also available separately above.

# Enterprise Data Cloud Operations

A Tableau portfolio project exploring the reliability of Salesforce Data Cloud-style ingestion, audience segmentation, and activation operations using entirely synthetic data.

**Live dashboard:** [Explore the interactive Tableau dashboard](https://public.tableau.com/app/profile/pavankumar.sindhuri/viz/EnterpriseDataCloudOperations/01ExecutiveHealth)

The GitHub TWBX is the CSV-based authoring version. The Tableau Public version uses extracts created during publication. For Public Edition, use the live link; the original local workbook may require extract conversion.

## Business questions

- Which data streams are failing or missing refresh commitments?
- Which business-critical streams need investigation?
- Are segment failures affecting activation readiness?
- Which sources and error categories drive failed processing attempts?

## Scope

83 streams, including 20 critical streams; 30 segments; 45 activations; 180 days from 19 March to 14 September 2026. Four dashboards and 34 supporting worksheets.

| Dashboard | Purpose |
|---|---|
| Executive Health | Stream health, run success, SLA compliance and accepted volume |
| Critical Streams | Critical stream status, SLA exceptions and refresh details |
| Segments and Activations | Audience population trends, failed segments and activation delivery |
| Operations Analysis | Error categories, source failures and selected-stream execution history |

## Open the dashboard

Download **Enterprise Data Cloud Operations.twbx** and open it in Tableau Public Edition or a compatible Tableau Desktop installation. The workbook was built for Tableau 2026.2 and contains four packaged CSV sources. No Snowflake account is required.

Start with **01 Executive Health**. Default date: 14 September 2026; trend window: 30 days. Scroll vertically on the taller detail dashboards.

## Modeling and metrics

The dataset includes dimensions, run-level facts, refresh obligations, daily snapshots and stream-to-segment/segment-to-activation bridges. The workbook uses prepared daily sources and enriched run history for convenient local authoring; it does not implement a live Salesforce or Snowflake connection.

- Stream health is a daily snapshot; failures count execution attempts, including attempts later recovered by retry.
- SLA compliance is obligations met divided by obligations due. Late completion can remain a breach.
- Record rejection rate is rejected records divided by accepted plus rejected records.
- Average refresh duration includes successful runs only.
- Failed segment population retains the last successful value. Populations can overlap and must not be interpreted as unique customer totals.
- Source/scope filters affect stream analyses, not segment/activation data. On Operations Analysis, Stream changes only the lower failure trend and execution log.

## Validation

CSV consistency checks, packaged source fields, worksheet references and workbook structure were checked. User-provided Tableau screenshots were used to review the layout. Default values reconcile to the synthetic data:

| Measure | Value |
|---|---:|
| Total / critical streams | 83 / 20 |
| Healthy / warning / failed streams | 70 / 6 / 7 |
| Successful segments | 20 / 30 (66.7%) |
| Successful activations | 31 / 45 (68.9%) |
| Failed attempts, last 30 days | 169 |
| SLA breaches, last 30 days | 303 |
| Average successful refresh, last 30 days | 31.2 minutes |
| Record rejection, last 30 days | 0.39% |

Interactive filter behavior and the published browser version still require a final smoke test. Status text is authoritative; Tableau currently renders its default categorical colors. Desktop-sized layouts are provided; a dedicated phone layout is not included.

## Repository contents

- `Enterprise Data Cloud Operations.twbx`: packaged dashboard workbook.
- `outputs/data_cloud_step2/`: synthetic CSV tables, dictionary and generation notes.
- `work/`: dataset generation, workbook construction and validation scripts.
- `docs/PUBLISHING.md`: publication steps and portfolio description.

## Reproduce locally

Use Python 3 with the dependencies in `requirements.txt`, then run from the repository root:

```sh
python -m pip install -r requirements.txt
python work/build_dataset.py
python work/validate_dataset.py
python work/build_tableau.py
python work/check_tableau.py
```

The workbook builder writes a new TWBX under `outputs/`. Review generated workbooks in Tableau before publishing. The XML builder is version-specific; Tableau itself is required for interactive rendering and publication.

## Portfolio context

This project demonstrates operational KPI design, dimensional data concepts, Tableau calculations and parameters, exception analysis, and iterative dashboard QA. Data and workbook scaffolding were developed with AI assistance. This is a learning/portfolio demonstration, not a production Salesforce implementation or evidence of measured business savings. No employer, client or real customer records are included.
