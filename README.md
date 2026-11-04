# TradePoint
**Context-Aware Trading Assistant**  
*A unified, agent-driven financial platform that automates a retail trader's daily research, sentiment analysis, and live market execution.*

## Why TradePoint
Retail traders face immense friction when managing isolated tools for market news, quantitative analysis, and trade execution. Quant-Sent bridges this gap by providing an end-to-end, automated trading intelligence platform. By integrating the PayPal REST API, this infrastructure is monetized as a seamless SaaS product, allowing traders to subscribe to premium multi-agent financial analytics and automated execution without leaving the ecosystem.

## Features and status
*   **Market Data Ingestion:** Decoupled headless Selenium script bypassing JS blocks to scrape unstructured daily financial news.
*   **Database Integration:** Scraped headlines, links, and metadata are inserted directly into a shared SQLite/PostgreSQL database via SQLAlchemy.
*   **AI Orchestration:** LangGraph stateful multi-agent workflow orchestrating Scraper, Quant, Risk Management, and Execution agents.
*   **Quantitative ML Pipeline:** Scikit-Learn and XGBoost models predict price movement probabilities by merging historical NSE telemetry with LLM sentiment scores.
*   **Automated Execution:** Daily OAuth login flow captures the Zerodha Kite API access token to automatically place Good Till Triggered (GTT) orders.
*   **SaaS Monetization:** PayPal REST API integration captures subscriptions, while a Gmail SMTP script delivers daily HTML email digests.

## Architecture
The pipeline relies on a clean data handoff:
1.  **Data Extraction:** The Selenium scraper runs independently of the Flask app, dumping daily news into the `News` database table.
2.  **AI & Quant Processing:** LangGraph agents pull the news, generate LLM sentiment scores, and feed the data alongside NSE telemetry into the XGBoost pipeline.
3.  **Market Ops:** The Flask backend manages the Zerodha Kite OAuth flow. The Execution agent uses the quantitative output to place GTT orders via the Kite API.
4.  **UI & Delivery:** The frontend Jinja templates query the database to render the "News Column" and user dashboard, while the SMTP engine sends daily digests.

## Technology and design
*   **Backend:** Python 3.11+, Flask, SQLAlchemy, SQLite/PostgreSQL.
*   **Market Operations:** Zerodha Kite Connect API.
*   **Data Pipeline:** Selenium (headless).
*   **Machine Learning:** Scikit-Learn, XGBoost, LangGraph, OpenAI-compatible LLMs.
*   **Frontend:** HTML5, CSS3, Jinja2, Bootstrap.
*   **Payments:** PayPal REST API.

## Install and configure
**1. Clone the repository**
git clone https://github.com/your-username/tradepoint.git
cd tradepoint

**2. Set up a virtual environment**
python3 -m venv .venv
source .venv/bin/activate  # On Windows use: .\.venv\Scripts\activate

**3. Install dependencies**
python -m pip install -r requirements.txt

**4. Configure Environment Variables**
Create a `.env` file in the root directory:
KITE_API_KEY=your_kite_api_key
KITE_API_SECRET=your_kite_api_secret
PAYPAL_CLIENT_ID=your_paypal_client_id
PAYPAL_CLIENT_SECRET=your_paypal_secret
FLASK_APP=app.py
FLASK_ENV=development
SECRET_KEY=your_flask_secret_key

## Quickstart
**Initialize the Database:**
flask db upgrade

**Run the Application (Terminal 1):**
flask run

**Run the decoupled Selenium news scraper (Terminal 2):**
python scraper/daily_news_scraper.py

*The dashboard will be available at `http://localhost:5000`.*

## Team Members
*   **Siddharth Goutham** – The AI & Quant Brain (LangGraph Orchestration, XGBoost, Prompt Engineering)
*   **Shreetham Anand** – Full-Stack & Market Ops (Flask, Zerodha Kite API, Dashboard UI)
*   **Shreeyansh Vats** – Data Pipeline & Public Web (Selenium Scraper, PayPal API Integration)

---

# GitHub Issues Roadmap

