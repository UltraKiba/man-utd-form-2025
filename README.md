
# 📊 Manchester United Current Form Analysis (Jan–May 2025)

A data analytics project examining Manchester United's win/loss trends and player performance from January 2025 to May 2025.

## 📈 Project Overview

- **Competitions**: Premier League, FA Cup, Europa League
- **Time Period**: January 2025 to May 2025
- **Metrics**: Match results, cumulative league points, player goals and assists

## 📊 Visualizations

📥 Download the [visualizations Excel file](./man_utd_visualizations.xlsx) containing:
- Cumulative Premier League points over time
- Goals scored over time by Bruno Fernandes, Amad Diallo, and Alejandro Garnacho

## 🗄️ Database Schema (SQL)

```sql
CREATE TABLE competitions (
  competition_id INTEGER PRIMARY KEY,
  name TEXT
);

CREATE TABLE matches (
  match_id INTEGER PRIMARY KEY,
  date DATE,
  competition_id INTEGER,
  opponent TEXT,
  venue TEXT,
  goals_for INTEGER,
  goals_against INTEGER,
  result CHAR(1),
  FOREIGN KEY (competition_id) REFERENCES competitions(competition_id)
);

CREATE TABLE players (
  player_id INTEGER PRIMARY KEY,
  name TEXT,
  position TEXT
);

CREATE TABLE appearances (
  match_id INTEGER,
  player_id INTEGER,
  minutes_played INTEGER,
  goals INTEGER,
  assists INTEGER,
  xg REAL,
  key_passes INTEGER,
  PRIMARY KEY (match_id, player_id),
  FOREIGN KEY (match_id) REFERENCES matches(match_id),
  FOREIGN KEY (player_id) REFERENCES players(player_id)
);
```

## 📊 Example SQL Queries

**Cumulative Points Over Time**
```sql
SELECT date,
       SUM(CASE 
             WHEN result='W' THEN 3 
             WHEN result='D' THEN 1 
             ELSE 0 
           END)
         OVER (ORDER BY date) AS cumulative_points
FROM matches
JOIN competitions USING (competition_id)
WHERE name = 'Premier League'
ORDER BY date;
```

**Top Performing Players**
```sql
SELECT p.name,
       SUM(a.goals)   AS total_goals,
       SUM(a.assists) AS total_assists,
       SUM(a.minutes_played) AS minutes_played
FROM players p
JOIN appearances a ON p.player_id = a.player_id
GROUP BY p.name
ORDER BY total_goals DESC, total_assists DESC;
```

## 📌 How to Use

1. 📥 Clone this repo or download the files.
2. 📊 Open the `man_utd_visualizations.xlsx` to explore the graphs.
3. 🛠️ Use the SQL schema and queries to analyze your own data.

## 📖 Sources

- Premier League Official Stats
- ESPN Football Player Stats
- FBref.com
- Understat.com
