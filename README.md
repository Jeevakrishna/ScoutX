<div align="center">
  <img src="scoutX.png" alt="Scout X Logo" width="880" />

</div>

#  Scout X: Big-Five Tactical Scouting Dashboard

**Scout X** is a high-performance tactical scouting tool covering every outfield player in Europe's Big Five leagues — Premier League, La Liga, Bundesliga, Serie A, and Ligue 1. By merging advanced metrics from multiple data providers with custom tactical algorithms, Scout X bridges the gap between raw data and actionable scouting insights.

---

##  Core Philosophy: Quality vs. Style

Most scouting tools rely on "black-box" ratings or raw volume. Scout X uses a transparent, dual-signal approach to evaluate players:

1.  **Quality (Q)**: How effective is the player at the specific tasks a role demands?
2.  **Style Fit (S)**: Does the player's statistical "shape" (spikes and troughs) match the role archetype?

By blending these two signals, Scout X prevents "elite-player carpet-bombing"—where world-class, highly versatile players dominate every leaderboard regardless of the role being searched for. Instead, it prioritizes players whose profiles genuinely align with the specific tactical requirements and responsibilities of the role.

---

##  Methodology & Formulas

Scout X is built on transparent mathematical foundations. Every grade can be traced back to the following logic:

### 1. Data Processing & Normalization
*   **Minutes Floor**: Only players with ≥ 900 minutes are included to ensure statistical significance.
*   **Positional Peers**: Percentile ranks are calculated strictly within position groups ( Defenders,  Midfielders,  Forwards).
*   **Percentile Calculation**:
    $$P = \text{rank}(v) / N \times 100$$
    *For inverted stats (e.g., Dispossessed), $$P=(1−rank(v)/N)×100$$ .

### 2. League Pressing Adjustment
To neutralize the structural bias of high-pressing leagues (like the Bundesliga) vs. possession-heavy ones (like La Liga), Scout X can apply a mean-normalization to defensive volume stats:
$$\text{Adjusted Value} = \text{Value} \times \frac{\text{Global Mean}}{\text{League Mean}}$$
This preserves individual deviation from the league baseline while removing league-wide inflation.

### 3. Role Grading (Quality - Q)
Roles are defined by stat weights on a 0–5 scale. Only sliders above 2.5 ("Neutral") contribute to the grade.
*   **Importance**: $I = \text{clip}((w - 2.5) / 2.5, 0, 1)$
*   **Category Fit**: $\text{CatFit} = \frac{\sum (P \times I)}{\sum I}$
*   **Category Influence (Pull)**: $\text{CatPull} = \text{mean}(I)$
*   **Quality Grade**: $Q = \frac{\sum (\text{CatFit} \times \text{CatPull})}{\sum \text{CatPull}}$


#### Key Terminology: CatScores & CatPull
To understand the relationship between a player and a role, Scout X uses two primary category-level metrics:
*   **CatScores (Absolute Strength)**: The raw average of a player's percentile ranks within a category (e.g., "Passing"). This represents how good the player is at that category in general. It is the data source for the **Attribute Radar**.
*   **CatPull (Tactical Importance)**: The average "Importance" $(I)$ of all stats in a category for a specific role. It defines how much a category "pulls" the final grade. For example, a *Regista* has a high **CatPull** for Passing but a low **CatPull** for Shooting.


### 4. Style Fit (S)
Style Fit measures the correlation between the player's category profile and the role's emphasis profile, independent of quality level.
*   **Centered Shapes**: 
    $PlayerShape = \text{CatScores} - \text{mean}(\text{CatScores})$
    $RoleShape = \text{CatPull} - \text{mean}(\text{CatPull})$
*   **Cosine Similarity**:
    $S = \frac{\cos(PlayerShape, RoleShape) + 1}{2} \times 100$

### 5. Final Headline Grade (The Blend)
The final rating shown in the UI (and represented by FM-style stars) is an equal-weighted average:
$$\text{Grade} = \frac{Q + S}{2}$$

### 6. Similarity Matching
Similar players are found using a category-equalized Manhattan distance:
$$\text{Similarity} = 100 - \frac{\sum (|P_{target} - P_{peer}| \times W)}{\sum W}$$
*Weights ($W$) are adjusted so each of the 7 statistical categories contributes equally, preventing data-heavy categories from drowning out specialized ones.*

---

##  Key Features

###  Five Core Workspaces
1.  **Scouting Report**: Deep-dive into a single player's fit for any role, with cohort filtering (U21, same-league, etc.).
2.  **Role Ranking**: Europe-wide leaderboards for 22+ tactical archetypes (Regista, Mezzala, Inverted Wing-Back, etc.).
3.  **Stat Leaderboards**: Pure volume and efficiency rankings for every metric in the catalog.
4.  **Player Comparison**: Side-by-side visualization of two players' styles and quality.
5.  **Shortlist Analyser**: Batch-process a custom list of targets to see which role they fit best.

###  Universal Export & Selection
*   **Export CSV**: Instant client-side export for visible tables or manual selections across all pages.
*   **Selection System**: Multi-select players across different position groups and save them to a global export list.

###  Technical Highlights
*   **Responsive UI**: Sticky navigation, mobile-first design, and high-performance radar charts.
*   **Dynamic Data**: The app reloads its 1,400+ player dataset automatically if the source CSV is updated.


---

##  Tech Stack
*   **Framework**: Streamlit (Python)
*   **Data Science**: Pandas, NumPy, SciPy
*   **Visualization**: Plotly, Custom HTML/CSS
*   **Data Sources**: FBref, Sofascore, Understat

---

##  File Structure
```
.
├── scouting_app.py                 # Core application logic & UI
├── player_scouting_data_2026.csv   # Master merged dataset
├── requirements.txt                # Dependencies
└── README.md                       # Documentation
```

---

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/your-repo/scout-x.git
cd scout-x
```

### 2. Create a Virtual Environment

#### Windows (PowerShell)

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

#### Windows (Command Prompt)

```cmd
python -m venv .venv
.venv\Scripts\activate.bat
```

#### macOS / Linux

```bash
python -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run Scout X

```bash
streamlit run scouting_app.py
```

### 5. Open in Browser

```text
http://localhost:8501
```

---
### live demo : https://scoutx.streamlit.app/
---


##  Roadmap
- [x] Universal CSV Export & Selection
- [x] Premium "Scout X" Branding & Navigation
- [ ] Advanced Team-Possession Normalization
- [ ] Goalkeeper-specific Model & Dashboard
- [ ] Historical Time-Series Tracking

---

##  Acknowledgments
This project is made possible by the incredible data infrastructure provided by **FBref**, **Sofascore**, and **Understat**. Statistical scraping and processing are powered by `soccerdata` and `ScraperFC`. Tactical role archetypes are inspired by the *Football Manager* taxonomy.
