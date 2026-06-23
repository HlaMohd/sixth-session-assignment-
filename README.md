# Chess Data Analysis Project

## Overview
This project analyzes chess game data and answers different analytical questions using SQL queries.

---

## Database Structure

The database consists of 3 main tables:

- **players** (players)
- **games** (games)
- **openings** (chess openings)

---

## Foreign Keys (FKs)

- `games.white_id → players.username`  
  Ensures that the white player exists in the system.

- `games.black_id → players.username`  
  Ensures that the black player exists in the system.

- `games.opening_code → openings.opening_code`  
  Ensures that the opening exists in the openings reference table.

### Why these FKs exist:
- `white_id` and `black_id`: Prevent inserting games with unregistered players.
- `opening_code`: Prevent using undefined or invalid openings.

---

## CHECK Constraints

These constraints ensure data validity:

- `registered_year BETWEEN 1900 AND 2025`  
  Prevents unrealistic registration years.

- `rating_registry BETWEEN 0 AND 3000`  
  Ensures valid chess rating values.

- `account_status IN ('active','closed','banned')`  
  Restricts account status to valid categories.

- `join_platform IN ('web','mobile','api')`  
  Restricts allowed platforms.

- `turns > 0`  
  Prevents games with zero or negative moves.

- `victory_status IN (...)`  
  Ensures valid game outcome values.

- `winner IN ('white','black','draw')`  
  Restricts winner values to valid options.

- `rated IN (0,1)`  
  Ensures binary values only.

---

## Performance Optimization (Indexes)

Indexes were added to improve query performance:

- `games(white_id)` → speeds up queries filtering by white player
- `games(black_id)` → speeds up queries filtering by black player
- `games(opening_code)` → speeds up joins with openings table

---

## Query Execution Plan (EXPLAIN QUERY PLAN)

Before indexing:
- Queries used full table scan (SCAN)
- Complexity: **O(n)**

After indexing:
- Queries use indexed search (SEARCH INDEX)
- Complexity improves to approximately **O(log n)**

---

## Solved Questions

### 1. Which opening has the highest draw rate?
- Calculate draw percentage for each opening.

### 2. Players who win more as black than white
- Compare number of wins as black vs white for each player.

### 3. Most common opening per win type
- Use `ROW_NUMBER()` to rank openings per victory status.

### 4. Top 3 opening families by average number of turns
- Group by `opening_fullname` and compute average turns.

### 5. Ranking game lengths per player
- Use `RANK()` with `PARTITION BY player_id ORDER BY turns DESC`.

---

## Feature Table

A file `features.csv` was created containing 7 columns:

- `game_id`: Game identifier
- `rating_diff`: Difference in ratings between players
- `turns`: Number of moves in the game
- `rated`: Whether the game is rated (0/1)
- `opening_shortname`: Short name of the opening
- `white_experience`: Number of previous games played by white player
- `winner`: Game result (white / black / draw)

---

## How to Run

```bash
python src/db.py        # Create database
python src/queries.py   # Run SQL queries and save results
python src/features.py  # Build feature dataset
