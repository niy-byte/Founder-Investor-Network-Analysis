# Founder–Investor Network Analysis: Indian Startup Ecosystem

An empirical network analysis and econometric study investigating how founders, startups, and investors are connected across the Indian venture capital ecosystem, and whether network positions directly correlate with subsequent funding outcomes.

---

## Executive Project Summary

### Aim & Core Question
This project investigates how founders and investors are connected across the Indian startup ecosystem and whether network position directly predicts subsequent funding outcomes.

### Methods & Rationale
Using 1,209 verified deals from the Indian venture ecosystem, we constructed a tripartite network (Founder–Startup–Investor) comprising 4,002 entities and 3,881 ties. Multi-metric centralities (Degree, Betweenness, Eigenvector, and PageRank) were computed to quantify connectivity, brokerage, and prestige. We applied modularity maximization to identify co-investment syndicates and calculated Gini and Herfindahl-Hirschman Index (HHI) metrics to measure capital concentration. Finally, heteroskedasticity-robust OLS regressions evaluated the relationship between network centrality and capital raised, controlling for stage, sector, and geographic fixed effects.

### Results & Ecosystem Interpretation
1. **Structural Connectivity**: 85.3% of entities form a single connected component, with Bengaluru and Delhi-NCR anchoring over 60% of all activity.
2. **Market Concentration**: High capital inequality exists (Gini = 0.548); the top 5% of investors (e.g., Tiger Global, Peak XV / Sequoia, Accel, Blume Ventures) dominate 24.5% of deal flow as core gatekeepers.
3. **Funding Premium**: Startup degree ($\beta = 0.223, p < 0.001$) and investor PageRank prestige ($\beta = 276.69, p < 0.001$) significantly increase funding round size. Investor network prestige acts as a vital certification signal in Indian venture financing.

---

## Empirical Visualizations and Results

### 1. Tripartite Venture Capital Network Graph
Heterogeneous network mapping founders, startups, and institutional investors. Node sizes scale with degree centrality, and colors denote entity types (Purple: Investors, Cyan: Startups, Orange: Founders).

![Tripartite Network Graph](images/network_graph_visualization.png)

### 2. Centrality Correlation Matrix
Spearman and Pearson correlation profiles across Degree, Betweenness, Eigenvector, and PageRank centralities for institutional investors in the ecosystem.

![Centrality Correlation Heatmap](images/centrality_correlation_heatmap.png)

### 3. Investor Market Concentration & Lorenz Curve
Empirical distribution of deal flow and capital connectivity across the investor population, illustrating high concentration (Gini coefficient = 0.548) and power-law distribution tails.

![Investor Concentration and Lorenz Curve](images/investor_concentration_lorenz.png)

### 4. Geographic Hubs & Sectoral Deal Allocations
Comparative deal distribution across primary startup hubs (Bengaluru, Delhi-NCR, Mumbai) and key verticals (FinTech, EdTech, E-commerce).

![Geographic Hub and Sectoral Deal Distribution](images/geographic_sector_distribution.png)

---

## Methodology & Model Justification

This section outlines the methodological and econometric justification for every model, metric, and analytical tool deployed in the project.

### 1. Tripartite Heterogeneous Network Architecture (Founder -> Startup <- Investor)
- **How Measured**: Constructed a multi-relational graph $G = (V, E)$ with three explicit node classes (Founders, Startups, Investors) connected by 'Founded' and 'Invested' relations weighted by round capital ($USD Mn).
- **Why Chosen & Why Not Alternatives**: Venture ecosystems are fundamentally tripartite: human capital (founders) and institutional financial capital (investors) interface through corporate vehicles (startups). Standard unipartite or simple bipartite projections collapse or discard the human founder dimension entirely.

### 2. Degree Centrality (Direct Deal Flow & Portfolio Volume)
- **How Measured**: Measured as the normalized count of incident edges for each vertex: $C_D(v) = \frac{\text{deg}(v)}{|V| - 1}$, representing an investor's portfolio volume or a startup's syndicate size.
- **Why Chosen & Why Not Alternatives**: Provides an unweighted baseline of direct market activity and deal access. Closeness centrality was avoided because startup networks contain disconnected components where geodesic distances become infinite or ill-defined.

### 3. Betweenness Centrality (Information Brokerage & Gatekeeping)
- **How Measured**: Calculated the fraction of all network shortest paths traversing a given node: $C_B(v) = \sum_{s \neq v \neq t} \frac{\sigma_{st}(v)}{\sigma_{st}}$.
- **Why Chosen & Why Not Alternatives**: Identifies structural bridge investors and serial founders who bridge otherwise disconnected industry sectors and regional hubs. Unlike closeness, betweenness isolates gatekeeping power and informational leverage across fragmented sub-networks.