## Phase 1: Foundation & Data Plumbing
**Issue 1: [Backend] Initialize Flask Boilerplate & Database Schema**
*Assignee: Shreetham Anand*
*   [ ] Set up the Flask application factory and configure SQLite/PostgreSQL.
*   [ ] Create SQLAlchemy models: `User`, `News` (title, link, sentiment_score), and `Trade` (status, symbol, GTT parameters).
*   [ ] Create the base `layout.html` and `dashboard.html` Jinja templates.

**Issue 2: [Data] Build Headless Selenium News Scraper**
*Assignee: Shreeyansh Vats*
*   [ ] Write a decoupled Python script using Selenium (headless) to bypass JS on financial news sites.
*   [ ] Extract headlines, summaries, and URLs.
*   [ ] Wire the script to insert scraped data directly into the `News` table in the shared database.

**Issue 3: [AI] Setup ML Environment & Initial LLM Prompts**
*Assignee: Siddharth Goutham*
*   [ ] Set up the Python environment for Scikit-Learn, XGBoost, and LangGraph.
*   [ ] Draft system prompts for LLM Sentiment Analysis (extracting bullish/bearish signals).
*   [ ] Write a script to fetch raw news from the `News` database table and append dummy sentiment scores for UI testing.

## Phase 2: Core Engine & User Interface
**Issue 4: [Backend] Implement Zerodha Kite Daily OAuth Flow**
*Assignee: Shreetham Anand*
*   [ ] Build the login route that redirects users to the Zerodha Kite login page.
*   [ ] Handle the callback route to capture the `request_token`.
*   [ ] Exchange the `request_token` for the daily `access_token` and store it securely.
*   [ ] Build the "News Column" UI on the dashboard to fetch and render articles from the database.

**Issue 5: [Web] Build Static Landing Page & Email Digest Template**
*Assignee: Shreeyansh Vats*
*   [ ] Build the public `index.html` landing page explaining "Quant-Sent".
*   [ ] Design the pricing tier section (UI only, for PayPal integration later).
*   [ ] Design the HTML/CSS template for the daily SMTP email digest (using Jinja formatting).

**Issue 6: [AI] Build LangGraph State Workflow & XGBoost Pipeline**
*Assignee: Siddharth Goutham*
*   [ ] Define the LangGraph `State` dictionary to hold market data, sentiment, and risk parameters.
*   [ ] Create the nodes: `Quant Agent`, `Risk Agent`, and `Execution Agent`.
*   [ ] Build the preliminary XGBoost pipeline to output a probability score.

## Phase 3: Execution & Monetization
**Issue 7: [Backend] Implement GTT Order Execution via Kite API**
*Assignee: Shreetham Anand*
*   [ ] Build the settings UI for users to define risk parameters (max loss, target profit).
*   [ ] Write Python execution logic to take JSON output from AI Agents and place GTT orders via Kite Connect API.
*   [ ] Update the `Trade` database table with order confirmation status.

**Issue 8: [Web/Data] PayPal Integration & SMTP Engine**
*Assignee: Shreeyansh Vats*
*   [ ] Integrate the PayPal REST API Smart Buttons onto the landing page.
*   [ ] Create a Flask webhook route to capture successful subscription payments and update `User` table status.
*   [ ] Write the Python SMTP script that queries the `News` table and emails the HTML digest to users.

**Issue 9: [AI] End-to-End Orchestration & Handoff Testing**
*Assignee: Siddharth Goutham*

<FollowUp label="Write the Flask Application Factory" query="Let's start with Issue 1 and write the Flask application factory and SQLAlchemy models."/>

<ElicitationsGroup message="Select a component to build first:">
  <Elicitation label="Initialize Flask Boilerplate" query="Write the Flask application factory and SQLAlchemy database schema."/>
  <Elicitation label="Build Selenium Scraper" query="Write the headless Selenium script for scraping financial news."/>
  <Elicitation label="Set up ML Prompts" query="Draft the initial LangGraph nodes and sentiment analysis prompts."/>
</ElicitationsGroup>
*   [ ] Connect LLM sentiment output to update the `sentiment_score` column in the database.
*   [ ] Wire the final output of the LangGraph `Execution Agent` directly into the GTT execution function.
*   [ ] Run a full simulated loop: News Scraped -> Sentiment Scored -> Probability Calculated -> GTT Order Generated.
