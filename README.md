# FiTrace — Cyberpunk-Themed Workout Tracking System 

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.9–3.12-blue?logo=python" alt="Python 3.9–3.12"/>
  <img src="https://img.shields.io/badge/Flask-3.x-black?logo=flask" alt="Flask"/>
  <img src="https://img.shields.io/badge/Chart.js-4.x-orange?logo=chartdotjs" alt="Chart.js"/>
  <img src="https://img.shields.io/badge/HTML5-CSS3-blue?logo=html5" alt="HTML5/CSS3"/>
  <img src="https://img.shields.io/badge/License-MIT-yellow" alt="MIT License"/>
</p>

**FiTrace** is an end-to-end, interactive workout calendar and fitness logging platform designed with a high-fidelity cyberpunk aesthetic. By combining dynamic frontend interactions—custom slider inputs, real-time tooltips, and interactive **Chart.js** bar charts—with a lightweight, multi-user **Flask** backend, FiTrace logs, persists, and visualizes muscle-group training volumes to empower athletes with data-driven insights.

---

## Key Features

| Feature | Description |
|---|---|
| **Cyberpunk UI Dashboard** | Highly polished dark-mode interface utilizing glassmorphism, glowing neon borders, and dynamic styling. |
| **Interactive Calendar** | Sleek month-by-month grid rendering of calendar days where scheduled workouts are color-coded. |
| **Real-Time Tooltip Charts** | Hovering over logged days displays custom micro-tooltips and builds immediate muscle-group volume charts via Chart.js. |
| **Dynamic Exercise Filter** | Adaptive dropdown options that adjust exercises based on selected body parts (chest, back, shoulders, etc.). |
| **Persistence Layer** | Multi-user authentication and database modeling using a structured JSON backend for zero-overhead local storage. |

---

## Technology Stack

- **Frontend**: HTML5, CSS3, JavaScript (ES6+), Orbitron & Rajdhani Typefaces (Google Fonts), Neon Glassmorphism UI, Chart.js (version 4.x via CDN)
- **Backend**: Flask (Python 3.9–3.12), session-based user authentication
- **Storage**: JSON-based persistent flat-file database (`users.json`)

---

## Project Structure

```
FITRACE/
│
├── workoutapppy/
│   ├── static/
│   │   └── script.js         ← JavaScript logic (Chart.js integration, tooltips, slider display)
│   │
│   ├── templates/
│   │   ├── index.html        ← Login / Landing screen (neon cyber theme)
│   │   ├── register.html     ← User registration screen
│   │   ├── calendar.html     ← Core dashboard displaying months and tooltip visualizations
│   │   └── workout.html      ← Add, edit, delete workout sets and session parameters
│   │
│   ├── app.py                ← Flask application serving API and HTML templates
│   └── users.json            ← Application users and session database
│
├── requirements.txt          ← Python package requirements (Flask, Gunicorn)
└── README.md                 ← Project documentation
```

---

## Data Structure & User Schema

The platform maps workouts to individual users using unique email keys within a secure, lightweight JSON database.

| Level | Key/Field | Type | Description |
| :--- | :--- | :--- | :--- |
| **Root** | `email` | String | Unique identifier/login email key for user profile |
| **Profile** | `password` | String | Plain-text credentials (configured for local standalone use) |
| **Profile** | `workouts` | Object | Dictionary mapping date strings (`YYYY-MM-DD`) to workout arrays |
| **Workout** | `body_part` | String | Target muscle group (e.g., chest, back, shoulders, triceps) |
| **Workout** | `exercise` | String | Logged exercise name (e.g., bench press, overhead press) |
| **Workout** | `weight` | String/Int | Mass loaded for the set (in pounds/kg) |
| **Workout** | `reps` | String/Int | Count of successful repetitions completed |
| **Workout** | `notes` | String | Optional comments or performance details |

---

## Mathematics & Volume Calculation Logic

### 1. Daily & Muscle-Group Volume Calculation
The application monitors training load by computing total volume on a per-set basis. Before plotting the Chart.js visualizer on calendar hover, the frontend processes the raw weights and reps:
$$\text{Set Volume} = \text{Weight} \times \text{Reps}$$

Daily training volume for any specific muscle group is accumulated across all completed sets on that day:
$$\text{Muscle Group Volume} = \sum_{i=1}^{N} \left(\text{Weight}_i \times \text{Reps}_i\right)$$
where $N$ is the number of logged sets targeting that muscle group on the selected day.

### 2. Multi-Set Progression
The UI aggregates sets dynamically to prevent duplicate listings while mapping distinct metrics. Muscle groups are represented by neon palette configurations:
$$\text{Total Daily Volume} = \sum_{m \in \text{Muscle Groups}} \text{Volume}_m$$

---

## Installation

### Prerequisites
- **Python 3.9 – 3.12**
- **Git**

### 1. Clone the Repository
```bash
git clone https://github.com/<your-username>/FiTrace.git
cd FiTrace
```

### 2. Create a Virtual Environment
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS / Linux
python -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

---

## Complete Workflow (from scratch)

Follow these steps **in order** to run your local cyberpunk workout tracker.

### Step 1 — Verify Project Configuration
1. Open the project directory.
2. Ensure you have the `requirements.txt` file ready.

### Step 2 — Run the Flask Server
1. Navigate into the application subdirectory or run from the project root:
   ```bash
   python workoutapppy/app.py
   ```
2. Open your browser and navigate to: **http://127.0.0.1:5000**

### Step 3 — Register and Authenticate
1. From the landing screen, click **Register** to create a new profile.
2. Log in using your registered credentials.
3. Upon successful validation, the server redirects you to the calendar dashboard showing the current month.

### Step 4 — Log Workout Sessions
1. Click on any active date cell on the calendar grid to access the **Workout Logger**.
2. Select a target **Muscle Group** from the dropdown menu.
3. Pick the specific **Exercise** (filtered dynamically based on the selected muscle group).
4. Use the sliders to input **Weight** and **Reps**.
5. Add optional notes, and click **Add Set**.
6. Saved sets are listed below the entry console with options to select and delete them.

### Step 5 — Visualize Training Data
1. Navigate back to the **Calendar View**.
2. Hover over any date where workouts have been logged.
3. A sleek, animated cybernetic tooltip displays:
   - A list of exercises performed.
   - A live **Chart.js** bar chart depicting total volume accumulated for each muscle group.
