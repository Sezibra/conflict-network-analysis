# Notebook 03: Network Metrics and Violence Outcomes

## What This Notebook Does

This notebook connects network structure to violence outcomes. We take the
centrality measures computed in Notebooks 01 and 02, merge them back with
the original event data, and test whether an actor's network position relates
to its involvement in violence against civilians. We also produce
publication-quality figures for the GitHub portfolio.

## Why This Matters

Network analysis on its own describes structure. The value for conflict
research comes from connecting structure to outcomes. The research question
driving this notebook: Does an actor's position in the conflict network
predict how much violence it inflicts on civilians?

This question has direct policy relevance. If structurally central actors
(those fighting on multiple fronts or bridging different conflict theaters)
also commit more one-sided violence, this suggests that military overextension
or strategic dominance creates conditions for civilian targeting. If peripheral
actors commit more civilian violence, the explanation points toward weaker
command structures or local militia dynamics.

Kramer (2017) argues that network position shapes armed group behavior because
it determines information flow, resource access, and strategic options.
Metternich et al. (2013) found that network structure among antigovernment
groups predicts conflict intensity. This notebook applies similar logic at
the actor level.

## Key Concepts

### Merging Network Data with Event Data

Network metrics live at the actor level (one row per armed group). Event data
lives at the event level (one row per violent incident). To test the
relationship between centrality and civilian targeting, we need to bring
these two levels together.

We do this by:
1. Computing each actor's centrality from the rivalry network (Notebook 01).
2. Going back to the full event dataset (including one-sided violence).
3. For each one-sided violence event, looking up the perpetrator's centrality.
4. Aggregating: for each actor, count how many one-sided violence events they
   committed and how many civilian fatalities they caused.
5. Comparing these counts to centrality measures.

### Correlation vs. Causation

We compute correlations between centrality and civilian violence. A positive
correlation means actors with more connections tend to commit more violence
against civilians. This is a descriptive finding, not a causal claim. We
cannot say centrality causes civilian targeting because both might be driven
by a third factor (such as military capability or territorial control).

This is appropriate for a portfolio project. Establishing the descriptive
pattern is the first step. Causal analysis comes in Project 5 (Causal
Inference for Conflict with ML).

### Spearman Rank Correlation

With only 15 actors in our network, we use Spearman rank correlation instead
of Pearson. Spearman works on ranks rather than raw values, making it robust
to outliers and non-normal distributions. It measures whether actors that
rank higher on centrality also rank higher on civilian violence.

### Publication-Quality Figures

Portfolio projects need polished visuals. This notebook produces final
versions of the network diagram and statistical plots with clean labels,
consistent color schemes, appropriate font sizes, and figure annotations
that explain the finding without requiring the reader to run the code.

## Data Pipeline

1. Reload the full Ethiopia event dataset (including one-sided violence).
2. Compute actor-level civilian violence statistics (events, fatalities).
3. Merge with centrality measures from the aggregate network.
4. Compute Spearman correlations between centrality and civilian violence.
5. Produce scatter plots showing the relationship.
6. Create publication-quality versions of key figures from all three notebooks.

## What to Expect

The Government of Ethiopia has both the highest centrality and the highest
number of one-sided violence events (as the primary perpetrator in the Tigray
conflict). TPLF, Government of Eritrea, and OLA also committed one-sided
violence. The correlation is likely positive but driven by a small number of
actors, so we interpret it cautiously.

## Python Libraries Used

- pandas: data merging and aggregation
- networkx: centrality computation
- scipy.stats: Spearman rank correlation
- matplotlib: publication-quality figures
- seaborn: enhanced scatter plots

## References

- Kramer, C.R. (2017). Network Theory and Violent Conflicts. Palgrave
  Macmillan.
- Metternich, N. et al. (2013). Antigovernment Networks in Civil Conflicts.
  American Journal of Political Science.
- Deutschmann, E., Lorenz, J., and Nardin, L.G. (eds.) (2020). Computational
  Conflict Research. Springer.
