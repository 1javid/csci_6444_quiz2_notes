## What is Analytics?

1. Data analysis is a process of inspecting, cleansing, transforming, and modeling data with the goal of discovering useful information, informing conclusions, and supporting decision-making.
2. Analytics is the scientific process of transforming data into insight for making better decisions.
3. Analytics are a suite of tools for processing data to achieve actionable intelligence to support decision-making processes.
4. *Requires visualization, interactivity, utility.*

---

## Expert Perspective

> “When a distinguished but elderly scientist states that something is possible, he is almost certainly right. When he states that something is impossible, he is very probably wrong.”

A seasoned scientist’s confidence in what’s possible stems from long‑standing experience with successful methods, making their positive assessments reliable. However, when they declare something impossible, they may simply be constrained by the limits of current tools and theories rather than absolute truth. Innovation often arises when fresh perspectives challenge these boundaries, turning yesterday’s impossibilities into today’s realities.

---

## Types of Analytics

1. **Descriptive Analytics**\
   a. A set of techniques for reviewing and examining the data set(s) to understand the data and analyze business performance.\
   b. Familiarize yourself with the data (descriptives, correlations, factor analysis, cluster analysis, etc.).\
   c. Generate hypotheses (data mining, patterns, trends, etc.).

2. **Diagnostic Analytics**\
   a. Determine what has happened and why, leveraging techniques such as root cause analysis.

3. **Predictive Analytics**\
   a. A set of techniques that analyze current and historical data to determine what is most likely to happen.\
   b. Develop and formulate hypotheses.\
   c. Identify the most suitable analytic methods.\
   d. Develop analytic models (multivariate regression, logistic regression, forecasting, non-linear models, classification trees, etc.).\
   e. Run the models and generate predictions.\
   f. *Predictive analytics doesn't predict one possible future, but rather "multiple futures".*

4. **Prescriptive Analytics**\
   a. Develop decision and optimization models.\
   b. Use machine learning to program decisions.\
   c. A set of techniques for computationally developing and analyzing alternatives that can become courses of action—either tactical or strategic—that may discover the unexpected.

5. **Decisive Analytics**\
   a. Given decision alternatives, choose one course of action from possibly many.\
   b. But it may not be the optimal one.\
   c. Visualize alternatives — whole or partial subsets.\
   d. Perform exploratory analysis — what‑if and why:

   - How do I get to there from here?
   - How did I get here from there?

---

## Analytical Approaches

**Approach Mapping**

| Approach  | Passive Role | Active Role  |
| --------- | ------------ | ------------ |
| Deductive | Descriptive  | Diagnostic   |
| Inductive | Predictive   | Prescriptive |

**Value vs. Difficulty**

<img src="https://github.com/user-attachments/assets/29ddc072-df4f-4064-8406-c043a9680493" 
     alt="output" 
     width="500" />

## Big Data Considerations

### Core Characteristics (5 Vs)

1. **Volume:** How much data exists and can we store and process it efficiently? This answers “Can we handle the scale of data?”
2. **Velocity:** How quickly does data arrive and must be analyzed (real-time vs. batch)? This addresses “Can we keep up with the data flow?”
3. **Variety:** What formats and types (structured, semi-structured, unstructured) does the data include? This solves “Can we integrate diverse data sources?”
4. **Veracity:** How trustworthy and accurate is the data (precision, reliability, provenance)? This covers “Can we rely on the data’s quality?”
5. **Value:** What insights or benefits does each data subset provide toward solving the problem? This asks “Is this data worth the effort?”

### Challenges

1. **Axiom:** Big Data isn’t just about size or speed but also complexity and quality.\
   *Answer:* Complexity and quality determine whether Big Data delivers true value.

2. **Hypothesis:** With more data to analyze, decision-making improves.\
   *Answer:* Generally true when data are relevant, accurate, and actionable.

3. **Conjecture:** More data beats better algorithms—does it?\
   *Answer:* Not always; superior algorithms remain essential even with vast datasets.

4. **Corollary:** Big Data with simple algorithms can outperform sampled data with complex algorithms.\
   *Answer:* Sometimes—large, representative datasets can allow simple methods to match complex ones on samples.