### 4. Eigenvector Centrality (Prestige by Elite Association)
- **How Measured**: Determined by the principal eigenvector of the adjacency matrix: $\lambda x_v = \sum_{u \in N(v)} x_u$, assigning higher scores to entities tied to already-influential entities.
- **Why Chosen & Why Not Alternatives**: In venture capital, who backs an entity matters as much as how many back it. Simple degree counts treat an investment from an elite lead fund identically to an isolated angel; eigenvector centrality rewards prestige-by-association.

### 5. PageRank Algorithm (Recursive Capital Flow & Market Prestige)
- **How Measured**: Computed stationary random-walk probability with damping factor $\alpha = 0.85$ and capital-weighted transition matrices across bilateral funding ties.
- **Why Chosen & Why Not Alternatives**: Eigenvector centrality frequently collapses onto localized dense clusters in sparse, bipartite venture graphs. PageRank's teleportation damping prevents localized rank trapping, realistically modeling diffuse prestige across the Indian venture ecosystem.

### 6. Modularity Maximization (Investor Co-Investment Syndicates)
- **How Measured**: Applied modularity optimization on the projected investor-investor graph (edge weights = shared portfolio companies) to partition funds into collaborative cliques.
- **Why Chosen & Why Not Alternatives**: Detects natural co-investment syndicates endogenously without requiring an arbitrary pre-specified number of clusters ($k$). Distance-based clustering (e.g., k-means, GMM) requires arbitrary Euclidean embeddings that distort topological network cohesion.

### 7. Gini Coefficient & Herfindahl-Hirschman Index (HHI) (Network Inequality)
- **How Measured**: Gini was calculated from the empirical Lorenz curve of investor deal counts; HHI was computed as the sum of squared percentage deal shares across all active institutional funds.
- **Why Chosen & Why Not Alternatives**: These are standard economic measures for distribution inequality and market concentration. Basic variance or standard deviation cannot quantify bounded, scale-invariant power-law skewness in venture deal allocation.

### 8. Log-Linear OLS Regression with Robust Standard Errors (HC1)
- **How Measured**: Estimated $\log(\text{Funding Amount}_i)$ against startup degree, investor PageRank prestige, founder team size, and sector/hub/stage fixed effects using White's heteroskedasticity-consistent variance estimator.
- **Why Chosen & Why Not Alternatives**: The research objective is parameter inference and hypothesis testing (verifying if network position causes higher funding), not black-box prediction. OLS provides interpretable percentage elasticities; machine learning models lack formal statistical p-values. Standard OLS was adjusted because venture capital returns exhibit severe heteroskedasticity.

### 9. Interactive Force-Directed Network Visualization
- **How Measured**: Rendered dynamic graph with repulsion-gravity physics, mapping node types to distinct colors, sizing by degree, and enabling search, category filtering, and real-time node inspection.
- **Why Chosen & Why Not Alternatives**: Static plots degenerate into unreadable hairballs when visualizing thousands of entities. The interactive visualizer enables panning, zooming, filtering, and local inspection of investment syndicates directly in the browser.

---

## Repository Structure

```text
├── Indian_Startup.csv                     # Raw venture funding and entity dataset (1,209 rounds)
├── Network-Investor analysis.ipynb        # Primary analysis and modeling notebook
├── founder_investor_network.html          # Interactive PyVis network visualization
├── Project_Summary_Network_Analysis.docx  # Executive summary document (<200 words)
├── Methodology_and_Model_Justification.docx # Detailed model and methodology justification
├── requirements.txt                       # Python dependencies
├── .gitignore                             # Git ignore rules for virtualenvs and temporary files
├── README.md                              # Complete GitHub documentation with figures
└── images/                                # High-resolution analytical figures and network diagrams
    ├── network_graph_visualization.png
    ├── centrality_correlation_heatmap.png
    ├── investor_concentration_lorenz.png
    └── geographic_sector_distribution.png
```

---

## Quick Start & Installation

### 1. Clone the Repository
```bash
git clone https://github.com/niy-byte/Founder-Investor-Network-Analysis.git
cd Founder-Investor-Network-Analysis
```

### 2. Set Up Virtual Environment & Dependencies
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 3. Run the Jupyter Notebook
```bash
jupyter notebook "Network-Investor analysis.ipynb"
```

### 4. View the Interactive Visualization
Open `founder_investor_network.html` in any web browser:
```bash
# On Linux
xdg-open founder_investor_network.html

# On macOS
open founder_investor_network.html
```
