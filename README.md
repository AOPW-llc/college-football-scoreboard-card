# College Football Scoreboard Card

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)](https://github.com/hacs/integration)
[![Validate](https://github.com/AOPW-llc/college-football-scoreboard-card/actions/workflows/validate.yml/badge.svg)](https://github.com/AOPW-llc/college-football-scoreboard-card/actions/workflows/validate.yml)

A Home Assistant dashboard card for NCAA FBS college football scores. It shows
one week of the college football calendar at a time, with one column per day.
Each column has a dropdown of that day's games and shows the selected game's
score, team stats, leaders and conference standings, with AP Top 25 rankings
throughout. Tap a team logo for that team's season stats against the FBS
leaders, its full conference standings and the AP poll.

No integration, sensor or API key is needed. The card reads ESPN's public
scoreboard, game summary, rankings, standings and team statistics feeds
straight from the browser.

## Features

- **College football weeks.** Each screen is one week of the season (Week 9,
  … Bowls & CFP), named in the header. Weeks run Monday to Sunday and come from
  ESPN's calendar. The long stretches are split so they stay readable: Week 1
  (from the late-August "Week 0" games through Labor Day) is two screens, and
  bowl season and the College Football Playoff are one screen per week.
- **Carousel.** The arrows move a week back or forward with a sliding
  carousel. Within the week, swipe or scroll through the days, or tap a day
  button (M T W T F S S) to jump to it.
- **Fits the screen.** By default the card shows as many whole days as fit
  across it, so no column is cut off. On a phone that's one day at a time. Or
  choose how many days to show (1–7).
- **Game dropdown per day,** with AP rankings:
  - Started or finished: `#3 Georgia (31) - #17 Florida (20) Final` or
    `#22 Arizona (14) - #13 Texas Tech (10) 7:42/3rd`
  - Not started: `#1 Miami v. North Carolina, 7:30 PM`
- **Choose which games to list:** all FBS games, only games with a ranked
  team, or one conference. Your favorite team's game is always listed.
- **Picks the best game.** Without a favorite team (or on its bye week), each
  day opens on the highest-ranked game that's on, then the highest-ranked game
  still to come.
- **Game view.**
  - Both teams with AP rank, logos, records and the score.
  - Bowl and playoff games are named above the matchup (e.g. `CFP
    Quarterfinal at the Rose Bowl`).
  - Team stats side by side once the game has started, with the better number
    highlighted.
  - Passing, rushing and receiving leaders.
  - Tap the **Team stats** or **Leaders** heading to hide that section in every
    day column, and tap it again to bring it back. The browser remembers your
    choice.
  - The top 3 in each team's conference, plus the team's own place if it's
    lower, with conference records.
  - Before kickoff: venue, TV and (optionally) the betting line.
- **Team view.** Tap a team logo to see:
  - The team's season stats against the FBS leader in each, with its FBS rank.
  - Its full conference standings (conference and overall records).
  - The AP Top 25 with each team's movement since last week.
  - Tap any team in either table to switch to it.
- **Picks up where you left off.** Leave the dashboard and come back, and the
  card returns to the week, day and team page you were on. It starts fresh at
  today in a new tab or app session, or once the date has changed.
- **Live updates.** Refreshes every 30 seconds while a game is on (or about to
  start), and every 5 minutes otherwise. Only days with a game on are
  re-downloaded on each live refresh, which keeps a full Saturday light.
- **Game counts.** Each day shows how many games are on it, in gray.
- **Favorite team.** Optionally picks your team's game automatically on the
  days they play.

## Installation

### HACS (recommended)

1. In HACS, open the menu (⋮) → **Custom repositories**.
2. Add `https://github.com/AOPW-llc/college-football-scoreboard-card` with
   category **Dashboard**.
3. Search for **College Football Scoreboard Card**, download it, and reload
   your browser.

### Manual

1. Download `college-football-scoreboard-card.js` from the
   [latest release](https://github.com/AOPW-llc/college-football-scoreboard-card/releases/latest).
2. Copy it to `/config/www/college-football-scoreboard-card.js`.
3. Go to **Settings → Dashboards → ⋮ → Resources** and add
   `/local/college-football-scoreboard-card.js` as a **JavaScript module**.

## Configuration

Add the card from the dashboard card picker (search "College"). Everything can
be set in the visual editor. The card works best full width, for example in a
**Panel** view.

```yaml
type: custom:college-football-scoreboard-card
team: UGA
games: all
days: auto
refresh_interval: 30
show_odds: true
```

| Option | Default | Description |
|---|---|---|
| `team` | none | Favorite team's ESPN abbreviation (e.g. `UGA`, `OSU`, `ALA`, `TEX`). Its game is selected automatically on the days it plays. The visual editor lists every FBS team. |
| `games` | `all` | Which games each day's dropdown lists: `all` (every game involving an FBS team), `top25` (games with an AP-ranked team), or a conference's ESPN id (see below). |
| `days` | `auto` | How many days of the week show at once (`auto` or `1`–`7`). The rest of the week is a swipe or scroll away, and the arrows always move a whole week. See below. |
| `refresh_interval` | `30` | Seconds between refreshes while a game is live (10–300). |
| `show_odds` | `true` | Show the betting line before kickoff. |

Conference ids for `games`: ACC `1`, American `151`, Big 12 `4`, Big Ten `5`,
C-USA `12`, FBS Independents `18`, MAC `15`, Mountain West `17`, Pac-12 `9`,
SEC `8`, Sun Belt `37`. A conference lists every game one of its teams plays,
including non-conference games.

### Days on screen

The card always works in calendar weeks. `days` only sets how many of that
week's days are on screen together:

- **`auto`:** as many columns at least 300px wide as fit. One day below 600px
  wide (phones), and up to the whole week on a wide screen.
- **`1`–`7`:** that many days. `7` shows a normal week with nothing to swipe.
  Longer weeks (the first week of the season, the last week of bowl season)
  still scroll.
  If the card is too narrow for that many readable columns (about 220px each),
  it shows as many as fit and the rest scroll.

The card measures its own width, not the device's, so the result also depends
on the dashboard layout. A **Panel** view gives the card the full width.
Masonry columns are narrow and usually leave room for only one day.

## Stats shown

**Game stats:** total yards, passing, rushing, first downs, 3rd down, 4th down,
turnovers, penalties, possession (plus red zone and sacks when ESPN has them).

**Season stats in the team view:** points per game, points allowed, total yards,
passing yards, rushing yards, yards allowed, 3rd down %, turnover differential,
takeaways, sacks. Ranks are among all FBS teams.

## Notes

- Times are shown in your browser's time zone. A game belongs to the day it
  kicks off locally.
- Rankings are the AP Top 25. A game shows each team's ranking as of that game.
- Conference standings are in ESPN's order. Sun Belt standings are shown by
  division.
- Season stats, standings and the team picker cover FBS teams only. FCS
  opponents still show in games, without standings or season stats.
- ESPN's feeds are public but undocumented, and can change without notice.

## Disclaimer

This is an unofficial, fan-made card. It is not affiliated with, endorsed by or
sponsored by the NCAA, its conferences or member schools, the Associated Press
or ESPN. Team names and logos are trademarks of their respective owners. Logos
are loaded from ESPN at runtime and are not included in this repository.

## License

[MIT](LICENSE)
