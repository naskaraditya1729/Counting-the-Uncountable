# Counting the Uncountable
## BJKST and Distinct-Element Estimation in Massive Data Streams

> A mathematical and experimental study of randomized streaming algorithms for estimating the number of distinct elements in massive data streams.

---

## 1. Overview

How many **distinct elements** occur in a data stream?

For a small dataset, this is trivial:

`Stream → HashSet → |HashSet|`

But what happens when the stream contains millions, billions, or even trillions of observations?

Storing every distinct element becomes prohibitively expensive.

This project studies the **BJKST algorithm**, a randomized streaming algorithm designed to estimate the number of distinct elements while using substantially less memory than exact counting.

Our objective is not merely to implement BJKST. We aim to:

- understand its mathematical foundations;
- derive the estimator and its accuracy guarantees;
- implement the algorithm from first principles;
- experimentally validate its theoretical behaviour;
- study its memory–accuracy–runtime trade-offs;
- test its behaviour under different stream distributions;
- compare it against exact counting and other approximate approaches;
- build an interactive demonstration of the algorithm.

---

# 2. Core Research Question

> **Can we accurately estimate the number of distinct elements in a massive data stream while storing only a tiny fraction of the stream itself?**

Given a stream

x₁, x₂, ..., xₙ,

we want to estimate

F₀ = |{xᵢ : 1 ≤ i ≤ n}|,

without explicitly storing every distinct element.

---

# 3. Why BJKST?

BJKST is interesting because it sits at the intersection of:

- Probability
- Randomized algorithms
- Hashing
- Data structures
- Approximation algorithms
- Streaming computation
- Large-scale data processing

The central question is:

> **What information can we throw away while still retaining enough information to estimate a global property of an enormous dataset?**

This makes BJKST an excellent case study in algorithmic efficiency.

---

# 4. Project Philosophy

We do **not** want this project to become:

> "We implemented an algorithm and showed that it works."

Instead, the project should answer:

1. Why is exact distinct counting expensive?
2. What information does BJKST retain?
3. Why does that information allow us to estimate F₀?
4. What does probability say about the estimator?
5. Does the implementation actually exhibit the theoretical behaviour?
6. How do memory, accuracy and runtime interact?
7. How does performance change under different stream distributions?
8. What are the algorithm's limitations?
9. Where does BJKST sit within the broader family of streaming algorithms?

---

# 5. Project Architecture

```text
                         ┌──────────────────────┐
                         │     Data Stream      │
                         └──────────┬───────────┘
                                    │
                 ┌──────────────────┴──────────────────┐
                 │                                     │
                 ▼                                     ▼
        ┌─────────────────┐                  ┌─────────────────┐
        │ Exact Counter   │                  │ BJKST Sketch    │
        │    HashSet      │                  │ Randomized      │
        └────────┬────────┘                  └────────┬────────┘
                 │                                    │
                 ▼                                    ▼
        Exact Distinct Count                  Estimated Count
                 │                                    │
                 └────────────────┬───────────────────┘
                                  ▼
                         ┌─────────────────┐
                         │ Experimental    │
                         │ Evaluation      │
                         └────────┬────────┘
                                  │
                                  ▼
                    Accuracy / Memory / Runtime
                                  │
                                  ▼
                         Visualisation /
                           Dashboard
```

---

# 6. Main Work Packages

## WP1 — Mathematical Foundations

Topics:

- Streaming model
- Distinct-element problem
- Frequency moments
- Hash functions
- Pairwise independence
- Randomized sampling
- BJKST estimator
- Probability of retention
- Accuracy analysis
- Space complexity

### Deliverable

A mathematically rigorous explanation of BJKST, including the reasoning behind its estimator and theoretical guarantees.

---

## WP2 — Core Implementation

Implement BJKST **from first principles**.

The implementation should expose parameters such as:

- stream length;
- universe size;
- target error;
- random seed;
- hash-function configuration.

The implementation must process the stream sequentially rather than relying on the entire dataset being available in memory.

### Deliverable

A clean, documented implementation with unit tests.

---

## WP3 — Experimental Framework

Construct a reproducible framework for generating and processing streams.

Experiments should include:

### A. Uniform streams

Elements sampled approximately uniformly.

### B. High-duplication streams

A small number of elements occur repeatedly.

### C. High-cardinality streams

A large fraction of observations are distinct.

### D. Skewed distributions

Popular elements occur disproportionately often.

### E. Increasing stream size

For example:

```text
10⁴
10⁵
10⁶
10⁷
...
```

where computationally feasible.

### Deliverable

A reproducible experiment pipeline producing raw results and summary statistics.

---

## WP4 — Comparative Analysis

At minimum compare:

**Exact baseline**

`HashSet`

against

**Approximate method**

