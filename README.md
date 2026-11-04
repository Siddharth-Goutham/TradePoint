# 📈 TradePoint: Context-Aware Trading Assistant

> A unified, agent-driven financial platform that automates retail trading research and execution.

TradePoint is a state-of-the-art automated trading engine built for modern retail traders. It seamlessly combines unstructured market news scraping, LLM-powered sentiment analysis, and historical NSE price action via an ML pipeline to automatically execute intelligent Good Till Triggered (GTT) orders via the Zerodha Kite API.

---

## 🏗️ Architecture & Workflow

TradePoint is designed with a decoupled architecture to ensure that market intelligence runs uninterrupted, even in the face of broker API token expirations.

1. **Data Ingestion (Decoupled & Database-Backed):** 
   A headless Selenium scraper runs independently, bypassing JS blocks to extract daily financial news. It formats and saves this data directly into our shared database.

2. **AI Orchestration (LangGraph):**
   * **Scraper/Data Agent:** Pulls and structures the raw news from the database.
   * **Quant Agent:** Applies LLM sentiment analysis to the news, updating the database with a "Sentiment Score". It then fuses these scores with historical NSE telemetry using an XGBoost/Scikit-Learn ML pipeline to predict price movement probabilities.
   * **Risk Management Agent:** Validates trade signals against user-defined risk parameters and capital limits.
   * **Execution Agent:** Formats the final approved signals for broker execution.

3. **Market Operations & Dashboard (Flask):**
   * The backend manages the Zerodha Kite Connect daily OAuth lifecycle and executes GTT orders.
   * **In-App News Column:** The frontend dashboard queries the database to display a live feed of the scraped market news right alongside the AI-generated sentiment scores and active trades.

4. **Monetization & Delivery:** 
   Subscriptions are handled via the PayPal REST API. Users can also opt-in to receive daily HTML market digests via a background SMTP engine.

---

## 🛠️ Tech Stack

* **Backend & UI:** Python, Flask, SQLAlchemy, Jinja2 Templates, Bootstrap
* **Machine Learning & AI:** Scikit-Learn, XGBoost, LangGraph (Multi-Agent Workflow), LLMs
* **Data Pipeline:** Python, Selenium (Headless)
* **Broker Integration:** Zerodha Kite Connect API
* **Payments & Notifications:** PayPal REST API

---

## 🚀 Getting Started

### Prerequisites

* Python 3.9+
* Zerodha Kite Developer App (API Key & Secret)
* LLM API Key (e.g., OpenAI / Anthropic)
* Chrome WebDriver (for Selenium)

### Installation

1. **Clone the repository:**
    ```bash
    git clone [https://github.com/Siddharth-Goutham/TradePoint.git](https://github.com/Siddharth-Goutham/TradePoint.git)
    cd TradePoint
    ```

2. **Set up a virtual environment:**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows use: venv\Scripts\activate
    ```

3. **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4. **Environment Variables:**
    Create a `.env` file in the root directory based on `.env.example` and populate it with your credentials:
    ```env
    KITE_API_KEY=your_kite_api_key
    KITE_API_SECRET=your_kite_api_secret
    LLM_API_KEY=your_llm_api_key
    PAYPAL_CLIENT_ID=your_paypal_client_id
    ```

5. **Initialize the Database:**
    ```bash
    flask db upgrade
    ```

6. **Run the Application:**
    ```bash
    # Terminal 1: Run the main Flask App (UI, Dashboard News, Market Ops)
    flask run
    
    # Terminal 2: Run the Decoupled Scraper & AI Pipeline
    python scraper_pipeline.py
    ```

---

## 👥 The Team

Built during the PayPal Hackathon by:

* **Siddharth Goutham** (AI & Quant Brain)
* **Shreetham Anand** (Full-Stack & Market Ops)
* **Shreeyansh Vats** (Data Pipeline & Public Web)

*Prioritizing functional, robust MVPs over premature optimization.*
