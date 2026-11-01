## Initial Thoughts

Q: If you had to guess the winner of tonight's Lakers at Jazz game, what five things would you look at to come up with a guess?
A: I would look at 
1. Each team's current win-loss record.
2. The matchup win-loss last season.
3. The starting five roster.
4. Injuries.
5. Schedule.

Q: If you always picked the home team, what percentage of games would you get right? 
A: I'd say 60%.

## Get Game Data

I used `nba_api` to get 2024-25 regular season game data, stored it in `data/games.csv`.

The csv file has 2,460 rows. There are 30 teams in the NBA, and each team plays 82 games. 30*82 = 2,460. Each row is a game from one team's perspective. For example, if Lakers played at Jazz, and Lakers won, there will be one row where it says Lakers won, and another row where it says Jazz lost.

There are 1,230 games: this is confirmed with `nunique()` on `GAME_ID` column.

 The column `WL` tells us who won. The column `MATCHUP` tells us whether it's a home or away game: `vs.` is home and `@` is away.

 ### Not every row has a "home team"

There are five games that are included in the regular season, but not played in the home stadiums:
- 2025-01-25, IND against SAS, in Paris
- 2025-01-23, IND against SAS, in Paris
- 2024-12-14, HOU against OKC, in Las Vegas
- 2024-12-14, ATL against MIL, in Las Vegas
- 2024-11-02, MIA against WAS, in Mexico

So all five are neutral-site games: neither team really had home court. If home-court advantage is a factor, I would keep these games but call them "neutral."

## Reshape Game Data

Right now, each game appears twice, once from each team’s point of view. A model needs one row per game with one answer to predict. 

In this one-row-per-game table, I need to keep:
- Game ID and Game Date
- Which team is home, which team is away
- What is home team points, what is away team points
- If a game is treated "neutral"

We find the following:
- Home team won 54% of games - lower than my initial guess of 60%.
- On average, the home team scores 1.7 points more than the away team.
- The top three teams with the best home record are OKC, CLE, and LAL with win percentages of 85%, 83%, and 76%
- The bottom three teams with the worst home record are WAS, UTA, and CHA with win percentages of 20%, 24%, and 29%
- If we predicted "home team wins" for every game, we would only be 54% accurate: just slightly better than flipping a coin. This is our baseline.

Interesting thought: OKC ended up winning the Chip, is it because of their home court performance, or just because they're a good team? To tell what is what, we would need to compare home-win % and away-win%. I would expect a good team to have similar home-win and away-win percentages.

## Other Methods of Prediction - Win-Loss Record

| Method | Accuracy |
|---|---|
| Always pick home team | 54% |
| Better record (full season) | 70% |
| Better record (games before only) | 64% |

Note: the issue with the full-season record method is that it's data leakage: we're making a prediction using data we wouldn't have access to.

If we were making real-time predictions, I would trust the rolling win-loss record more because it's based data we have.

Interesting result: predicting with the rolling win-loss resulted in accuracy of 60.2% in the first-half of the regular season and 67.8% in the second-half of the regular season. This is from sample size: early in the season, we're averaging over a small sample size. As more games are played, win % gets closer to a team’s real strength, so the predictions improve.


