# Flight Search System — SQLite B-Tree Indexing
### DSCI-551: Foundations of Data Management | Spring 2025-26
**Author:** Abhey Sabesan Mageswaran Aryaan | USC ID: 7162-7286-71
**Professor:** Wensheng Wu

## Project Overview

This project investigates how B-Tree indexing in SQLite impacts 
query performance at scale. A Flight Search System was built with 
1 million synthetic flight records to prove that the right B-Tree 
index makes queries up to 100 times faster, while also 
demonstrating the read-write tradeoff where indexes make 
writes 2.7x slower.

## Key Results

| Experiment | Without Index | With Index | Speedup |
|------------|--------------|------------|---------|
| Route search (LAX→JFK) | 0.5394s | 0.0058s | ~92x faster |
| Route search (ORD→MIA) | 0.5456s | 0.0052s | ~104x faster |
| Sort by price | ~1.07s | ~0.002s | ~630x faster |
| INSERT 10,000 rows | 0.0140s | 0.0372s | 2.7x slower |

---

## Environment Setup

### Requirements
- Python 3.8 or higher
- Jupyter Notebook

### Install Dependencies

```bash
pip install jupyter matplotlib numpy ipywidgets
```

### Enable ipywidgets (for the interactive UI)

```bash
jupyter nbextension enable --py widgetsnbextension
```

---

## How to Run

### Step 1 — Clone the repository

```bash
git clone https://github.com/abhey-10/DSCI551-Flight-Search-SQLite.git
cd DSCI551-Flight-Search-SQLite
```

### Step 2 — Open Jupyter Notebook

```bash
jupyter notebook
```

### Step 3 — Open the notebook

Open `Flight_Enginer.ipynb` in your browser.

### Step 4 — Run all cells

Go to **Kernel → Restart & Run All**

This will automatically:
1. Connect to (or create) the SQLite database `flights.db`
2. Create the flights table
3. Generate 1 million synthetic flight records
4. Run all performance experiments
5. Generate the charts
6. Launch the interactive UI

---

## Dataset

This project uses fully synthetic data generated automatically 
by the notebook. No external dataset download is required.

Data generation pipeline (runs automatically in the notebook):
- 5 airlines: Delta, United, American, Southwest, JetBlue
- 8 airports: LAX, JFK, ORD, ATL, SFO, DFW, MIA, SEA
- 1 million flights generated using Python random module
- Random prices between $80 and $800
- Random departure times across June 2025
- All data inserted automatically using cursor.executemany()

Simply run **Kernel → Restart & Run All** and the entire 
database is created automatically! No manual setup needed.

---

## Database

- **System:** SQLite (embedded, no server needed)
- **File:** `flights.db` (auto-created by notebook)
- **Records:** 1,000,000 synthetic flight records
- **Schema:**

```sql
CREATE TABLE flights (
    id          INTEGER PRIMARY KEY,
    airline     TEXT,
    origin      TEXT,
    destination TEXT,
    departure   TEXT,
    price       REAL
)
```

---

## No Secret Keys Required

This project uses only:
- SQLite (built into Python — no installation needed)
- Standard Python libraries (random, time, matplotlib)

No API keys, credentials, or environment variables needed.

---

## Project Structure

The notebook is organized into these sections:

1. Database Setup — connect, create table, generate 1M records
2. Search WITHOUT index — measure baseline speed
3. EXPLAIN QUERY PLAN — shows SCAN (full table scan)
4. Create B-Tree Index — composite index on (origin, destination)
5. Search WITH index — same query, dramatically faster
6. Scale to 1 Million rows
7. Multi-Route Testing — consistent 90-100x speedup
8. Performance Charts — visual comparison
9. VDBE Bytecode — machine level proof
10. Additional Experiments — airline filter, price sort, wrong index
11. INSERT Tradeoff — indexes slow writes 2.7x
12. Interactive UI — live flight search application
