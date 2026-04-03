# 🗽 NY Liberty Analytics: Enterprise Data Warehouse

## 📌 Project Overview
This project is an end-to-end, local Data Engineering pipeline built to analyze the performance of the WNBA's New York Liberty. It extracts 5 years of live team and player data, loads it into a local SQL Server, normalizes the raw data into a Star Schema, and executes advanced SQL queries to generate actionable sports intelligence.

## 🛠️ Tech Stack & Architecture
* **Language:** Python (Pandas)
* **Database:** Microsoft SQL Server (SSMS)
* **Pipeline:** ELT (Extract, Load, Transform) via `nba_api` & `SQLAlchemy`
* **Architecture:** Star Schema (Fact & Dimension Tables)

## 🗄️ Database Schema
The raw API data was transformed into the following normalized schema:
* **Dimension Tables:** `Dim_Teams`, `Dim_Dates`, `Dim_Players`
* **Fact Tables:** `Fact_Games` (Team Box Scores), `Fact_Player_Stats` (Individual Box Scores)

## 📊 Key Insight: The "Franchise Carry"
To demonstrate advanced SQL capabilities (CTEs and Window Functions), I queried the database to isolate the #1 top-scoring NY Liberty player for each of the last 5 seasons. 

**The SQL Query:**
```sql
WITH YearlyPlayerStats AS (
    SELECT 
        p.PLAYER_NAME,
        d.Game_Year,
        SUM(CAST(fps.Points AS FLOAT)) AS Total_Season_Points,
        ROW_NUMBER() OVER(PARTITION BY d.Game_Year ORDER BY SUM(CAST(fps.Points AS FLOAT)) DESC) as Team_Rank
    FROM Fact_Player_Stats fps
    JOIN Dim_Players p ON fps.PLAYER_ID = p.PLAYER_ID
    JOIN Dim_Teams t ON p.TEAM_ID = t.TEAM_ID
    JOIN Fact_Games fg ON fps.GAME_ID = fg.GAME_ID
    JOIN Dim_Dates d ON fg.GAME_DATE = d.GAME_DATE
    WHERE t.TEAM_ABBREVIATION = 'NYL'
    GROUP BY p.PLAYER_NAME, d.Game_Year
)
SELECT Game_Year, PLAYER_NAME AS MVP_Scorer, Total_Season_Points
FROM YearlyPlayerStats
WHERE Team_Rank = 1
ORDER BY Game_Year DESC;
