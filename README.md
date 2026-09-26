# Smart Campus Delivery System

An agent-based simulation built for the **ConsultX case competition**, modeling delivery-fleet movement on a university campus to quantify the operational impact of switching gate entry from a **written log** to a **QR-code scan**. Rather than argue the case qualitatively, the simulation runs many delivery agents through a congestion-aware campus network and measures the actual time and throughput difference between the two entry systems.

## What it simulates

- A campus road network (`networkx` graph) connecting gates, delivery hubs, and hostels, with realistic edge distances
- A fleet of delivery agents across three vehicle types (petrol, e-bike, cycle) with different speed profiles, routed with congestion-aware shortest-path search that reacts to live traffic on each edge, not just static distance
- Two gate-entry systems modeled with distinct timing distributions:
  - **Written entry** — slow base time, high congestion multiplier and queue penalty
  - **QR-code entry** — a 4x faster base time with proportionally lower congestion sensitivity
- GPS-style position tracking per agent with smoothed path interpolation, so agent motion (and the resulting dashboard) reflects realistic movement rather than teleporting between nodes

## Dashboard

A multi-tab `Tkinter` + `matplotlib` GUI (`EnhancedCampusGUI`) renders the simulation live:
- **Simulation view** — the campus graph with agents moving in real time, adjustable agent count (1–1000+) and playback speed via sliders, with level-of-detail auto-enabled for large agent counts
- **GPS tracking view** — per-agent trajectory tracking
- **Analytics dashboard** — a 4-panel breakdown of simulation-wide metrics
- **Entry-system comparison dashboard** — the core deliverable: side-by-side entry time, congestion impact, time-efficiency, and system-usage panels comparing written vs. QR entry
- **Route/loop analysis** — aggregate view of delivery loop times across the run

A `Flask`/`Plotly`-based web dashboard (`templates/index_advanced.html`) is included as an alternative front end for the same live metrics.

There's also a standalone Colab-friendly script (`workinggooglecollabsim.py`) with the same core swarm simulation logic, packaged as a single notebook-runnable file for environments without a display server.

## Stack

`NetworkX` (routing) · `NumPy` / `SciPy` (congestion-aware path optimization) · `scikit-learn` (nearest-neighbor queries) · `Matplotlib` + `Tkinter` (live dashboard) · `pandas` (analytics)

## Running it

```bash
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
python main.py
```

Use the sliders in the Simulation tab to scale agent count and playback speed; click the Simulation tab to add/remove landmarks and watch the routing graph update dynamically. For 1000+ agents, level-of-detail rendering kicks in automatically to keep the animation responsive.
