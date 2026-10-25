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