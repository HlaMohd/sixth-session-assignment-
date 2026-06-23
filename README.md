Chess Data Analysis Project
Overview

This project analyzes chess game data and answers various analytical questions using SQL queries.

Database Structure

The database contains 3 main tables:

players (players)
games (games)
openings (chess openings)
Foreign Keys
games.white_id → players.username
Ensures that the white player exists in the system.
games.black_id → players.username
Ensures that the black player exists in the system.
games.opening_code → openings.opening_code
Ensures that the opening exists in the openings reference table.
Why these foreign keys exist:
white_id and black_id: Prevent adding games with unregistered players.
opening_code: Prevent using undefined or unrecognized openings.
CHECK Constraints and What They Prevent
registered_year BETWEEN 1900 AND 2025
Prevents unrealistic registration years.
rating_registry BETWEEN 0 AND 3000
Prevents invalid chess rating values.
account_status IN ('active','closed','banned')
Ensures only valid account statuses.
join_platform IN ('web','mobile','api')
Restricts allowed registration platforms.
turns > 0
Prevents games with zero or negative moves.
victory_status IN (...)
Ensures only valid game outcome values.
winner IN ('white','black','draw')
Prevents invalid winner labels.
rated IN (0,1)
Ensures binary values only for rated games.
Performance Optimization (Indexes)

The following indexes were added to improve query performance:

INDEX ON games(white_id) → speeds up queries filtering by white player
INDEX ON games(black_id) → speeds up queries filtering by black player
INDEX ON games(opening_code) → speeds up joins with openings table
Query Execution Plan (EXPLAIN QUERY PLAN)

Before indexing, queries used a full table scan (SCAN).
After adding indexes, queries use indexed search (SEARCH INDEX), reducing time complexity from O(n) to approximately O(log n).

Solved Questions
1. Which opening has the highest draw rate?
Compute draw percentage for each opening.
2. Players who win more as black than white
Compare wins as black vs wins as white per player.
3. Most common opening per win type
Use ROW_NUMBER() to rank openings per victory status.
4. Top 3 opening families by average number of moves
Group by opening_fullname and compute average turns.
5. Game length ranking per player
Use RANK() with PARTITION BY player_id ORDER BY turns DESC.
Feature Table

A features.csv file was created containing 7 columns:

game_id: Game identifier
rating_diff: Rating difference between players
turns: Number of moves
rated: Whether the game is rated
opening_shortname: Short opening name
white_experience: Number of previous games of the white player
winner: Game result (white / black / draw)
How to Run
python src/db.py           # Create database  
python src/queries.py     # Run SQL queries and save outputs  
python src/features.py    # Build feature dataset  
