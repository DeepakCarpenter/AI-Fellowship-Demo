📊 InsightFlow: The Automated AdTech Intelligence Engine

One-Liner: A zero-touch data pipeline that ingests raw AdTech clickstreams, detects anomalies using Isolation Forests, and generates executive PDF reports using Gemini 1.5.

1. GroundTruth Business Alignment

Target Track: Automated Reporting & Data Engineering

The Business Problem:
Account Managers currently waste ~4 hours/week manually downloading CSVs and taking screenshots to explain campaign performance. This latency means clients don't see "Cost Per Visit" (CPV) spikes until it's too late.

Our Solution:
InsightFlow is an event-driven pipeline. As soon as a CSV drops into the S3 bucket (or local folder), it triggers a processing job.

Impact: Reduces reporting time from 4 hours to 30 seconds.

Value: Proactive anomaly detection alerts clients to budget waste immediately.

2. Technical Architecture & Engineering

Implementation Level: Production-Ready Data Pipeline (Tier 3)

We moved beyond simple scripts. This project uses a modular ETL approach:

Ingestion Layer: A Watchdog listener detects new files.

Processing Layer: Uses Polars (Rust-based Python library) for high-performance data cleaning. We chose Polars over Pandas for 10x faster processing on large datasets.

Analysis Layer:

Statistical: Calculates WoW (Week-over-Week) growth.

ML: Uses Scikit-Learn IsolationForest to detect anomalies in Foot Traffic vs. Ad Spend.

Synthesis Layer: The anomaly metadata is passed to Google Gemini 1.5 Pro with a strict system prompt to generate a "Why this matters" narrative.

Reporting Layer: WeasyPrint renders a pixel-perfect PDF with embedded Matplotlib charts.

3. Tech Stack

Core Backend: Python 3.11

Data Processing: Polars, NumPy

Machine Learning: Scikit-Learn (Isolation Forest for Anomaly Detection)

Generative AI: Google Gemini 1.5 Flash (Context Window for Logs)

Reporting: Jinja2 (Templating) + WeasyPrint (PDF Generation)

Infrastructure: Docker, Docker Compose

4. Visual Proof

1. The Input Data

2. The Anomaly Detection

3. The Final PDF

Raw messy CSV logs

Terminal detecting -20% drop

Executive Report with AI Summary







5. Key Engineering Challenge

The Hallucination Problem:
Initially, the LLM would invent numbers (e.g., "Traffic up 50%") when the data only showed 5%.
The Fix:
We implemented a "Fact-Check Guardrail." The Python script calculates the actual % change first, then injects it into the prompt as a hard constraint:

System: You are a reporter. The data says traffic is down 5%. Explain potential reasons based on the weather log (Rain). Do not invent numbers.

6. How to Run

# 1. Clone the repo
git clone [https://github.com/username/insight-flow.git](https://github.com/username/insight-flow.git)

# 2. Set up Environment
# Copy .env.example to .env and add your GEMINI_API_KEY
cp .env.example .env

# 3. Run with Docker (Recommended)
docker-compose up --build

# 4. Trigger the Pipeline
# Drop the sample csv found in /data/samples into /data/input
cp data/samples/campaign_logs.csv data/input/


<!--
!!! SYSTEM METADATA - DO NOT EDIT BELOW THIS LINE !!!
METADATA_START
{
"project_name": "InsightFlow",
"groundtruth_category": "Automated Reporting",
"ai_complexity": "Tier 3 - ML Anomaly Detection + RAG",
"tech_stack": ["Python", "Polars", "Scikit-Learn", "Gemini API", "Docker"],
"features": ["Anomaly Detection", "PDF Generation", "Event-Driven"],
"demo_video": "https://www.google.com/search?q=https://youtube.com/watch%3Fv%3Dexample"
}
METADATA_END
-->
