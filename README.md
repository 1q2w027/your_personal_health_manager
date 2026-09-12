# YPH — Your Personal Health Manager

A lightweight, dependency-free health tracking dashboard. Log workouts, meals, sleep, and mood in the browser, and see 7-day trends update instantly — no account, no server, no data leaving your machine.

Built with vanilla HTML, CSS, and JavaScript. The only external dependency is Chart.js.


<img width="1426" height="732" alt="Screenshot 2026-09-11 at 10 19 50 PM" src="https://github.com/user-attachments/assets/8737a77f-a07a-437d-aaba-3d71a1d2bb29" />


---

## Features

### Fitness
Log workout duration and type per day. The dashboard renders a 7-day bar chart of minutes trained, a rolling weekly average in hours and minutes, an estimated calorie burn for the current day, and a goal indicator that flips once you pass 30 minutes.

### Nutrition
Record meals by name, portion, and calories. Shows a 7-day calorie chart, weekly average intake, running total for today, and progress against the daily target.

### Sleep
Track hours, minutes, and subjective sleep quality. Produces a 7-day duration chart, weekly average, and a goal status for last night's sleep.

### Mental Wellness
Rate your mood daily and keep a titled diary entry alongside it. Entries are stored by date and can be reopened or removed from the history list.

### Across all modules
- Full history view with per-entry delete
- Date picker defaults to today, so logging takes two clicks
- Responsive layout — the dashboard reflows for phone, tablet, and desktop

---

## Getting started

No build step, no install.

```bash
git clone https://github.com/1q2w027/your_personal_health_manager.git
cd "your_personal_health_manager/your personal health manager"
open before_login.html      # macOS
# start before_login.html   # Windows
```

Or serve it locally:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000/before_login.html
```

---

## How it works

Each module owns an isolated `localStorage` bucket, keyed by ISO date:

| Module | Storage key | Shape |
| --- | --- | --- |
| Fitness | `workoutData` | `{ "2025-06-02": [{ minutes, type }] }` |
| Nutrition | `foodData` | `{ "2025-06-02": [{ name, amount, calories }] }` |
| Sleep | `sleepData` | `{ "2025-06-02": [{ hours, minutes, quality }] }` |
| Mental | `mentalData` | `{ "2025-06-02": [{ rating, title, content }] }` |

On every write, the page walks back seven days from today, aggregates each day's entries, pushes the totals into the Chart.js dataset, and recomputes the summary stats before re-rendering the history list. Because each module reads and writes only its own key, the four trackers stay fully independent — a corrupted or cleared bucket never affects the others.

The aggregation path tolerates both array and single-object day values, so records written by earlier versions of the app still load correctly.

---

## Project structure

```
your personal health manager/
├── before_login.html    # Landing page
├── log_in.html          # Login screen
├── after_login.html     # Home dashboard
├── fitness.html         # Workout tracker
├── food.html            # Nutrition tracker
├── sleep.html           # Sleep tracker
├── mental.html          # Mood + diary
├── services.html        # Feature overview
├── about.html
├── contact.html
└── img/                 # Icons and assets
```

Styles and scripts are inlined per page. At this scale it keeps each tracker a single self-contained file you can open and read top to bottom.

---

## Limitations

- Data lives in `localStorage` only — clearing browser data wipes history, and nothing syncs across devices.
- Login is a front-end mockup; there is no authentication backend.
- Calorie burn in the fitness module uses a flat 5 kcal/minute estimate, not a per-activity or bodyweight-adjusted formula.

---

## Tech

Vanilla JavaScript · HTML5 · CSS3 · [Chart.js](https://www.chartjs.org/) · Web Storage API

## Author

Daniel Park (박찬일) — [park34ci@berkeley.edu](mailto:park34ci@berkeley.edu)
