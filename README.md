# multi-agent\_merchant\_insight

![Project Status](https://img.shields.io/badge/status-beta-orange)

## Overview

**multi-agent\_merchant\_insight** is a prototype framework for an LLM-driven, agentic recommendation system targeting merchant services. It synthesizes custom data (merchant daily stats, metadata, service plan matrices) and uses a hierarchy of specialized agents to:

1. **Ingest** raw inputs (PDFs, CSVs, DataFrames)
2. **Analyze** trends and peer comparisons
3. **Compose** human-readable insights
4. **Match** merchants to optimal service plans
5. **Generate** actionable recommendations
6. **Critique** for concise, customer-ready messaging

## Table of Contents

* [Architecture](#architecture)
* [Getting Started](#getting-started)
* [Directory Structure](#directory-structure)
* [Usage](#usage)

  * [Data Generation](#data-generation)
  * [Running the Pipeline](#running-the-pipeline)
* [Agents](#agents)
* [Project Status](#project-status)
* [Contributing](#contributing)
* [License](#license)

## Architecture

```text
┌────────────────────────┐
│Data Ingestion Agents   │
│(Plan, Metadata, Stats, │
│ Segmentation)          │
└──────────┬─────────────┘
           │
┌──────────▼─────────────┐
│OrchestratorAgent       │
└──────────┬─────────────┘
           │
┌──────────▼─────────────┐
│Trend & Peer Agents     │
│(SalesTrend, TransactionTrend, ATS, PeerStats,│
│ WeekdayProfile)        │
└──────────┬─────────────┘
           │
┌──────────▼─────────────┐
│InsightManagerAgent     │
└──────────┬─────────────┘
           │
┌──────────▼─────────────┐
│RecommendationManagerAgent│
│(PlanMatching, Author, Validator)│
└──────────┬─────────────┘
           │
┌──────────▼─────────────┐
│RecommendationCritiqueAgent │
└────────────────────────┘
```

## Getting Started

### Prerequisites

* Python 3.8 or higher
* `pip` package manager
* OpenAI API Key (set `OPENAI_API_KEY` in your environment)

### Installation

```bash
# Clone the repo
git clone https://github.com/<your-username>/multi-agent_merchant_insight.git
cd multi-agent_merchant_insight

# Create a virtual environment and activate it
python -m venv venv
source venv/bin/activate     # Linux/macOS
venv\Scripts\activate      # Windows

# Install dependencies
pip install -r requirements.txt
```

## Directory Structure

```
multi-agent_merchant_insight/
├── agents/                  # Python modules for each agent
│   ├── ingestion.py         # Plan, Metadata, Stats, Segmentation
│   ├── analysis.py          # Trend, Peer, Insight Manager
│   ├── matching.py          # LLMPlanMatcherAgent
│   ├── critique.py          # RecommendationCritiqueAgent
│   └── orchestrator.py      # OrchestratorAgent, DataIngestionManager
├── data/                    # Sample/synthesized input data
├── notebooks/               # Colab prototype notebooks
│   └── prototype.ipynb      # End-to-end demo
├── scripts/                 # Utility scripts (e.g. data generation)
├── requirements.txt         # Python dependencies
└── README.md                # Project documentation
```

## Usage

### Data Generation

Generate synthetic merchant stats and plan matrices in Colab:

```python
from scripts.generate_data import generate_synthetic_data
stats_df, merchant_df, plan_matrix = generate_synthetic_data(...)
```

### Running the Pipeline

```bash
python -m scripts.run_pipeline \
  --stats data/merchant_stats.csv \
  --metadata data/merchant_metadata.csv \
  --segments data/all_merchants_segments.csv \
  --plan-matrix data/service_plan_matrix.pdf
```

Or in Python:

```python
from agents.orchestrator import DataIngestionManagerAgent, OrchestratorAgent

ingestor = DataIngestionManagerAgent(...)
data = ingestor.run_ingestion()

orc = OrchestratorAgent(data)
insights = orc.run_insights()

recommendations = orc.run_recommendations(insights)
```

## Agents

| Agent                         | Input                      | Output                   |
| ----------------------------- | -------------------------- | ------------------------ |
| PlanIngestionAgent            | PDF                        | `plan_matrix`            |
| MetadataIngestionAgent        | CSV                        | `merchant_df`            |
| DailyStatsIngestionAgent      | CSV/DataFrame              | `stats_df`               |
| AllMerchantsSegmentationAgent | CSV                        | `all_merchants_df`       |
| DataIngestionManagerAgent     | Above 4 agents             | `data_bundle`            |
| OrchestratorAgent             | `data_bundle`              | -                        |
| SalesTrendAgent               | `stats_df`                 | `sales_trends`           |
| TransactionTrendAgent         | `stats_df`                 | `transaction_trends`     |
| ATSAgent                      | `stats_df`                 | `ats_trends`             |
| PeerStatsAgent                | `stats_df`                 | `peer_trends`            |
| WeekdayProfileAgent           | `stats_df`                 | `weekday_profiles`       |
| InsightManagerAgent           | trends + peer\_trends      | `insights`               |
| PlanMatchingAgent             | `insights`, `plan_matrix`  | `matched_plan`           |
| RecommendationAuthorAgent     | `matched_plan`, `insights` | `verbose_recommendation` |
| RecommendationValidatorAgent  | `verbose_recommendation`   | validation status/fixes  |
| RecommendationCritiqueAgent   | `verbose_recommendation`   | `concise_recommendation` |

## Project Status

| Milestone                | Status         |
| ------------------------ | -------------- |
| Data Ingestion           | ✅ Completed    |
| Trend & Peer Analysis    | ✅ Completed    |
| Insight Generation       | ✅ Completed    |
| Plan Matching            | ✅ Completed    |
| Recommendation Authoring | ✅ Completed    |
| Recommendation Critique  | ✅ Completed    |
| Colab Prototype          | 🟡 In Progress |
| Dashboard Integration    | ⏳ Planned      |

## Contributing

Contributions are welcome! Please open issues or pull requests for:

* Bug fixes
* New agents or analysis modules
* Improved prompts or thresholds

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
