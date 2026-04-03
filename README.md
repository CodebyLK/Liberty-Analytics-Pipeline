# 🏀 NY Liberty: AI-Powered Performance & Sentiment Warehouse

### **Project Overview**
This project builds an end-to-end Data Engineering pipeline that correlates WNBA professional athlete performance with unstructured fan sentiment. By integrating official game statistics with AI-processed social media commentary, the warehouse identifies how on-court milestones (e.g., a 30-point game) impact brand perception across different operational categories like Coaching, Merchandise, and Ticketing.

---

### **The Technical Challenge: "The Laney Paradox"**
A primary engineering hurdle was **Entity Resolution**. Social media data is "noisy" and lacks relational keys (Player IDs). Early iterations suffered from **Attribution Error**, where general team complaints (e.g., ticket prices) were incorrectly linked to individual player performance. 

**The Solution:** I implemented a strict attribution logic using Python and SQL subqueries to ensure that only entity-specific commentary was joined to player stats, while general operational "noise" remained unattributed at the team level.

---

### **The Architecture**
1.  **Phase 1: ETL & Star Schema**
    * Automated ingestion of 5 years of WNBA data via REST API (`nba_api`).
    * Designed a normalized Star Schema in SQL Server consisting of `Fact_Games`, `Fact_Player_Stats`, `Dim_Players`, and `Dim_Dates`.
2.  **Phase 2: AI Enrichment Pipeline**
    * Developed an NLP pipeline to categorize unstructured text into Sentiment (Positive/Negative/Neutral) and Topic (Performance/Operations/Merchandise).
    * Mapped informal aliases (e.g., "Stewie", "Sabrina") to official `PLAYER_ID` records using custom mapping logic.
3.  **Phase 3: Business Intelligence & Analytics**
    * Executed "Mixed-Grain" joins to correlate 20+ point games with specific fan sentiment shares.
    * Built custom visualizations in Jupyter using Matplotlib and Seaborn to communicate brand health to stakeholders.

---

### **Key Insights**
| Player | Milestone | AI Sentiment | Business Context |
| :--- | :--- | :--- | :--- |
| **Breanna Stewart** | 3+ Defensive "Stocks" | **Positive** | High correlation with "Elite Defense" topics. |
| **Sabrina Ionescu** | 4+ Three-Pointers | **Positive** | Drives "MVP" narrative and shooting efficiency discussions. |
| **B. Laney-Hamilton**| 30-Point Game | **Negative/Neutral** | Performance was offset by fan frustration regarding coaching and ticket prices. |

---

### **Tech Stack**
* **Database:** SQL Server (T-SQL)
* **Languages:** Python (Pandas, SQLAlchemy)
* **API:** WNBA Stats API
* **AI/NLP:** Custom Sentiment Logic (GPT-Instruction ready)
* **Visualization:** Matplotlib, Seaborn

---

### **How to Run**
1.  Execute `NY Liberty Analytics.ipynb` to build the Star Schema.
2.  Run `Reddit_Sentiment_Engine.ipynb` to process the NLP pipeline and map entities.
3.  Open `NY_Liberty_Insights.ipynb` to view the final performance-sentiment correlation and visualizations.

---