`BJKST`

Potential extension:

`HyperLogLog`

Comparison metrics:

- Accuracy
- Relative error
- Memory consumption
- Runtime
- Scalability

### Deliverable

Tables and plots demonstrating the trade-offs between the methods.

---

## WP5 — Visualisation & Demonstration

Build an interactive demonstration showing the algorithm operating on a stream.

Possible interface:

```text
Stream length:        10,000,000
True distinct count:  2,847,193

BJKST estimate:       2,831,904
Relative error:           0.54%

Memory
────────────────────────
Exact HashSet       ████████████████████
BJKST               ██
```

Possible controls:

- Stream size
- Number of unique elements
- Distribution
- Error parameter
- Random seed

Possible visualisations:

- Estimated vs actual cardinality
- Relative error vs stream size
- Memory vs accuracy
- Runtime vs stream size
- Distribution of estimation errors

---

# 7. Experimental Questions

The project should explicitly investigate:

### Q1. Accuracy

How close is the BJKST estimate to the true value?

Relative error:

`|estimate − true| / true`

### Q2. Memory

How does BJKST's memory usage scale as the stream grows?

### Q3. Runtime

How expensive is processing each incoming element?

### Q4. Accuracy–Memory Trade-off

What happens when we demand higher accuracy?

### Q5. Distribution Sensitivity

Does the underlying distribution of the stream affect empirical performance?

### Q6. Randomness

How much variation occurs between independent runs?

### Q7. Hashing

How sensitive is the implementation to the choice and quality of hash function?

---

# 8. Experimental Protocol

Experiments must be **reproducible**.

Every experiment should record:

```text
Random seed
Stream length
Universe size
True cardinality
Algorithm
Parameters
Runtime
Peak memory
Estimate
Absolute error
Relative error
```

Results should be saved rather than manually copied into the report.

---

# 9. Team Structure

There are **4 members** in the project.

Each member owns a primary work package, but everyone is expected to understand the complete project.

## Member 1 — Mathematical Lead

Responsibilities:

- Literature review
- Mathematical formulation
- BJKST derivation
- Probability analysis
- Theoretical guarantees
- Mathematical section of report

---

## Member 2 — Algorithm & Software Lead

Responsibilities:

- Core BJKST implementation
- Hashing
- Data structures
- Unit testing
- Performance optimisation
- Code quality

---

## Member 3 — Experimental Lead

Responsibilities:

- Stream generators
- Exact baseline
- Benchmarking
- Statistical analysis
- Reproducibility

---

## Member 4 — Visualisation & Presentation Lead

Responsibilities:

- Visualisation pipeline
- Dashboard/demo
- Figures
- Presentation design
- Results communication
- Final demonstration

---

## Important

These roles represent **ownership**, not isolation.

Every member must be able to explain:

- the problem;
- the intuition behind BJKST;
- the main algorithm;
- the experimental results;
- their own contribution.

---

# 10. Contribution Policy

Every member's contribution will be transparently documented.

Contribution will be evaluated across:

- Mathematical work
- Coding
- Experiments
- Documentation
- Visualisation
- Presentation
- Literature review
- Project coordination

The final repository will contain a contribution record.

### Expected standard

Each member should:

- complete assigned tasks;
- communicate blockers early;
- review relevant pull requests;
- contribute to the final presentation;
- understand the work they present.

If a member does not complete their assigned contribution, this will be **clearly reflected in the final contribution record**.

The goal is transparency, not policing.

---

# 11. GitHub Workflow

We will use a lightweight Git workflow.

### Branches

```text
main
│
├── theory/...
├── implementation/...
├── experiments/...
└── visualization/...
```

Avoid directly pushing unfinished work to `main`.

## Commit Guidelines

Use meaningful commits.

Good:

```text
feat: implement BJKST sketch
fix: correct hash threshold update
exp: add skewed stream benchmark
docs: derive estimator
viz: add error comparison plot
test: add cardinality estimation tests
```

Avoid:

```text
update
stuff
changes
final
final2
final_final
```

---

# 12. Pull Requests

Every substantial contribution should preferably go through a Pull Request.

A PR should contain:

```text
What changed?

Why was it needed?

How was it tested?

What remains to be done?
```

At least one other member should review substantial changes before merging.

---

# 13. Issue Tracking

Use GitHub Issues for:

- bugs;
- mathematical questions;
- implementation tasks;
- experiment ideas;
- literature;
- presentation tasks.

Suggested labels:

```text
theory
implementation
experiment
visualization
documentation
bug
research
presentation
```

---

# 14. Repository Structure

