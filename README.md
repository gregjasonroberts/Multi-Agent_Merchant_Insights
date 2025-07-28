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

This framework addresses the merchant services team's need to deliver personalized upsell recommendations by analyzing daily performance metrics and peer benchmarks, ensuring each suggestion aligns with the merchant's unique trends and goals. By simulating an intelligent business advisor with specialized agents collaborating, merchants gain actionable, context-aware insights that drive tangible improvements in revenue and engagement.

Leveraging an agentic AI approach, each component from trend analysis to LLM-driven plan matching operates autonomously yet cohesively, producing explainable and adaptive recommendations that evolve with the business context.

## Table of Contents

* [Architecture](#architecture)
* [Getting Started](#getting-started)
* [Directory Structure](#directory-structure)
* [Agents](#agents)
* [Project Status](#project-status)


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
┌──────────▼─────────────────────────────────────┐
│Trend & Peer Agents                             │
│(SalesTrend, TransactionTrend, ATS, PeerStats,  │
│ WeekdayProfile)                                │
└──────────┬─────────────────────────────────────┘
           │
┌──────────▼─────────────┐
│InsightManagerAgent     │
└──────────┬─────────────┘
           │
┌──────────▼───────────────────────┐
│RecommendationManagerAgent        │
│(PlanMatching, Author, Validator) │
└──────────┬───────────────────────┘
           │
┌──────────▼─────────────────┐
│RecommendationCritiqueAgent │
└────────────────────────────┘
```

## Getting Started

### Prerequisites

* pandas>=1.5.3
* numpy>=1.21.0
* fpdf>=1.7.2
* pdfplumber>=0.10.2
* python-dateutil>=2.8.2
* matplotlib>=3.5.0
* seaborn>=0.11.2


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
| Data Ingestion           |  Completed    |
| Trend & Peer Analysis    |  Completed    |
| Insight Generation       |  Completed    |
| Plan Matching            |  Completed    |
| Recommendation Authoring |  Completed    |
| Recommendation Critique  |  Completed    |
| Colab Prototype          |  In Progress  |
| Dashboard Integration    |  Planned      |


