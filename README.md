# Conflict Actor Network Analysis

Social network analysis of armed group interactions during the Ethiopia/Tigray conflict (November 2020 to November 2022), using UCDP georeferenced event data.

## Navigation

| Section | Description |
|---------|-------------|
| [Motivation](#motivation) | Why network analysis matters for conflict research |
| [Key Findings](#key-findings) | Main results across all three notebooks |
| [Data](#data-sources) | Two UCDP datasets covering 1,764 conflict events |
| [Methods](#methods) | Graph construction, centrality analysis, community detection, temporal snapshots |
| [Results in Detail](#results-in-detail) | Figures and interpretation for each analytical step |
| [Limitations](#limitations) | Honest assessment of constraints |
| [Notebooks](#notebooks) | Three-notebook analytical progression |
| [How to Reproduce](#how-to-reproduce) | Setup and replication instructions |
| [References](#key-references) | Academic sources |

## Motivation

Traditional conflict analysis treats each armed group as an independent unit and studies their attributes: size, ideology, capabilities. Network analysis takes a different approach. It studies the relationships between groups, asking who fights whom, how often, and how that structure changes over time.

This matters because a group's behavior often depends on its position in the broader web of conflict relationships. Metternich et al. (2013) showed that the structure of antigovernment networks predicts conflictual behavior. Lilja (2012) found that rebel group network structure influences flexibility in peace negotiations. Kramer (2017) argued that network position shapes armed group behavior because it determines information flow, resource access, and strategic options.

This project applies network methods to the Ethiopia/Tigray conflict (November 2020 to November 2022), building on the same case study used in Project 1 (geospatial analysis). It constructs conflict actor networks from UCDP event data, tracks how network structure evolves across four conflict phases, and tests whether an actor's network position relates to its violence against civilians.

## Key Findings

**The conflict network is fragmented into five disconnected components.** 15 actors and 11 rivalry edges form five separate sub-networks. Government of Ethiopia is the only actor bridging multiple conflict fronts (Tigray, Oromiya, Gambella). TPLF and OLA each fight only the government.

**The network consolidated over time.** Active actors contracted from 8 in Phase 1 to 5 in Phase 4, while battle events increased from 242 to 370. Fewer actors fought more intensely as the conflict approached the November 2022 ceasefire.

**The deadliest actor against civilians is invisible in the rivalry network.** Government of Eritrea committed the most civilian fatalities (5,995) but does not appear as an independent combatant in UCDP data. All its recorded events are civilian targeting or coalition entries with Government of Ethiopia. This reveals a structural limitation in using UCDP for network construction.

**Among actors in the rivalry network, centrality tracks civilian violence.** Government of Ethiopia has both the highest centrality and the highest civilian violence count (270 events, 3,965 fatalities). Actors absent from the rivalry network committed fewer events (247) but caused more total fatalities (6,264), driven entirely by Government of Eritrea.

![Publication Figure](outputs/03_publication_figure.png)

## Data Sources

| Dataset | Version | Source | Role |
|---------|---------|--------|------|
| UCDP Georeferenced Event Dataset (GED) | v25.1 | [ucdp.uu.se](https://ucdp.uu.se/downloads/) | Event-level conflict data with actor pairs, locations, fatalities |
| UCDP Dyadic Dataset | v25.1 | [ucdp.uu.se](https://ucdp.uu.se/downloads/) | Structured conflict dyads (pairs of warring parties) |

**Filtered scope:** Ethiopia, November 2020 to November 2022.

| Category | Count |
|----------|-------|
| Total events | 1,764 |
| State-based conflict events | 1,203 |
| One-sided violence events | 554 |
| Non-state conflict events | 7 |
| Battle events (excluding civilian targeting) | 1,210 |
| Pairwise actor interactions | 1,214 |

## Methods

| Method | Tool | Purpose |
|--------|------|---------|
| Graph construction | NetworkX | Build weighted undirected networks from actor co-occurrence in conflict events |
| Centrality analysis | NetworkX | Degree, betweenness, eigenvector, closeness centrality |
| Community detection | python-louvain | Louvain algorithm to identify actor clusters |
| Temporal snapshots | NetworkX | Phase-specific networks across four conflict periods |
| Violence outcome analysis | pandas | Descriptive comparison of network position and civilian targeting |
| Static visualization | matplotlib | Network diagrams, trajectory plots, bar charts |
| Interactive visualization | pyvis | HTML network graphs with hover details |

## Results in Detail

### Network Structure: 15 Actors, 5 Components

The aggregate conflict network has 15 actors connected by 11 rivalry edges. Network density is 0.105, meaning only 10.5% of all possible actor pairs have recorded conflict interactions. The network splits into five disconnected components:

| Community | Members | Interpretation |
|-----------|---------|----------------|
| 0 | Government of Ethiopia, TPLF, OLA, GLF | Main armed conflict |
| 1 | Gumuz, Amhara, Oromo, Shinasha, Agaw | Ethnic communal violence |
| 2 | Al-Shabaab, Government of Somalia | Cross-border spillover |
| 3 | Murle, Nuer | Pastoral conflict |
| 4 | Burji, Guji | Localized ethnic clash |

Modularity is 0.028, reflecting the disconnected components rather than meaningful internal structure. The Louvain algorithm separated the already disconnected parts.

![Static Network](outputs/01_static_network.png)

### Centrality Rankings: Government of Ethiopia Dominates

| Actor | Degree | Interactions | Degree Centrality | Betweenness Centrality |
|-------|--------|-------------|-------------------|----------------------|
| Government of Ethiopia | 3 | 1,197 | 0.214 | 0.033 |
| TPLF | 1 | 699 | 0.071 | 0.000 |
| OLA | 1 | 495 | 0.071 | 0.000 |
| Gumuz | 4 | 7 | 0.286 | 0.055 |
| Al-Shabaab | 1 | 6 | 0.071 | 0.000 |

Government of Ethiopia has the highest interaction volume (1,197) and is the only actor fighting on multiple fronts. Gumuz has the highest degree centrality (0.286) because it connects to four different ethnic groups in localized conflicts, but its total interaction volume is low (7 events).

### Temporal Evolution: Consolidation Across Four Phases

The conflict passed through four distinct phases. The network shrank while violence intensified.

| Phase | Period | Battle Events | Active Actors | Active Edges | Density |
|-------|--------|---------------|---------------|--------------|---------|
| 1. War Outbreak | Nov 2020 to Jun 2021 | 242 | 8 | 7 | 0.250 |
| 2. TPLF Counteroffensive | Jul 2021 to Dec 2021 | 277 | 8 | 5 | 0.179 |
| 3. Re-escalation | Jan 2022 to Jun 2022 | 321 | 8 | 5 | 0.179 |
| 4. Ceasefire and Peace | Jul 2022 to Nov 2022 | 370 | 5 | 3 | 0.300 |

Peripheral actors (ethnic communal violence groups) exited after Phase 1, leaving a smaller core. By Phase 4, only five actors remained, all connected through the Government of Ethiopia in a star-shaped network.

![Phase Networks](outputs/02_phase_networks.png)

### Actor Trajectories: Opposite Arcs for TPLF and OLA

Government of Ethiopia's interaction volume rose steadily across all phases (238, 275, 319, 365). Its degree centrality increased from 0.286 to 0.500 as peripheral actors exited the network.

TPLF and OLA followed opposite arcs. OLA rose from 42 interactions in Phase 1 to 196 in Phase 3 (reflecting the intensification of the Oromiya insurgency), then fell to 145 in Phase 4. TPLF dipped from 196 to 122 across Phases 1 to 3, then surged to 220 in Phase 4 during the final battles before the ceasefire.

![Actor Trajectories](outputs/02_actor_trajectories.png)

### Network Metrics Over Time: Fewer Actors, More Violence

Network density dropped from 0.250 to 0.179 in the middle phases as edges disappeared, then jumped to 0.300 in Phase 4 when the remaining actors were tightly interconnected. Battle events rose steadily across all four phases, peaking at 370 in Phase 4 immediately before the November 2022 ceasefire.

![Network Metrics](outputs/02_network_metrics.png)

### Civilian Violence: Network Position and One-Sided Violence

554 one-sided violence events caused 11,315 civilian fatalities. Six actors committed these events. Only three of them appear in the rivalry network.

| Actor | OSV Events | Civilian Fatalities | In Rivalry Network |
|-------|-----------|--------------------|--------------------|
| Government of Ethiopia | 270 | 3,965 | Yes |
| Government of Eritrea | 220 | 5,995 | No |
| TPLF | 54 | 881 | Yes |
| OLA | 45 | 205 | Yes |
| Fano | 25 | 235 | No |
| OLA (Fekade Abdisa faction) | 2 | 34 | No |

Actors absent from the rivalry network committed fewer events (247) but caused more civilian fatalities (6,264) than those present (369 events, 5,051 fatalities). Government of Eritrea drives this entirely: it committed the most civilian fatalities (5,995) while remaining invisible in the rivalry network because all its UCDP entries are coded as civilian targeting or coalition operations with Government of Ethiopia.

![Civilian Violence](outputs/03_civilian_violence_by_actor.png)

## Limitations

1. **UCDP coding conventions hide some actors.** Government of Eritrea and Fano militia appear only in one-sided violence or coalition entries, making them invisible in the rivalry network. Any network analysis using UCDP will undercount these actors' battlefield roles.

2. **Small network limits statistical analysis.** With 15 actors and only 3 overlapping between the rivalry network and civilian violence data, no correlation test is statistically appropriate. Findings are descriptive.

3. **The network is dominated by one dyad.** Government of Ethiopia vs. TPLF accounts for 699 of 1,214 pairwise interactions (57.6%). This concentration limits what centrality measures reveal beyond confirming the obvious.

4. **Undirected edges lose information.** UCDP does not record which side initiated violence, so all edges are symmetric. A directed network would better capture asymmetric relationships (e.g., government offensives vs. rebel ambushes).

5. **Coalition splitting is imperfect.** When UCDP records "Government of Eritrea, Government of Ethiopia" as side_a, we split this into two separate actors. We cannot determine which coalition member was responsible for how much of the violence in that event.

6. **Temporal phase boundaries are chosen manually.** The four phases reflect the historical record, but alternative periodizations (e.g., quarterly, event-driven breakpoints) would produce different temporal patterns.

## Project Structure

```
conflict-network-analysis/
├── data/
│   ├── GEDEvent_v25_1.csv
│   └── Dyadic_v25_1.csv
├── notebooks/
│   ├── 01_network_construction.ipynb
│   ├── 01_network_construction_GUIDE.md
│   ├── 02_temporal_networks.ipynb
│   ├── 02_temporal_networks_GUIDE.md
│   ├── 03_network_outcomes.ipynb
│   └── 03_network_outcomes_GUIDE.md
├── outputs/
│   ├── 01_static_network.png
│   ├── 01_interactive_network.html
│   ├── 02_actor_trajectories.png
│   ├── 02_phase_networks.png
│   ├── 02_network_metrics.png
│   ├── 03_civilian_violence_by_actor.png
│   └── 03_publication_figure.png
├── .gitignore
└── README.md
```

## Notebooks

| Notebook | Description | Key Output |
|----------|-------------|------------|
| 01 Network Construction | Builds aggregate conflict network, computes centrality measures, runs Louvain community detection, produces static and interactive visualizations | 15 actors, 11 edges, 5 components |
| 02 Temporal Networks | Splits conflict into four phases, builds phase-specific networks, tracks actor centrality trajectories and network-level metrics | Actors: 8 to 5, events: 242 to 370 |
| 03 Network Outcomes | Merges network metrics with one-sided violence data, compares civilian targeting across actors with and without rivalry network positions | Eritrea: 5,995 fatalities, invisible in network |

Each notebook has a companion `_GUIDE.md` file explaining the concepts, methods, and references used.

## How to Reproduce

1. Download UCDP GED v25.1 (CSV) from https://ucdp.uu.se/downloads/ and place in `data/`
2. Download UCDP Dyadic Dataset v25.1 (CSV) from https://ucdp.uu.se/downloads/ and place in `data/`
3. Install dependencies: `pip install pandas networkx python-louvain pyvis matplotlib seaborn scipy`
4. Run notebooks in order: 01, 02, 03

All data processing runs locally. No API keys or external authentication required.

## Key References

- Deutschmann, E., Lorenz, J., and Nardin, L.G. (eds.) (2020). *Computational Conflict Research*. Springer.
- Kramer, C.R. (2017). *Network Theory and Violent Conflicts*. Palgrave Macmillan.
- Metternich, N. et al. (2013). Antigovernment Networks in Civil Conflicts. *American Journal of Political Science*.
- Perliger, A. and Pedahzur, A. (2011). Social Network Analysis in the Study of Terrorism and Political Violence. *PS: Political Science and Politics*.
- Lilja, J. (2012). Rebel Group Network Structure and Peace Negotiations. *International Interactions*.
- Brandenberger, L. (2020). Interdependencies in Conflict Dynamics: Analyzing Endogenous Patterns in Conflict Event Data Using Relational Event Models. In *Computational Conflict Research*, Springer.

## Skills Demonstrated

Graph construction from relational conflict data, centrality analysis (degree, betweenness, eigenvector, closeness), Louvain community detection, temporal network snapshots and evolution tracking, interactive network visualization with pyvis, integration of network metrics with event-level violence data, descriptive analysis of network position and civilian targeting.

## Portfolio Context

| Project | Topic | Status |
|---------|-------|--------|
| 1 | [Conflict Event Data Analysis and Geospatial Visualization](https://github.com/Sezibra/conflict-event-analysis) | Complete |
| 2 | [LLM-Powered Conflict Text Analysis](https://github.com/Sezibra/conflict-text-analysis) | Complete |
| 3 | **Conflict Actor Network Analysis** (this repo) | **Complete** |
| 4 | [Conflict Forecasting with Machine Learning](https://github.com/Sezibra/conflict-forecasting-ml) | Complete |
| 5 | [Causal Inference for Conflict with ML](https://github.com/Sezibra/conflict-causal-inference) | Complete |
| 6 | [Satellite Imagery for Conflict Damage Assessment](https://github.com/Sezibra/conflict-satellite-damage) | Complete |