```text
.
├── README.md
│
├── docs/
│   ├── theory.md
│   ├── literature.md
│   ├── methodology.md
│   └── contribution.md
│
├── src/
│   ├── bjkst/
│   ├── baselines/
│   └── hashing/
│
├── experiments/
│   ├── generators/
│   ├── benchmarks/
│   └── configs/
│
├── results/
│   ├── raw/
│   ├── processed/
│   └── figures/
│
├── tests/
│
├── dashboard/
│
├── notebooks/
│
├── report/
│
└── presentation/
```

---

# 15. Development Roadmap

## Phase 0 — Orientation

- Read the original BJKST paper
- Identify the exact algorithm variant
- Understand the streaming model
- Establish mathematical notation

**Goal:** Everyone can explain BJKST without referring to notes.

---

## Phase 1 — Mathematical Reconstruction

- Derive the estimator
- Understand the sampling mechanism
- Study the role of hashing
- Establish error intuition
- Record theoretical assumptions

**Deliverable:** `docs/theory.md`

---

## Phase 2 — Minimal Implementation

Implement the simplest correct version.

Before optimisation:

> **Correctness > speed**

Create tests against small streams where the exact answer is known.

---

## Phase 3 — Experimental Framework

Build:

- stream generators;
- exact counter;
- benchmark framework;
- metrics;
- reproducible configurations.

---

## Phase 4 — Large Experiments

Run controlled experiments varying:

- stream length;
- cardinality;
- duplication;
- distribution;
- error parameter;
- random seed.

---

## Phase 5 — Comparative Study

Compare:

```text
Exact HashSet
      vs
BJKST
      vs
(optional) HyperLogLog
```

---

## Phase 6 — Interactive Demonstration

Build the visual interface.

The demo should make the algorithm understandable even to someone who has never seen BJKST.

---

## Phase 7 — Final Analysis

Answer:

- Does empirical behaviour match theory?
- Where does it perform best?
- Where does it struggle?
- What are the memory savings?
- What accuracy is achieved?
- What assumptions matter?

---

## Phase 8 — Report & Presentation

Finalise:

- report;
- figures;
- mathematical derivations;
- experimental tables;
- live demo;
- presentation;
- contribution record.

---

# 16. Stretch Goals

These are **not required for the core project**.

If the core implementation is solid, we can explore:

### Stretch Goal 1

Implement HyperLogLog and perform a serious comparison.

### Stretch Goal 2

Investigate alternative hash functions.

### Stretch Goal 3

Study distributed streaming:

```text
Stream A ──┐
           ├──> sketches ──> combined estimate
Stream B ──┘
```

### Stretch Goal 4

Investigate adversarial or pathological streams.

### Stretch Goal 5

Study how empirical error behaves across repeated randomized runs.

### Stretch Goal 6

Optimise the implementation for very large streams.

---

# 17. What Counts as a Successful Project?

The project is successful if we can demonstrate all of the following:

- [ ] We understand the mathematical basis of BJKST.
- [ ] We can explain why exact counting is expensive.
- [ ] We implemented BJKST ourselves.
- [ ] The implementation is tested.
- [ ] The estimator behaves as expected.
- [ ] We measured memory usage.
- [ ] We measured runtime.
- [ ] We measured estimation error.
- [ ] We tested multiple stream distributions.
- [ ] We compared against an exact baseline.
- [ ] We produced reproducible experiments.
- [ ] We have meaningful visualisations.
- [ ] We have a working demonstration.
- [ ] Every team member can explain the complete project.
- [ ] Individual contributions are documented.

---

# 18. Final Deliverables

The final project should contain:

### 1. Source Code

Clean, documented implementation.

### 2. Mathematical Documentation

Derivation and explanation of BJKST.

### 3. Experimental Framework

Reproducible benchmark suite.

### 4. Results

Tables, plots and statistical analysis.

### 5. Interactive Demo

A visual demonstration of streaming distinct counting.

### 6. Technical Report

Complete methodology, theory, implementation and results.

### 7. Presentation

A concise narrative focused on:

> **Problem → Idea → Mathematics → Implementation → Evidence → Insight**

### 8. Contribution Record

Transparent record of individual work.

---

# 19. Definition of Done

A task is considered complete only when it is:

1. Implemented or written;
2. Tested/verified where applicable;
3. Documented;
4. Committed to GitHub;
5. Reviewed when substantial;
6. Integrated with the rest of the project.

> "Works on my machine" is not a definition of done.

---

# 20. Guiding Principle

> **Don't merely demonstrate that BJKST works. Demonstrate why it works, when it works, how well it works, what it costs, and what we learn from it.**

That is the standard we are aiming for.

---

## Team

| Member | Primary Role |
|---|---|
| Member 1 | Mathematical Theory |
| Member 2 | Algorithm & Software |
| Member 3 | Experiments & Analysis |
| Member 4 | Visualisation & Presentation |

---

## Status

🚧 **Project under development**
