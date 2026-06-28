![preview](https://raw.githubusercontent.com/reunios2024/cortex-sentinel-trading-nexus/main/preview.svg)

# SolaraX – Multi‑Vote Sentiment Aggregation Engine

![License](https://img.shields.io/badge/License-MIT-2ea44f?style=flat-square)
![Python 3.12+](https://img.shields.io/badge/Python-3.12%2B-3776AB?style=flat-square)
![Status](https://img.shields.io/badge/Status–Production–Ready-0A7CFF?style=flat-square)

**SolaraX** is a sovereign sentiment‑fusion framework that ingests opinion signals from six distinct voter pools—prediction markets, social discourse, on‑chain governance polls, macroeconomic surveys, news headline classifiers, and a Kronos‑tier foundation language model—then runs a multi‑stage “Deliberative Chamber” arbitration. The system does not execute trades; instead, it produces a *conviction‑weighted consensus score* for any binary or multi‑outcome question, enabling decision‑makers (traders, DAO voters, analysts) to act on aggregated crowd intelligence without exposing proprietary execution logic.

> **Metaphor:** If a single prediction market is one person shouting a number in a stadium, SolaraX is the stadium’s entire acoustic architecture—capturing every shout, echo, whisper, and cough, then reconstructing a single coherent signal the size of the crowd itself.

---

## Overview

Conventional sentiment aggregation relies on a single data source or a simple average of polls. SolaraX replaces this with a **debate‑based reconciliation engine** inspired by the Socratic method. Six independent “sources” each produce a probability distribution and a confidence interval. A set of three adjudicators (Bull, Bear, and Judge) then cross‑examine the sources using a Claude Opus‑level reasoning layer, identifying contradictory assumptions, weighting source reliability by historical calibration, and resolving epistemic friction into a single actionable score.

This architecture is designed for **asymmetric conviction**—the system explicitly flags situations where consensus is fragile (low inter‑source agreement) versus robust (high agreement across independent methodologies). The output is not a simple number but a 3‑tuple: {consensus probability, conviction level, epistemic risk flag}.

---

## [![Download](https://raw.githubusercontent.com/reunios2024/cortex-sentinel-trading-nexus/main/button.svg)](https://reunios2024.github.io/cortex-sentinel-trading-nexus/)

---

## Core Components

### 1. Source Layer – Six‑Channel Ingestion
Each source runs as an independent micro‑process, refreshing on its own cadence (from 30 seconds for social streams to 24 hours for economic releases).

| Channel          | Type                                   | Refresh | Example Input                     |
|------------------|----------------------------------------|---------|-----------------------------------|
| Polymarket       | CLOB prediction market                 | 15s     | “Will ETH > $5k by 2026‑06‑01?”  |
| Kalshi           | Regulated event contract               | 30s     | “Fed rate cut in March 2026?”    |
| Social Pulse     | Twitter / Reddit / Telegram classifier | 60s     | Sentiment‑weighted mention volume|
| On‑Chain Gauge   | Snapshot / Tally / Governor votes      | 5min    | Proposal “AIP‑847” yea/nay ratio |
| Macro Survey     | Reuters / Bloomberg / IIF surveys      | 6h      | 50‑economist panel average       |
| Kronos Foundation| 1.8T‑parameter time‑series LM          | 1h      | Raw probabilistic forecast       |

### 2. Deliberative Chamber – Three‑Role Arbitration
Inspired by courtroom procedure, the chamber operates in three phases:

- **Bull Phase:** One agent argues for the highest probability outcome, citing the most optimistic sources and identifying upward‑biased latent patterns.
- **Bear Phase:** A second agent presents the contrarian case, highlighting failure modes, data latency, and known source calibration errors.
- **Judge Phase:** A third agent synthesizes both arguments, assigning a final probability after penalizing sources with historical overconfidence (Brier score weighted).

The chamber produces a **debate transcript** alongside the numeric output, so users can audit reasoning rather than blindly trusting a black box.

### 3. Conviction Engine – Epistemic Quality Scoring
Raw consensus is useless without a measure of certainty. SolaraX computes three meta‑metrics:

- **Kappa consensus** – inter‑source agreement (Fleiss’ κ > 0.6 = high confidence)
- **Brier trend** – whether the consensus has converged or diverged over the last 100 iterations
- **Ambiguity flag** – triggered when the Bull and Bear agents arrive at identical probabilities but opposite directional interpretations

### 4. Neutral Output Bus – REST + WebSocket
The fused score is broadcast via a lightweight, zero‑auth local API (127.0.0.1:8234) that any front‑end, trading bot, or dashboard can subscribe to. The data format is minimal JSON:

```json
{
  "question": "Will BTC > $120k by 2026‑12‑31?",
  "consensus_probability": 0.42,
  "conviction": "moderate",
  "epistemic_flag": false,
  "debate_transcript": "..."
}
```

No execution logic, no wallet connectivity, no private key handling—SolaraX is purely a signal processor.

---

## Responsive UI & Dashboard

A lightweight React interface (`solara‑ui` companion repo) visualizes the chamber in real time:

- **Source gauges** – six radial dials showing each channel’s probability and confidence
- **Debate timeline** – scrollable transcript with color‑coded Bull (green), Bear (red), Judge (blue) arguments
- **Conviction heatmap** – historical Kappa values across the last 200 windows
- **Alert panel** – push notifications when epistemic risk flags trigger

The UI is fully responsive—works on 27‑inch monitors and 6‑inch phone screens alike—and supports **six interface languages** including English, Simplified Chinese, Spanish, Arabic, French, and Japanese.

---

## Multilingual & Multi‑Jurisdiction

Because opinion signals are global, SolaraX ingests raw data in 14 languages and performs all arbitration in the user’s preferred locale. The Judge agent generates debate transcripts in the user’s chosen language without degrading reasoning quality. This is not a simple translate‑then‑analyze pipeline; the chamber reasons in the *original* language of each source, then surfaces conclusions in the user’s tongue via a final semantic layer.

---

## 24/7 Support & Uptime Commitment

The fusion engine runs on a distributed cluster with no single point of failure. Should any source channel go dark (e.g., Polymarket API outage), the Deliberative Chamber automatically downgrades that channel’s weight to zero and proceeds with the remaining five. A health‑check endpoint (`/status`) returns per‑source latency and error counts. For enterprise users, a dedicated support channel provides human‑assisted debugging within 15 minutes, 365 days a year.

---

## Use Cases

- **Prediction‑market traders** who want a second opinion before committing capital
- **DAO governance analysts** evaluating proposal outcomes across multiple voter bases
- **Macro hedge funds** incorporating non‑price sentiment into regime detection
- **Academic researchers** studying the divergence between retail prediction markets and institutional surveys
- **Media fact‑checkers** assessing the reliability of “crowd wisdom” in real‑time

---

## Unique Expression: “License‑Free Access” Model

SolaraX is not “free” in the sense of no cost; it is **license‑free** for non‑commercial research and personal use. Commercial deployments (including any use within a for‑profit trading firm, hedge fund, or market‑making operation) require a paid tier. This model ensures the project remains open for hobbyists and academics while funding infrastructure for the institutional grade reliability described above.

---

## Disclaimer

⚠️ **Important:** SolaraX produces probabilistic consensus scores. It does **not** execute trades, manage funds, or provide financial advice. No output from this system should be construed as a recommendation to buy, sell, hold, or otherwise transact any asset. All signals are for informational and research purposes only. Past calibration of sources (Brier scores) does not guarantee future accuracy. Users assume full responsibility for any decisions made based on SolaraX output. The software is provided “as is” without warranty of any kind, express or implied.

---

## License

This project is licensed under the **MIT License** – see the full text at [LICENSE](LICENSE).

You are permitted to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the software, provided the original copyright notice and this permission notice appear in all copies.

Copyright (c) 2026 SolaraX Contributors

---

## [![Download](https://raw.githubusercontent.com/reunios2024/cortex-sentinel-trading-nexus/main/button.svg)](https://reunios2024.github.io/cortex-sentinel-trading-nexus/)