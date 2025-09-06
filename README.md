# 🚀 ccda-a1: Multi-Container Docker App

## 📌 What the stack does
This project demonstrates a simple **multi-container application** using Docker Compose. It consists of:
- **Postgres database** (seeded with trip data from [`db/init.sql`](db/init.sql))
- **Python app** ([`app/main.py`](app/main.py)) that connects to the database, runs summary queries, and writes results to a JSON file.

The app prints results to the terminal and saves them into [`out/summary.json`](out/summary.json).

---

## ▶️ How to build and run
From the root of the project (`ccda-a1`):

```bash
# Build and start services
docker compose up --build

# Stop and remove containers, networks, and volumes
docker compose down -v
📂 Output location
Results are written to:

pgsql
Copy code
out/summary.json
This file is created each time the app runs and is also printed to stdout.

📝 Example output
Example output from the Python app:

json
Copy code
=== Summary ===
{
  "total_trips": 6,
  "avg_fare_by_city": [
    { "city": "Charlotte", "avg_fare": 16.25 },
    { "city": "New York", "avg_fare": 19.0 },
    { "city": "San Francisco", "avg_fare": 20.25 }
  ],
  "top_by_minutes": [
    { "id": 6, "city": "San Francisco", "minutes": 29, "fare": 29.3 },
    { "id": 4, "city": "New York", "minutes": 26, "fare": 27.1 },
    { "id": 2, "city": "Charlotte", "minutes": 21, "fare": 20.0 },
    { "id": 5, "city": "San Francisco", "minutes": 11, "fare": 11.2 },
    { "id": 1, "city": "Charlotte", "minutes": 12, "fare": 12.5 }
  ]
}
⚠️ Troubleshooting
Database not ready / connection errors
If you see "Waiting for database..." repeatedly, check DB logs:

bash
Copy code
docker compose logs db
Port conflict on 5432
If another Postgres is running locally, remove or comment out the ports: section in compose.yml. The app will still work using the internal Docker network.

Permission issues writing out/summary.json
Ensure out/ exists and is writable:

bash
Copy code
rm -rf out && mkdir out
Old containers/volumes interfering
Clean up with:

bash
Copy code
docker compose down -v
📖 Notes
Database: Postgres 16

App runtime: Python 3.11-slim

Schema & seed data: db/init.sql

Queries & summary logic: app/main.py
