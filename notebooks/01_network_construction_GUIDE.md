# Notebook 01: Network Construction and Static Analysis

## What This Notebook Does

This notebook builds conflict actor networks from UCDP event data on the
Ethiopia/Tigray conflict (November 2020 to November 2022). We extract pairs
of opposing actors from each recorded violent event, construct a network graph
where nodes are armed groups and edges represent hostile interactions, then
analyze the structure of that network using centrality measures and community
detection.

## Why Network Analysis for Conflict Research

Traditional conflict analysis treats each armed group as an independent unit
and studies their attributes (size, ideology, capabilities). Network analysis
takes a different approach: it studies the relationships between groups. This
matters because a group's behavior often depends on who it fights, who it
allies with, and where it sits in the broader web of conflict relationships.

Metternich et al. (2013) showed that the structure of antigovernment networks
(alliances and strategic interactions) predicts conflictual behavior. Perliger
and Pedahzur (2011) demonstrated that network position reveals which groups
play brokerage roles in terrorist organizations. Lilja (2012) found that rebel
group network structure influences flexibility in peace negotiations.

The Computational Conflict Research volume (Deutschmann et al., 2020)
highlights relational event models as a way to study temporal dynamics in
conflict event data, capturing patterns like reciprocity (if A attacks B, does
B attack A back?) and transitivity (the enemy of my enemy is my friend).

## Key Concepts

### Nodes and Edges

A network (or graph) has two components:
- Nodes (vertices): the actors. In our case, armed groups like the Government
  of Ethiopia, TPLF, Eritrean forces, OLA, Fano militia.
- Edges (links): the relationships between actors. In our case, each edge
  means two groups appeared on opposing sides of a violent event.

### Weighted Edges

An unweighted edge simply records that two groups interacted. A weighted edge
records how many times they interacted. If the Government of Ethiopia and
TPLF appeared as opponents in 400 events, the edge weight is 400. Higher
weight means more frequent conflict interaction.

### Directed vs. Undirected Networks

In a directed network, the edge from A to B is different from B to A. This
matters when one side initiates violence. In an undirected network, the edge
between A and B is symmetric. We use an undirected network because UCDP
records both sides of a conflict event without indicating who initiated.

### Centrality Measures

Centrality answers: "Which actors are most important in this network?" There
are several ways to define importance:

- Degree centrality: How many other groups does this actor fight? An actor
  with high degree centrality fights many different opponents. Weighted degree
  (also called strength) sums the edge weights, measuring total interaction
  volume.

- Betweenness centrality: How often does this actor sit on the shortest path
  between two other actors? An actor with high betweenness connects parts of
  the network that would otherwise be disconnected. In conflict terms, this
  actor is a "bridge" between separate theaters of war.

- Eigenvector centrality: Is this actor connected to other well-connected
  actors? An actor with high eigenvector centrality fights groups that
  themselves fight many others. This captures indirect influence.

- Closeness centrality: How close is this actor to all other actors in the
  network? An actor with high closeness can "reach" any other group through
  few intermediary connections.

### Community Detection (Louvain Algorithm)

The Louvain algorithm identifies clusters of nodes that are more densely
connected to each other than to the rest of the network. In conflict networks,
communities often represent alliance blocs or conflict theaters. For example,
groups fighting in Tigray form one cluster, groups fighting in Oromiya form
another.

The algorithm works by optimizing modularity, a measure of how well the
network divides into communities. High modularity means clear separation
between groups.

## Data Pipeline

1. Load the UCDP GED v25.1 dataset (same as Project 1).
2. Filter to Ethiopia, November 2020 to November 2022.
3. Keep events with two identified sides (side_a and side_b both present).
4. Clean actor names for consistency.
5. Build an edge list: each row is (actor_a, actor_b, event_count).
6. Construct a NetworkX graph from the edge list.
7. Compute centrality measures for each actor.
8. Run Louvain community detection.
9. Visualize the network with node size proportional to centrality and color
   indicating community membership.

## What to Expect

The Ethiopia/Tigray conflict network has a clear hub structure. The Government
of Ethiopia has the highest degree centrality because it fights on multiple
fronts (Tigray, Oromiya, Amhara). TPLF is the dominant node in the Tigray
theater. The OLA dominates in Oromiya. Community detection should separate
these theaters.

## Python Libraries Used

- pandas: data loading and filtering
- networkx: graph construction and centrality computation
- community (python-louvain): Louvain community detection
- matplotlib: static network visualization
- pyvis: interactive HTML network visualization

## References

- Deutschmann, E., Lorenz, J., and Nardin, L.G. (eds.) (2020). Computational
  Conflict Research. Springer.
- Metternich, N. et al. (2013). Antigovernment Networks in Civil Conflicts.
  American Journal of Political Science.
- Perliger, A. and Pedahzur, A. (2011). Social Network Analysis in the Study
  of Terrorism and Political Violence. PS: Political Science and Politics.
- Lilja, J. (2012). Rebel Group Network Structure and Peace Negotiations.
  International Interactions.
- Kramer, C.R. (2017). Network Theory and Violent Conflicts. Palgrave
  Macmillan.
