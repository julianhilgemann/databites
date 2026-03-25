

## 1. The "Save Me" Protocols (Crisis Management)

### Scenario A: You blank on Syntax

*You forget the arguments for `SUMMARIZE` or `Window Functions`.*

**The Move:** **Pseudo-Code & Architecture.**

> "I'm blanking on the exact argument order, but the logic is: I need to generate a virtual table at the Customer Grain, calculate the ranking, and then filter the top 10. In my workflow, I usually define this structural logic first and use the IDE (or AI) to handle the syntax boilerplate."

### Scenario B: You face a "Hard" Technical Question

*They ask about a specific edge case in Fabric or complex [[DAX]] you haven't memorized.*

**The Move:** **The "Research" Pivot.**
> "I haven't encountered that specific edge case in my recent builds. However, my standard procedure for blockers is to consult the **Microsoft Fabric documentation** or **SQLBI** for the pattern. I don't guess; I verify the best practice to ensure I'm not breaking [[Query Folding|query folding]] or the calculation engine."

### Scenario C: The "AI / Copilot" Question

*They ask how you code or notice you rely on tools.*

**The Move:** **The Force Multiplier.**

> "I use AI as a **Strategic Force Multiplier**. It allows me to operate as a 'Full Stack' engineer.
> *   I use it to generate the **[[dbt]] YAML** and boilerplate [[SQL]] so I can focus on the **Data Integrity**.
> *   I use it to draft complex **[[DAX Patterns|DAX patterns]]**, which I then validate against the Evaluation [[Context]].
> *   It doesn't replace my knowledge; it accelerates my **Velocity**. It’s how I delivered 10+ Enterprise reports in 2 years while handling everything from Requirements to RLS."

---

## 2. Modern Engineering & Governance (The Senior Check)

*   **Source Control:** "I use **PBIP** (Power BI Projects) integrated with **Azure DevOps/[[git|Git]]**. We use feature branches for new measures and Pull Requests for code review."
*   **CI/CD:** "I believe in 'Deployment Pipelines.' We never publish directly to Production. Changes move from Dev $\to$ Test $\to$ App."
*   **Golden Datasets:** "I separate the **Data Workspace** (Semantic Models) from the **Reporting Workspace** (Thin Reports). This protects the 'One Version of the Truth' while allowing analysts to build their own visuals."

---

## 3. The "Closer" Questions

*Ask these to show you are looking at the big picture.*

1.  "Are we currently using **[[TMDL]]** and **Git Integration** for the models, or are we still managing binary `.pbix` files?"
2.  "How mature is the **Fabric** adoption? Are we looking to utilize **Direct Lake** to reduce the refresh latency for the operations team?"
3.  "Is there an established **Design System** or JSON Theme file, or will I be helping to define the visual identity for the reporting suite?"