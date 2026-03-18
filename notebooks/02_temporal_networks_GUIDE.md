# Notebook 02: Temporal Network Evolution

## What This Notebook Does

This notebook tracks how the Ethiopia conflict actor network changes over
time. Instead of one static network for the entire study period, we build
separate networks for distinct conflict phases and compare their structure.
We track which actors enter or exit the network, how centrality rankings
shift, and whether the network becomes more fragmented or more consolidated
as the conflict evolves.

## Why Temporal Analysis Matters

A static network shows the aggregate picture but hides dynamics. Consider:
the Tigray conflict had distinct phases. Early months saw rapid territorial
gains by TPLF, followed by government counteroffensives with Eritrean and
Amhara support, then a humanitarian ceasefire, renewed offensives, and a
final peace agreement. Actor relationships shifted across these phases. Groups
that were peripheral early on became central later. Alliances formed and
dissolved.

The Computational Conflict Research volume (Deutschmann et al., 2020)
emphasizes relational event models for capturing these temporal dynamics.
Brandenberger's chapter on endogenous patterns in conflict event data shows
how interaction sequences reveal reciprocity, escalation, and diffusion
patterns that static analysis misses.

## Conflict Phases

We divide the study period into four phases based on the historical record
of the Tigray conflict:

Phase 1: War Outbreak (November 2020 to June 2021)
  The federal government launched a military operation in Tigray. Eritrean
  forces entered in support. TPLF lost major cities but regrouped.

Phase 2: TPLF Counteroffensive (July 2021 to December 2021)
  TPLF recaptured most of Tigray and advanced into Amhara and Afar regions.
  The government declared a state of emergency. OLA intensified operations
  in Oromiya.

Phase 3: Government Re-escalation (January 2022 to June 2022)
  Government forces, supported by Eritrean and Amhara regional forces, pushed
  TPLF back. Drone strikes intensified. Humanitarian access remained blocked.

Phase 4: Ceasefire and Peace Process (July 2022 to November 2022)
  A humanitarian truce began in March 2022 but collapsed. Fighting resumed
  before the Cessation of Hostilities Agreement was signed in November 2022.
  Violence gradually decreased.

## Key Concepts

### Temporal Network Snapshots

We take the full event dataset and split it by time period. For each period,
we build a separate network from the events in that window. This gives us a
sequence of network snapshots we compare side by side.

### Network Evolution Metrics

For each phase we track:
- Number of active actors (nodes)
- Number of active rivalries (edges)
- Network density (proportion of possible connections that exist)
- Total event count (overall conflict intensity)
- Centrality rankings (which actors are most important in each phase)
- Community structure (do alliance blocs shift?)

### Actor Trajectories

We follow individual actors across phases. An actor's centrality trajectory
shows whether it gained or lost network importance over time. For example,
if TPLF's degree centrality increases from Phase 1 to Phase 2 (during its
counteroffensive), this reflects its expanding military engagement across
multiple fronts.

## Methods

1. Assign each event to a conflict phase based on its date.
2. For each phase, build a weighted undirected network (same method as
   Notebook 01).
3. Compute centrality measures for each phase-specific network.
4. Visualize phase networks side by side.
5. Plot centrality trajectories for key actors across phases.
6. Track network-level metrics (density, node count, edge count) across
   phases.

## What to Expect

Phase 1 should show Government of Ethiopia and TPLF as the dominant dyad.
Phase 2 should show TPLF expanding its connections (fighting in Amhara and
Afar). Phases 3 and 4 should show the network contracting as fighting
concentrates and then declines toward the peace agreement. The OLA subnetwork
in Oromiya operates somewhat independently throughout.

## Python Libraries Used

- pandas: data filtering by date
- networkx: graph construction per phase
- community (python-louvain): community detection per phase
- matplotlib: side-by-side network plots and trajectory charts

## References

- Deutschmann, E., Lorenz, J., and Nardin, L.G. (eds.) (2020). Computational
  Conflict Research. Springer.
- Brandenberger, L. (2020). Interdependencies in Conflict Dynamics: Analyzing
  Endogenous Patterns in Conflict Event Data Using Relational Event Models.
  In Computational Conflict Research, Springer.
