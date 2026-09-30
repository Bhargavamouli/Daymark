 Daymark

A daily routine tracker with meal check-ins, hour trends, and an optional public GitHub activity import.**

[Try the live app](https://daymark-bhargava.bhargavad161.chatgpt.site/) · [Online source](online/) · [Local Windows source](local/)

Daymark helps people see how they spend their days. Users record study, exercise, and sleep time during morning, afternoon, and night check-ins. Breakfast, lunch, and dinner are tracked with simple ticks, with no meal-hour entry. The dashboard calculates daily totals, displays charts and history, and marks whether sleep and exercise goals were met.

## Features

- Three daily check-ins for study, exercise, and sleep hours.
- One-tap breakfast, lunch, and dinner ticks.
- Daily totals, trend charts, meal streaks, and goal indicators.
- Day navigation to review and correct earlier entries.
- Optional import of recent **public** GitHub events through a connector and data pipeline.
- Separate online and local editions, each with its own storage.

## Live demo

Open the [online Daymark app](https://daymark-bhargava.bhargavad161.chatgpt.site/). You can try the daily check-ins and meal ticks, then review the totals and charts. The live app's saved data belongs to the signed-in account; this source repository contains no user records.

## How it works

| Part | Online edition (`online/`) | Local Windows edition (`local/`) |
| --- | --- | --- |
| Interface | React, TypeScript, Recharts | React, TypeScript, Recharts; built with Vite |
| API | TypeScript routes on a Cloudflare Worker | Python, FastAPI, Uvicorn |
| Habit data | Cloudflare D1, scoped to the signed-in user | Browser IndexedDB, with a localStorage fallback |
| Imported GitHub data | Cloudflare D1 | SQLite in a local `data/` folder |

When a user saves a check-in, Daymark stores the entered time for that date. Meal ticks are saved as true or false. The interface reads those records to calculate totals, goals, streaks, and chart points.

The optional GitHub pipeline requests recent public events from GitHub's REST API, checks and normalizes the records, then saves them using each event's external ID to avoid duplicates. It can resume an interrupted import. GitHub events are an activity feed, **not a measure of coding hours**.

The online and local editions do **not** synchronize with each other. The local edition stores habit data in the browser and imported events in SQLite on that computer. Its standalone HTML option works for habit tracking without the Python API, but GitHub import requires the local API.

## Run the local edition on Windows

Install **Node.js 22.12+** and **Python 3.11+**, then open a terminal inside `local/` and run:

```powershell
npm install
npm run build
python -m pip install -r requirements.txt
python run.py
```

Open `http://127.0.0.1:8080/` in your browser. After building, you can also run `Start-Daymark.bat`; it prepares a Python virtual environment and opens the local app. Its first launch needs an internet connection to install dependencies. See [local/README.md](local/README.md) for the standalone HTML option and backup details.

The hosted edition needs its own Worker deployment, D1 database binding, migrations, and authenticated identity configuration. See [online/README.md](online/README.md); the live demo is the easiest way to explore it.

## Repository guide

| Path | What to look for |
| --- | --- |
| [`online/app`](online/app) and [`online/lib`](online/lib) | Hosted UI, API routes, GitHub connector, and import logic |
| [`online/drizzle`](online/drizzle) | Database schema migrations |
| [`local/source`](local/source) | Local UI, browser storage, and backup controls |
| [`local/backend`](local/backend) | FastAPI endpoints, validation, pipeline, and SQLite storage |
| [`online/tests`](online/tests) and [`local/tests`](local/tests) | Connector and pipeline tests |

## Data and development notes

This repository contains source code and static assets. It excludes personal habit history, personalized generated HTML, SQLite databases, tokens, and virtual environments. A local backup is optional and stays on the user's computer unless they choose to move it.

This project was developed with AI assistance. The code and README are provided so the behavior and implementation can be reviewed directly.
