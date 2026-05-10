---
author: "Kyle Jones"
date_published: "April 23, 2025"
date_exported_from_medium: "November 10, 2025"
canonical_link: "https://medium.com/@kyle-t-jones/factorial-designs-for-causal-inference-in-econometric-analysis-of-public-policy-ef978b229bc9"
---

# Factorial Designs for Causal Inference in Econometric Analysis of Public Policy Public policy interventions often include multiple interacting
components. A job training program might combine skills workshops...

### **Factorial Designs for Causal Inference in Econometric Analysis of Public Policy**
Public policy interventions often include multiple interacting components. A job training program might combine skills workshops, mentorship, and placement support. A safety initiative might pair policing strategies with community outreach. Policymakers need to know not just whether each component works, but how they work in combination.

Factorial designs offer a method for testing these combinations systematically. They evaluate multiple factors at once, measuring both main effects --- the independent impact of each factor --- and interaction effects, where the effect of one factor depends on another. This approach moves beyond isolated trials, uncovering patterns that reflect the complexity of real-world programs.

To demonstrate this method, consider transportation employment policy. Suppose a policy analyst wants to understand how seasonal labor dynamics and economic cycles influence employment in the rail sector. Rather than testing these variables separately, a factorial design reveals whether they interact.

### Example: Rail Employment as a Case Study
We constructed a 2×2 factorial design using public data on U.S. rail transportation employment:

**Factor A: Season**

- Winter (December--February)
- Summer (June--August)

**Factor B: Economic Period**

- Pre-Recession (2007--2008)
- Post-Recession (2020--2021)

This yields four groups:

1.  [Winter during the Pre-Recession period]
2.  [Summer during the Pre-Recession period]
3.  [Winter during the Post-Recession period]
4.  [Summer during the Post-Recession period]

### What is a Factorial Design?
A factorial design tests every combination of two or more factors. This enables researchers to estimate:

- Main effects: How each factor influences the outcome independently
- Interaction effects: How the effect of one factor depends on the other

In our rail employment study:

- The main effect of Period tells us whether employment levels changed between economic eras.
- The main effect of Season examines whether summer or winter generally sees more employment.
- The interaction effect tells us whether seasonal patterns changed after the recession.

We use a 2² factorial ANOVA to analyze the effects.

```python
import pandas as pd
import numpy as np
from statsmodels.formula.api import ols
import statsmodels.api as sm
import seaborn as sns
import matplotlib.pyplot as plt

# Load the data (already preprocessed to include only relevant seasons and years)
url = "https://github.com/kylejones200/time_series/blob/main/rail_factorial_data.csv"
data = pd.read_csv(url, skiprows=1)
# Factorial ANOVA
model = ols('Employment ~ C(Season) * C(Period)', data=data).fit()
anova_table = sm.stats.anova_lm(model, typ=2)
print(anova_table)
# Interaction plot
sns.pointplot(data=data, x='Season', y='Employment', hue='Period', ci=None)
plt.title('Interaction Plot: Rail Employment')
plt.ylabel('Employment')
plt.savefig('rail_interaction_plot.png')
plt.show()
```

### Results


The interaction plot shows lines that are not parallel, suggesting that the relationship between season and employment may have shifted after the recession. This kind of result highlights the strength of factorial designs: they don't just answer *whether* something works, but *under what conditions* it works better or worse.


### Why It Matters
Most public policy interventions are multi-component and context-dependent. Testing components in isolation wastes time and misses the bigger picture. Factorial designs give policymakers a way to test combinations, identify synergies, and build more effective programs. When used with careful experimental control or quasi-experimental logic (e.g., comparing across time periods), they reveal not just *what* works but *how* and *when* it works.

This case shows how even with observational data, factorial logic can uncover complex patterns. For transportation policy, it could inform targeted seasonal hiring initiatives or resource allocations in economic downturns.

Factorial designs are an essential tool for policy analysis. They reduce the number of experiments needed, illuminate hidden interactions, and reflect the real complexity of government programs. By applying this framework to rail employment, we see not just changes over time, but changes in how time and season work together. This approach is scalable to many domains --- from labor policy to education and health --- wherever outcomes depend on the interplay of multiple levers.

``` 
###
# Prettier plot
###


# Plot again with fixed y-axis ticks
plt.figure(figsize=(6, 4))
plt.plot(plot_data.index, plot_data['Pre-Recession'], marker='o', color='black')
plt.plot(plot_data.index, plot_data['Post-Recession'], marker='o', color='gray')

# Add labels with horizontal padding
plt.annotate('Pre-Recession',
             xy=('Summer', plot_data['Pre-Recession'].loc['Summer']),
             xytext=(10, 0), textcoords='offset points',
             ha='left', va='center', fontsize=10, color='black')

plt.annotate('Post-Recession',
             xy=('Summer', plot_data['Post-Recession'].loc['Summer']),
             xytext=(10, 0), textcoords='offset points',
             ha='left', va='center', fontsize=10, color='gray')

# Minimalist styling
ax = plt.gca()
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['left'].set_position(('outward', 5))
ax.spines['bottom'].set_position(('outward', 5))

# Custom y-axis ticks
plt.yticks([100000, 150000, 200000], fontsize=9)
plt.xticks(fontsize=9)
plt.title('Rail Employment by Season and Economic Period', fontsize=11)
# Save and show
plt.tight_layout()
plt.savefig('minimalist_rail_interaction_plot_fixed_yticks.png', dpi=300)
plt.show()
```