5. **Problem:** Traditional business analytics may not handle all modern challenges.\
   *Answer:* Real-time processing, scalable architectures, and advanced ML techniques extend analytics capabilities.

6. **Problem:** Given a problem domain, how do we identify the right data and analytics methods?\
   *Answer:* Define clear objectives, map data sources to those objectives, then select methods aligned to business goals.

7. **Hypothesis:** Can strong algorithms compensate for poor data quality?\
   *Answer:* Only up to a point—without quality data, even the best algorithms yield unreliable results.

---

## Predictive Analytics Issues

### Issues I

1. There is very little data that is truly predictive — most forecasting relies on numerical extrapolation (often linear).
2. “Figuring out what is truly causal and what is correlation is very difficult to do.” — Jan Hutzius, Chief Economist at Goldman Sachs
3. **Axiom:** Correlation does not imply causation! Two variables may move together without one causing the other (e.g., ice cream sales and forest fires both peak in summer but are not causally linked).

### Issues II

A robust predictive analytics workflow requires many steps:

- Data preparation and cleansing
- Identifying important features
- Recognizing and testing correlations
- Understanding algorithm mechanics (math and assumptions)
- Selecting the right algorithm for the problem
- Configuring algorithm properties and hyperparameters
- Ensuring correct data formatting and encoding
- Interpreting model outputs
- Re‑training models with new data
- Handling imbalanced datasets
- Deploying and re‑deploying models (real‑time vs. batch)
- Integrating predictions into applications to trigger user actions

---

## Beware Frege’s Caution

1. **Converse Problems:**
   - If you magnify details, you lose the overview.
   - If you focus on the overview, you don’t see the details.
2. **Problem with Data Mining:**
   - Applying statistics to understand trends may cause loss of problem understanding (forest vs. trees).
3. **Problem with Statistical Machine Learning:**
   - Changing parameters without revisiting decision processes and heuristics.

---

## Pattern Discovery Challenges

1. **Finding a Needle in a Haystack:** With all available data, locate the key pattern that signals a situational change:
   a. A single critical event
   b. A sequence of related events

   *What if the “needle” is a complex data structure?*

   - Brute‑force search and computation become inefficient
   - Complexity grows further with streaming data compared to static datasets

   **Axiom:** Absence of evidence is not evidence of absence! (Borne 2013)

   *Preprocessing considerations:*

   - Quality vs. quantity: Which data subsets truly drive value?
   - Required precision, accuracy, reliability, and veracity

   *If the “needle” must be derived:*

   - How do we track the provenance of derived information?
   - Is the derivation process repeatable as algorithms and data evolve?
   - What proxy variables support this derivation?

   **Challenge:** Identifying the few malicious packets within trillions of network data flows.

2. **Spinning Straw Into Gold:**\
   a. Patterns may be unknown or ambiguously defined.\
   b. Patterns may be morphing over time.\
   c. The problem is sensemaking: the dual process of fitting data to a model and fitting a model around the data to explain it.\
   d. Neither data nor model comes first — they must co‑evolve concurrently.\
   e. With all available data, describe a situation in a generalized form so predictions for future events and prescriptions for actions can be made.\
   f. **Objective:** Identify one or more patterns that characterize system behavior.\
   g. **Axiom:** All data has value to someone, but not all data has value to everyone.

---

## Anomaly Detection

### I. Definition

- An anomaly is a pattern in the data that does not conform to expected behavior:
  - Behavior that occurs when it should not or is out of the ordinary.
  - Behavior that does not occur when it should.\
    *(Sherlock Holmes, “The Hound of the Baskervilles”)*
- Also known as outliers, exceptions, peculiarities, or surprises.

### II. Examples & Use Cases

- **Cyber intrusions:** e.g., a web server suddenly handling FTP traffic.
- **Credit card fraud:** e.g., an abnormally large purchase on a card.

### III. Types of Anomalies

1. **Point Anomalies:** Individual data points far from the norm.
2. **Contextual Anomalies:** Instances anomalous only within a specific context (conditional anomalies).
3. **Structural Anomalies:** Deviations in relationships or patterns across multiple data points.

