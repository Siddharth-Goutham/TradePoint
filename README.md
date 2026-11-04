# 📈 TradePoint – Context-Aware Trading Assistant

**TradePoint** is a modular, agent-driven financial platform that
automates a retail trader’s daily research and execution. It scrapes
unstructured market news, applies LLM sentiment analysis, merges it with
historical NSE price action via an ML pipeline, and automatically
executes Good Till Triggered (GTT) orders via the Zerodha Kite API.

The project explores the PayPal Hackathon challenge by integrating the
PayPal REST API to monetize this infrastructure as a seamless SaaS
product.

> **Note:** This is an open-source development project built for
> demonstration purposes. It does not provide official financial advice,
> guarantee profits, or claim production readiness for real-money
> trading.

**GitHub Repository:**
[TradePoint](https://github.com/Siddharth-Goutham/TradePoint)

------------------------------------------------------------------------

## 📑 Table of Contents

- [Why TradePoint?](#-why-tradepoint)
- [Features and Status](#-features-and-status)
- [Architecture](#-architecture)
- [Technology Stack](#-technology-stack)
- [Repository Structure](#-repository-structure)
- [Installation and Configuration](#-installation-and-configuration)
- [Quickstart](#-quickstart)
- [Dashboard and Deployment](#-dashboard-and-deployment)
- [Trust, Verification, and
  Limitations](#-trust-verification-and-limitations)
- [ML Pipeline Evaluation](#-ml-pipeline-evaluation)
- [Data Privacy and Responsible Use](#-data-privacy-and-responsible-use)
- [Current Constraints and Roadmap](#-current-constraints-and-roadmap)
- [GitHub Issues Roadmap](#-github-issues-roadmap)
- [Open Source and Community](#-open-source-and-community)

------------------------------------------------------------------------

## 🎯 Why TradePoint?

Retail traders face immense friction when managing isolated tools for
market news, quantitative analysis, and trade execution.

Reviewers and traders also need to assess whether market sentiment
supports a specific trade direction without being blinded by noise.

TradePoint provides repeatable preprocessing, AI-powered sentiment
scoring, and evidence-based probability checks to help traders automate
their daily workflows.

The pipeline automates:

- Headless financial news extraction.
- LLM-based sentiment scoring.
- Machine learning probability generation.
- Conditional broker execution.

Engineers and traders still need to configure strict risk parameters and
capital limits.

A generated trade signal is a statistical triage signal, not a guarantee
of profitable trading.

The open-source goal is to make the data contracts, ML features, and
LangGraph multi-agent orchestration inspectable and reusable.

------------------------------------------------------------------------

## 🚀 Features and Status

| Area               | Features                                                                                                 |
|--------------------|----------------------------------------------------------------------------------------------------------|
| Data Extraction    | Headless Selenium scraper for extracting financial news and storing it in a shared SQL database.         |
| Orchestration      | Stateful LangGraph multi-agent workflow containing Scraper, Quant, Risk, and Execution agents.           |
| Sentiment Analysis | Configurable LLM client for classifying market news into bullish, bearish, and neutral sentiment scores. |
| Predictive ML      | Scikit-Learn and XGBoost pipeline combining historical NSE data with LLM sentiment scores.               |
| Market Operations  | Flask routes handling Zerodha Kite Connect OAuth and generating GTT orders.                              |
| Monetization       | PayPal REST API integration using Smart Buttons for SaaS subscriptions.                                  |
| Reports & UI       | Jinja/Bootstrap dashboard displaying financial news, sentiment scores, and active trades.                |

------------------------------------------------------------------------

## 🏗️ Architecture

TradePoint follows a decoupled architecture with two primary processes:

1.  **Flask Backend:** Handles the user interface, APIs, authentication,
    and market operations.
2.  **AI Pipeline:** Handles news scraping, multi-agent orchestration,
    sentiment analysis, and ML predictions.

The database acts as the shared handoff layer between the scraper and
the Flask dashboard.

### Key Design Decisions

**1. Decoupled Scraping**

The Selenium scraper runs independently of the Flask application. This
ensures data ingestion can continue even if the daily Zerodha API token
expires.

**2. Database Handoff**

Extracted financial news is stored directly in a shared database table.
The Flask dashboard queries this table to display the in-app news
column.

**3. Multi-Agent State Management**

LangGraph maintains a structured state dictionary. The Execution Agent
cannot proceed unless the Risk Agent validates the `trade_approved`
boolean based on user-defined parameters.

**4. Subscription Management**

Core AI agent routes in Flask check for a valid `is_subscribed` flag
managed through PayPal webhooks.

------------------------------------------------------------------------

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| Python 3.9+ | Core programming language |
| Flask | Backend framework and API routes |
| LangGraph | Multi-agent orchestration |
| Scikit-Learn | Machine learning pipeline |
| XGBoost | Predictive modeling |
| Selenium | Automated financial news scraping |
| SQLAlchemy | Database ORM |
| SQLite / PostgreSQL | Database management |
| OpenAI Python SDK | LLM-based sentiment analysis |
| Zerodha Kite Connect API | Broker authentication and GTT orders |
| PayPal REST API | SaaS subscription management |
| Jinja2 | Server-side templating |
| Bootstrap | Dashboard UI |

------------------------------------------------------------------------

## 📂 Repository Structure

``` text
TradePoint/
│
├── app/                    # Flask application, UI templates, and API routes
├── agents/                 # LangGraph nodes (Quant, Risk, Execution)
├── core/                   # SQLAlchemy models and database schemas
├── ml/                     # XGBoost pipelines and preprocessors
├── scraper/                # Standalone Selenium headless news scraper
├── tests/                  # Unit tests for APIs, agents, and data contracts
├── .env.example            # Environment variable template
├── requirements.txt        # Python dependencies
└── README.md               # Project documentation
```

------------------------------------------------------------------------

## ⚙️ Installation and Configuration

The project baseline is Python 3.9+.

### 1. Clone the Repository

``` bash
git clone https://github.com/Siddharth-Goutham/TradePoint.git
cd TradePoint
```

### 2. Create a Virtual Environment

**Linux / macOS**

``` bash
python3 -m venv .venv
source .venv/bin/activate
```

**Windows PowerShell**

``` powershell
py -3 -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 3. Install Dependencies

``` bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### 4. Configure Environment Variables

The application reads configuration values and API credentials from a file named `.env`.

**Step 1: Create your `.env` file**

Run the command for your operating system from the project root directory.

**Linux / macOS**

```bash
cp .env.example .env
```

**Windows PowerShell**

```powershell
copy .env.example .env
```

This creates a new `.env` file by copying the example configuration. Open `.env` in your code editor.

**Step 2: Add your credentials**

Replace the placeholder values with your own credentials. Do not add quotation marks unless the value itself requires them.

```env
KITE_API_KEY=your_zerodha_api_key
KITE_API_SECRET=your_zerodha_api_secret
LLM_API_KEY=your_llm_api_key
PAYPAL_CLIENT_ID=your_paypal_client_id
FLASK_APP=app
```

| Variable | What to enter |
|---|---|
| `KITE_API_KEY` | Your Zerodha Kite Connect API key |
| `KITE_API_SECRET` | Your Zerodha Kite Connect API secret |
| `LLM_API_KEY` | Your LLM provider API key |
| `PAYPAL_CLIENT_ID` | Your PayPal REST API client ID |
| `FLASK_APP` | The import path of your Flask application entry point; use the value matching your project structure |

**Step 3: Save the file**

Save `.env` in the root folder of the project, alongside `requirements.txt`.

> **Important:** Never upload `.env` to GitHub. It contains private credentials. Keep `.env` listed in `.gitignore`, and share only `.env.example` with placeholder values.

------------------------------------------------------------------------

## ▶️ Quickstart

After installation and environment configuration, initialize the
database and start both application processes.

### 1. Initialize the Database

``` bash
flask db upgrade
```

### 2. Start the Flask Application

Open the first terminal:

``` bash
flask run
```

Visit `http://localhost:5000` to view the dashboard and trigger the
Zerodha login flow.

### 3. Start the AI Pipeline

Open a second terminal:

``` bash
python scraper/daily_news_scraper.py
```

The scraper and AI pipeline run independently of the Flask application.

------------------------------------------------------------------------

## 📊 Dashboard and Deployment

The **Reports & UI** item refers to the application dashboard—not a
separate reports module. The dashboard is built using server-side Jinja
templates and displays:

- Scraped financial news.
- AI-generated sentiment scores.
- Active trade signals.
- Relevant market information.

The application is designed to be containerized using Docker and
deployed on platforms such as:

- Render
- AWS EC2

The repository currently defaults to local execution for hackathon
demonstration purposes.

To test the PayPal subscription flow locally, use PayPal Sandbox
credentials in your `.env` file.

------------------------------------------------------------------------

## 🛡️ Trust, Verification, and Limitations

### Deterministic Checks and Model Assistance

- Sentiment generation extracts bullish and bearish claims from
  pre-scraped financial news.
- Validators do not establish that a cited article guarantees market
  movement.
- ML probability scoring is model-assisted and heuristic.
- Missing scores or low-confidence predictions should default to
  inaction.
- The Risk Agent enforces deterministic checks, such as whether a trade
  exceeds 2% of total capital.
- A failed deterministic risk check prevents execution even if the Quant
  Agent expresses high confidence.

### Current Limitations

1.  Headless scraping is sensitive to changes in target website DOM
    structures.
2.  Zerodha access tokens require a daily login flow through the
    frontend.
3.  Full autonomous operation across multiple days requires manual user
    intervention every morning.
4.  ML models trained on historical data may experience concept drift in
    live markets.

------------------------------------------------------------------------

## 📈 ML Pipeline Evaluation

The ML pipeline evaluates prediction accuracy against historical NSE
datasets.

Evaluation metrics include:

- Accuracy
- Precision
- Recall
- F1-score

The final evaluation results will be documented in:

``` text
ml/evaluation_results.md
```

Evaluation details are pending final training passes.

------------------------------------------------------------------------

## 🔐 Data Privacy and Responsible Use

- Committed databases, if any, contain mock data.
- Real financial data may expose API keys, account balances, and
  personal information.
- Collect only necessary data and obtain appropriate permissions.
- Never commit `.env` files containing live Kite API secrets or PayPal
  production credentials.
- AI outputs may be incomplete, biased, or incorrect.
- Risk settings should be reviewed by a qualified person before enabling
  automated GTT execution.

> **Disclaimer:** This software is intended for educational and
> hackathon purposes. The creators are not responsible for financial
> losses incurred through deployment of this software in live markets.

------------------------------------------------------------------------

## 🗺️ Current Constraints and Roadmap

### Implemented

- Validated data ingestion.
- Headless Selenium news scraping.
- Decoupled database handoff.
- Jinja dashboard UI.
- PayPal Sandbox integration.
- LangGraph agent scaffolding.
- Flask blueprint structure.

### Tracked Separately

- Live cloud deployment.
- Finalizing XGBoost hyperparameter tuning.
- Advanced backtesting modules.

------------------------------------------------------------------------

# 🗺️ GitHub Issues Roadmap

The following roadmap can be used to create and track GitHub Issues for
the development team.

## Phase 1: Foundation & Data Plumbing

**Goal:** Get the database running, establish data flow, and configure
the foundational environments.

### Issue 1: \[Backend\] Initialize Flask Boilerplate & Database Schema

**Assignee:** Shreetham (Full-Stack & Market Ops)

**Description:** Set up the core application structure and shared
database.

**Tasks:**

- [ ] Set up the Flask application factory.
- [ ] Configure SQLite/PostgreSQL.
- [ ] Create SQLAlchemy models: User, News (title, link,
  sentiment_score), and Trade (status, symbol, GTT parameters).
- [ ] Create the base `layout.html` and `dashboard.html` Jinja
  templates.

### Issue 2: \[Data\] Build Headless Selenium News Scraper

**Assignee:** Shreeyansh (Data Pipeline & Public Web)

**Description:** Build the decoupled data ingestion pipeline.

**Tasks:**

- [ ] Write a decoupled Python script using Selenium in headless mode.
- [ ] Extract headlines, summaries, and URLs.
- [ ] Connect the script to the shared database.
- [ ] Insert scraped data directly into the News table.

### Issue 3: \[AI\] Setup ML Environment & Initial LLM Prompts

**Assignee:** Siddharth (AI & Quant Brain)

**Description:** Establish the machine learning environment and prompt
engineering.

**Tasks:**

- [ ] Set up Python dependencies for Scikit-Learn, XGBoost, and
  LangGraph.
- [ ] Draft system prompts for LLM sentiment analysis.
- [ ] Extract bullish/bearish signals from raw news text.
- [ ] Write a script to fetch raw news from the database and append
  dummy sentiment scores for UI testing.

------------------------------------------------------------------------

## Phase 2: Core Engine & User Interface

**Goal:** Connect the frontend to the APIs, build the multi-agent
workflow, and develop the public-facing pages.

### Issue 4: \[Backend\] Implement Zerodha Kite Daily OAuth Flow

**Assignee:** Shreetham (Full-Stack & Market Ops)

**Description:** Handle broker authentication and dashboard data
surfacing.

**Tasks:**

- [ ] Build the login route that redirects users to Zerodha Kite.
- [ ] Handle the callback route to capture the `request_token`.
- [ ] Exchange the `request_token` for the daily `access_token`.
- [ ] Store the token securely in the user’s session/database.
- [ ] Build the News Column UI on the dashboard.

**Assignee:** Shreeyansh (Data Pipeline & Public Web)

**Description:** Build public-facing marketing and email assets.

**Tasks:**

- [ ] Build the public `index.html` landing page explaining TradePoint.
- [ ] Design pricing tiers for PayPal integration.
- [ ] Use Jinja formatting to inject dynamic data.

### Issue 6: \[AI\] Build LangGraph State Workflow & XGBoost Pipeline

**Assignee:** Siddharth (AI & Quant Brain)

**Description:** Construct AI orchestration and predictive modeling.

**Tasks:**

- [ ] Define the LangGraph State dictionary.
- [ ] Store market data, sentiment, and risk parameters.
- [ ] Create the Quant Agent.
- [ ] Create the Risk Agent.
- [ ] Create the Execution Agent.
- [ ] Build the preliminary XGBoost pipeline using historical NSE data
  and dummy sentiment.
- [ ] Generate probability scores for price movement.

------------------------------------------------------------------------

## Phase 3: Execution & Monetization

**Goal:** Execute trades, process payments, and send notifications.

### Issue 7: \[Backend\] Implement GTT Order Execution via Kite API

**Assignee:** Shreetham (Full-Stack & Market Ops)

**Description:** Connect AI decisions to market order execution.

**Tasks:**

- [ ] Build the settings UI for users to define risk parameters.
- [ ] Allow users to configure maximum loss and target profit.
- [ ] Implement Python execution logic to receive JSON output from AI
  agents.
- [ ] Place Good Till Triggered (GTT) orders using the Kite Connect API.
- [ ] Update the Trade database table with order confirmation status.

**Assignee:** Shreeyansh (Data Pipeline & Public Web)

**Description:** Monetize the platform through subscription payments.

**Tasks:**

- [ ] Integrate PayPal REST API Smart Buttons into the landing page.
- [ ] Create a Flask webhook route for successful subscription payments.
- [ ] Update the User table subscription status.
- [ ] Email HTML digests to subscribed users.

### Issue 9: \[AI\] End-to-End Orchestration & Handoff Testing

**Assignee:** Siddharth (AI & Quant Brain)

**Description:** Connect all components for the final hackathon
demonstration.

**Tasks:**

- [ ] Connect LLM sentiment output to the `sentiment_score` database
  column.
- [ ] Connect the LangGraph Execution Agent to the GTT execution
  function.
- [ ] Test the complete workflow:

``` text
News Scraped
     |
     v
Sentiment Scored
     |
     v
Probability Calculated
     |
     v
Risk Validation
     |
     v
GTT Order Generated
```

------------------------------------------------------------------------

## 🤝 Open Source and Community

The project source is provided under the MIT License.

Third-party packages, ML libraries, and optional model weights remain
subject to their respective licenses and terms.

### Built By

- **Siddharth** – AI & Quant Brain
- **Shreetham** – Full-Stack & Market Ops
- **Shreeyansh** – Data Pipeline & Public Web

### Repository

[⭐ Explore TradePoint on
GitHub](https://github.com/Siddharth-Goutham/TradePoint)

------------------------------------------------------------------------

**TradePoint – Bridging Financial News, AI Intelligence, Quantitative
Analysis, and Automated Trading.**