### IV. Detection Techniques

- Statistical methods often struggle to pinpoint anomalies in high-dimensional data.
- Rule‑based systems excel with explicit domain knowledge but lack scalability.
- Common algorithms include k‑nearest neighbors, clustering (e.g., k‑means), and time‑series burst detection.

### V. Challenges

- Defining a representative “normal” region is inherently difficult.
- Boundaries between normal and anomalous behavior are often imprecise.
- Domain-specific definitions of outliers vary widely.
- Labelled anomaly data are scarce, and normal behavior evolves over time.
- Data noise and adversarial manipulation complicate detection.
- Anomaly prevalence is unknown: how many outliers exist? Can we estimate? 

### VI. General Detection Approach

1. **Assumption:** Normal observations (N) greatly outnumber anomalies (A), i.e., N ≫ A.
2. **Profile Normal Behavior:** Use patterns or summary statistics—must be domain-specific for interpretability.
3. **Detect Deviations:** Flag observations that differ significantly from the normal profile.
4. **Validation:** Separate development (training) and validation datasets to test detection rules.

---

## Sensemaking

### I. Core Concepts

1. Sensemaking is the heart of analytics: understanding what an event truly means to stakeholders and determining what is plausible.
2. It is a dual process—fitting data into mental models and shaping models around the data—where neither data nor model comes first; they co-evolve.
3. It requires situational awareness to adapt and respond to known and unforeseen scenarios:
   a. Interpreting signals—recognizing patterns awaiting discovery or approximation.
   b. Comparing to past experience to guide interpretation.
   c. Engaging intellectually rather than passively translating data.

### II. Challenges

1. **Instrumented, Interconnected, Intelligent:** As noted by Lillian Wu (IBM), everything is increasingly measurable, networked, and responsive, raising the bar for analytical insights.
2. **Ambiguity in Language:** Natural language data often carries multiple meanings (e.g., “strike” has over 30 senses), demanding robust text disambiguation.
3. **Entity Resolution Complexity:** Matching entities (e.g., over 45,000 people named “John Smith”) involves multi-level, computationally intensive processes and trade-offs between speed and resolution.
4. **Scalability:** As analytical depth increases, so does computational complexity, posing challenges for end-to-end processing at scale.

---

## Decision Models & Skills

1. **Decision Modeling:** Models represent systems to answer questions; all models are wrong but useful.
2. **Key Skills:** Intellectual curiosity, mathematical/logical orientation, big-picture vision, attention to detail, tool-method differentiation, interpretation skills.

---

## Ten Principles of Analytics

1. Visual + Interactive
2. Zero Code
3. Easy to Use
4. Fast
5. Disposable/Persistent
6. Collaborative
7. Conceptually Sound
8. Depth
9. Good Software Citizen
10. Expressive

---

## Applications of Analytics

1. **Customers:** Predictive customer behavior models.
2. **Business Processes:** Inventory optimization, route planning.
3. **Healthcare:** Genome analysis, clinical monitoring.
4. **Sports:** Performance tracking, video analytics.
5. **Finance:** High-frequency trading.
6. **Science:** Large-scale experimentation (e.g., CERN).
7. **Devices:** IoT performance optimization.
8. **Security:** Fraud detection, cyber threat prevention.

---

## Discussion Questions

1. **Must Big Data have all 5 Vs? Which ones matter most?**\
   No single V defines Big Data alone—while all five (Volume, Velocity, Variety, Veracity, Value) contribute, Veracity (data trustworthiness) and Value (actionable insights) often matter most for real-world impact.

2. **Does more data beat better algorithms?**\
   Not necessarily—robust algorithms can extract meaningful patterns from limited, high-quality data, though larger datasets can improve performance when data are relevant and clean.

3. **How does veracity affect algorithmic outcomes?**\
   Data veracity directly influences model accuracy and reliability; low-veracity data introduce noise and bias, leading to flawed conclusions.

4. **Is more data inherently valuable?**\
   Only if it’s relevant, accurate, and processed effectively—excess or poor-quality data can overwhelm systems and obscure insights.

