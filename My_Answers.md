# My Answers - Assignment 2 (CFLP: Tabu Search vs Genetic Algorithm vs Gurobi)

**Student Information:**
- **Student ID:** 447540100
- **Student Name:** Atheer Jaber Alqarni
- **Gurobi Version Used:** [x] Academic [ ] Free
- **Date Completed:** 22/11/2026

> Use specific numbers from **YOUR** results (`results_student_<ID>.csv/.xlsx`) and reference the plots by name (e.g., "As shown in `gap_by_size.png`..."). Explain **why**, not only **what**.

---

## Section A: Solution Quality Analysis (4 marks)

### A1 (1 mark) - Overall Mean Optimality Gap

| | Tabu Search | Genetic Algorithm |
|---|---|---|
| Number of instances used (Gurobi optimal) | 144 | 144 |
| Mean gap (%) | 10.3930 | 11.2510 |

**Method with the lower overall gap:** [x] TS [ ] GA

**Comment:**

Across all 144 instances, Tabu Search achieved a mean reported gap of 10.3930%, compared with 11.2510% for the Genetic Algorithm. TS therefore had a lower average gap relative to the Gurobi reference solutions by 0.8580 percentage points. All instances were labelled "Optimal" in the results file; however, the supplied code uses this label for both OPTIMAL and TIME_LIMIT termination, so the recorded status alone does not verify optimality.

---

### A2 (1 mark) - Gap by Instance Size

| | small | Medium | Large |
|---|---|---|---|
| TS mean gap (%) | 8.7028 | 10.5341 | 11.9421 |
| GA mean gap (%) | 9.8012 | 11.2363 | 12.7157 |

**Does the gap increase with size?** TS: [x] Yes [ ] No
GA: [x] Yes [ ] No

**Comparison of trends (TS vs GA):**

As shown in `gap_by_size.png`, both methods showed increasing mean gaps from small to medium to large instances. TS achieved a lower mean gap than GA at every size. Each size group contained 48 instances, all with Gurobi reporting Optimal status.

The increasing gaps suggest that the default search settings were less effective at finding near-optimal solutions in the larger instance groups. However, these averages alone do not establish that size was the sole cause.

---

### A3 (1 mark) - Gap by Difficulty Level

| | Easy | Moderate | Expensive |
|---|---|---|---|
| TS mean gap (%) | 8.6369 | 13.2928 | 9.2494 |
| GA mean gap (%) | 8.9490 | 14.7982 | 10.0059 |

- Hardest difficulty for TS: Moderate
- Hardest difficulty for GA: Moderate

**Explanation:**

As shown in `gap_by_difficulty.png`, Moderate instances had the highest mean optimality gap for both methods: 13.2928% for TS and 14.7982% for GA. Expensive instances had lower gaps of 9.2494% and 10.0059%, respectively, while Easy instances had the lowest gaps. Each difficulty group contained 48 instances.

A possible explanation is that Moderate instances require a more difficult balance between facility-opening costs and client-assignment costs, making locally attractive choices less effective overall. This is an interpretation rather than a conclusion established by the averages alone. In this experiment, the Expensive label did not imply the highest relative gap.

---

### A4 (1 mark) - TS vs GA Head-to-Head (`ts_vs_ga_scatter.png`)

- Fraction of instances where TS has a lower gap than GA: 81/144 (56.25%)
- Fraction of instances where GA has a lower gap than TS: 3/144 (2.08%)

**Instance types where one method consistently dominates:**

Based on the CSV results, TS achieved a lower gap in 81 instances, GA in 3, and the methods tied in 60 (41.67%).

TS was never worse than GA on Easy and Moderate instances across all three sizes. On Moderate instances, TS won 32 of 48 cases and tied in 16. All three GA wins occurred on Expensive instances: two Small and one Large. Thus, TS generally performed better, but did not strictly outperform GA on every instance.

---

## Section B: Computational Efficiency Analysis (4 marks)

### B1 (1 mark) - Computation Time and TS Speedup

| Mean time (s) | Small | Medium | Large |
|---|---|---|---|
| Gurobi | 3.1306 | 2.3573 | 2.6966 |
| Tabu Search | 0.8885 | 0.9570 | 1.0424 |
| Genetic Algorithm | 0.7738 | 0.9866 | 1.0378 |

**TS speedup over Gurobi on large instances:** 6.8286x

**Comment:**

As shown in `computation_time.png` and `speedup_factor.png`, both metaheuristics had lower mean computation times than Gurobi across all three size groups. GA had a lower mean runtime than TS on Small and Large instances, while TS had a slightly lower mean runtime on Medium instances.

For the 48 Large instances, the mean per-instance TS speedup over Gurobi was 6.8286×, computed by averaging `ts_speedup_factor`. By comparison, dividing the mean Gurobi time by the mean TS time (`2.6966 / 1.0424`) gives 2.5869×. These two values differ because the mean of individual per-instance ratios is not equal to the ratio of group means.

---

### B2 (1 mark) - GA Speedup vs TS Speedup

- Mean GA speedup over Gurobi (all sizes): 3.2554x
- Mean TS speedup over Gurobi (all sizes): 4.8618x
- Faster metaheuristic and by how much: GA, based on overall mean runtime: 0.9327 s versus 0.9626 s for TS (approximately 1.0320× faster).

**Comment:**

Across all 144 instances, the mean per-instance speedup over Gurobi (illustrated in `speedup_factor.png`) was 3.2554× for GA and 4.8618× for TS.

However, GA achieved a lower overall mean runtime: 0.9327 s versus 0.9626 s for TS, meaning GA used approximately 3.11% less time on average (a difference of about 0.0299 s per instance).

These results are consistent because `*_speedup_factor` averages `gurobi_time / method_time` across individual instances, whereas overall mean runtime averages `method_time` directly. A higher mean speedup factor over Gurobi does not necessarily imply a lower overall mean runtime.

---

### B3 (1 mark) - Growth of Gurobi Time vs Metaheuristic Time

| | Small -> Medium (x) | Medium -> Large (x) |
|---|---|---|
| Gurobi time growth | 0.7530 | 1.1439 |
| TS time growth | 1.0771 | 1.0892 |
| GA time growth | 1.2750 | 1.0519 |

**Comparison:**

As shown in `computation_time.png`, Gurobi's mean runtime decreased from 3.1306 s on Small instances to 2.3573 s on Medium instances (growth factor 0.7530×), then increased to 2.6966 s on Large instances (growth factor 1.1439×, or +14.39%).

TS mean runtime increased by approximately 7.71% from Small to Medium (1.0771×) and by 8.92% from Medium to Large (1.0892×). GA mean runtime increased by approximately 27.50% from Small to Medium (1.2750×) and by 5.19% from Medium to Large (1.0519×).

Both metaheuristics showed steady increases in mean runtime across instance sizes, whereas Gurobi's mean time fluctuated depending on instance hardness and solver search behavior rather than increasing monotonically.

---

### B4 (1 mark) - Quality-Speed Trade-off

**Method with the best balance of low gap and low time:** [x] TS [ ] GA [ ] Gurobi

**Justification (with numbers):**

I would choose TS when seeking fast solutions with a relatively low optimality gap. Across all 144 instances, TS achieved a lower mean gap of 10.3930% compared with 11.2510% for GA (a reduction of 0.8580 percentage points), while their overall mean runtimes were very close: 0.9626 s for TS versus 0.9327 s for GA (only about 0.0299 s longer on average for TS).

Gurobi serves as the 0% reference benchmark across all 144 instances, with mean runtimes of 3.1306 s (Small), 2.3573 s (Medium), and 2.6966 s (Large). If exact optimality is required and slightly higher runtimes are acceptable, Gurobi is preferable; otherwise, TS provides a strong sub-second trade-off between gap and runtime.

---

## Section C: Scalability Analysis (3 marks)

### C1 (1 mark) - Gap Heatmap (`gap_heatmap.png`)

- Highest TS gap: (size, difficulty) = (Large, Moderate) with  14.5262%
- Highest GA gap: (size, difficulty) = (Large, Moderate) with 16.1783%

**Interpretation:**

As shown in `gap_heatmap.png`, the Large–Moderate group had the highest mean optimality gap among the nine size–difficulty combinations for both methods: 14.5262% for TS and 16.1783% for GA. Each combination contained 16 instances. This group was therefore the most challenging for achieving near-optimal solutions, as measured by mean gap. TS performed better than GA in this group, with a mean gap lower by 1.6522 percentage points.

---

### C2 (1 mark) - Expected Scalability to Very Large Instances

**Method expected to scale better:** [x] TS [ ] GA

**Justification (algorithmic complexity and your observed results):**

I expect TS to offer better solution quality as instances grow. On Large instances, TS achieved a lower mean gap than GA (11.9421% versus 12.7157%) with similar mean runtimes (1.0424 s versus 1.0378 s). TS evaluates candidate moves along one search trajectory, whereas GA evaluates multiple chromosomes per generation, including assignment and repair costs. However, both methods become more expensive as instance size increases. These results support a tentative preference for TS, not a guarantee for instances with 200 facilities and 500 clients.

---

### C3 (1 mark) - Validity of Gap Measurements When Gurobi Hits the Time Limit

- Instances labelled "Optimal": 144 out of 144. The actual number of instances without proven optimality cannot be determined from the saved status column, because the supplied code labels both OPTIMAL and TIME_LIMIT outcomes as "Optimal".

**Effect on the validity of the gaps:**

All rows were labelled Optimal, but the supplied code also assigns this label after TIME_LIMIT. Therefore, the status column alone cannot confirm optimality. If optimality is unproven, the reported gaps compare heuristic costs with Gurobi's incumbent, not necessarily the true optimum.


**Better benchmark:**

Use a proven optimal solution when available. Otherwise, report Gurobi's best lower bound and incumbent cost, and evaluate feasible heuristic solutions against the lower bound, clearly labelling the resulting gaps as bounds rather than exact optimality gaps.

---

## Section D: Quality-Time Tradeoff Analysis (3 marks)

### D1 (1 mark) - 10-Second Time Budget (Medium Instances)

**Recommended method:** [x] TS [ ] GA

- Mean TS time / gap on medium instances: 0.9570 s / 10.5341%
- Mean GA time / gap on medium instances: 0.9866 s / 11.2363%

**Justification:**

TS achieved a lower mean gap than GA with nearly identical mean runtime. All 48 Medium instances finished within 10 seconds for both methods maximum times were 1.8116 s for TS and 2.1512 s for GA. Therefore, TS is recommended for its better average solution quality.

---

### D2 (1 mark) - Quality Is the Top Priority

| Size | Recommended approach (TS / GA / Gurobi) | Justification (numbers) |
|---|---|---|
| Small | Gurobi | 0% gap vs TS: 8.7028% & GA: 9.8012%|
| Medium |Gurobi | 0% gap vs TS: 10.5341% & GA: 11.2363%|
| Large |Gurobi | 0% gap vs TS: 11.9421% & GA: 12.7157%|

---

### D3 (1 mark) - Convergence Timing (3-5 convergence plots, pushed in `sample_convergence_plots/`)

**Selected plots:** 097_cflp_large_easy_fac24_cli65_convergence.png, 098_cflp_large_easy_fac22_cli65_convergence.png, 099_cflp_large_easy_fac21_cli65_convergence.png, 100_cflp_large_easy_fac20_cli65_convergence.png, 101_cflp_large_easy_fac20_cli65_convergence.png.

| Instance | TS: iteration of best solution / total iterations | GA: generation of best solution / total generations |
|---|---|---|
| 097_cflp_large_easy_fac24_cli65.json | 6/46 (13.04%) | 19/49 (38.78%) |
| 098_cflp_large_easy_fac22_cli65.json | 6/46 (13.04%) | 30/60 (50.00%) |
| 099_cflp_large_easy_fac21_cli65.json | 5/45 (11.11%) | 38/68 (55.88%) |
| 100_cflp_large_easy_fac20_cli65.json | 25/65 (38.46%) | 50/80 (62.50%) |
| 101_cflp_large_easy_fac20_cli65.json | 26/66 (39.39%) | 9/39 (23.08%) |

**What this implies for early stopping:**

TS found its best solution within 11.11–39.39% of the executed iterations; GA within 23.08–62.50% of the executed generations. The existing early-stopping rules stopped TS after 40 iterations without improvement and GA after 30 generations. Shorter patience could save time, but may miss later improvements and should be tested.

---

## Section E: Algorithm Understanding (3 marks)

### E1 (1 mark) - Tabu Search: Tabu List and Aspiration Criterion

**How the tabu list prevents cycling:**

After a move, TS temporarily forbids its reverse. This prevents immediately undoing recent changes and reduces cycling between the same solutions.

**Role of the aspiration criterion:**

A tabu move is allowed if it produces a solution with a lower total cost than the best solution found so far.

**Concrete CFLP example** (a specific tabu move, and when aspiration would override it):

After opening facility F2, closing F2 becomes tabu for the tabu tenure. Suppose the best cost so far is 10,000. If closing F2 later produces a feasible solution costing 9,500, aspiration allows the move despite its tabu status.

---

### E2 (1 mark) - Genetic Algorithm: Repair Operator and Crossover

**How the repair operator ensures feasibility, and what would happen without it:**

The repair operator opens additional facilities, choosing the closed facility with the largest capacity, and reassigns clients until feasibility is achieved or no closed facilities remain. Without repair infeasible chromosomes may remain the fitness function assigns them infinite cost.

**How uniform crossover combines two parent CFLP solutions:**

Each gene indicates whether a facility is open (1) or closed (0). Uniform crossover independently selects each gene from either parent with equal probability. **For example** parents [1,0,1,0] and [0,1,0,1] may produce [1,1,0,0]. The child is then repaired if necessary.

---

### E3 (1 mark) - Trajectory-Based vs Population-Based Exploration

**Meaning for the exploration of the solution space:**

TS follows one current solution, exploring neighboring solutions through facility and assignment moves. GA maintains multiple solutions and explores different facility configurations through selection, crossover, and mutation.

| | Advantages for the CFLP | Disadvantages for the CFLP |
|---|---|---|
| Tabu Search (trajectory) |Directly improves facility decisions and client assignments tabu memory reduces cycling | May remain in one region performance depends on neighborhood and tenure settings |
| Genetic Algorithm (population) | Explores multiple facility configurations and combines useful patterns | Evaluating and repairing many chromosomes is costly diversity may be lost prematurely |

---

## Section F: Critical Reflection (3 marks)

### F1 (1 mark) - Most Important TS Hyperparameters

1. [tabu_tenure — Controls how long reverse moves remain tabu — Default 10 reduces cycling but may delay useful moves ]

2. [max_candidates — Limits candidates per move type — Default 20 limits computation but may miss better neighbors]

3. [max_no_improve — Controls stopping after stagnation — Default 40 saves time but may miss later improvements]

**What I would change and why:**

I would test increasing max_candidates from 20 to 40 to explore more neighbors. On Large instances, TS averaged 11.9421% gap and 1.0424 s runtime. More candidates might improve quality at additional computational cost; this requires testing.
---

### F2 (1 mark) - Most Important GA Hyperparameters

1. [population_size — Number of chromosomes maintained in each generation]

2. [max_generations — Maximum number of generations allowed]

3. [mutation_rate — Probability of flipping each gene, helping maintain diversity]

**Effect of increasing `population_size` / `max_generations` on quality and runtime:**

A larger population explores more solutions per generation but increases computation. More generations allow further improvements but may increase runtime. Neither guarantees better quality, and increasing max_generations may have no effect if early stopping occurs first.

---

### F3 (1 mark) - Proposed Algorithmic Improvements

**Improvement for Tabu Search:**

[**Description:** Add a diversification restart after stagnation |**Justification:** Perturbing facility decisions and repairing feasibility can help explore new regions. TS's Large-instance mean gap of 11.9421% motivates testing this approach ]

**Improvement for the Genetic Algorithm:**

[**Description:** Apply short TS refinement to the best chromosome each generation | **Justification:** This combines population exploration with local improvement. TS had a lower Large-instance mean gap than GA (11.9421% vs 12.7157%). supporting testing this hybrid although runtime would increase]

---

## BONUS A: Tabu Search Improvement (+5%, optional)

*Leave this section empty if you do not attempt the bonus. See Section 8 of the guide. Your improved notebook (`StudentID_FirstName_LastName_Assignment2_IMPROVED.ipynb`) must be pushed to your repository.*

**(a) Screening results (small/moderate subset):**

| Enhancement tested | Mean gap before (%) | Mean gap after (%) | Improvement |
|---|---|---|---|
| Adaptive tenure | 11.2458 | 11.1534 | 0.0924 |
| Aspiration+ criterion | 11.2458 | 11.1375 | 0.1082 |
| Frequency-based memory | 11.2458 | 11.0913 | 0.1545 |
| Diversification restart | 11.2458 | 11.2157 | 0.0301 |

**(b) Selected enhancement and justification:**

I selected **Frequency-based memory** because it achieved the largest screening gap reduction (**0.1545** percentage points, lowering the mean gap from **11.2458%** to **11.0913%**). By penalizing repeatedly selected moves and visited configurations over the search history, frequency-based long-term memory reduces cycling into stagnant areas and guides Tabu Search toward less-explored regions of the solution space.


**(c) Full 144-instance results:**

| Metric | Baseline TS (corrected) | Improved TS |
|---|---|---|
| Mean gap (%) | 10.6309 | 10.4374 |
| Mean runtime (s) | 1.6483 | 1.8182 |

**Improved results files (names in your repository):** bonusA_corrected_full_student_447540100_details.csv
bonusA_corrected_full_student_447540100_summary.csv
bonusA_corrected_full_student_447540100.xlsx

---

## BONUS B: Genetic Algorithm Improvement (+5%, optional)

*Leave this section empty if you do not attempt the bonus.*

**(a) Screening results (small/moderate subset):**

| Enhancement tested | Mean gap before (%) | Mean gap after (%) | Improvement |
|---|---|---|---|
| Adaptive mutation | 12.434893 | 12.447729 | -0.012836 |
| Two-point crossover | 12.434893 | 12.434893 | 0 |
| Memetic GA (TS + GA) | 12.434893 | 12.434893 | 0 |
| Diversity preservation | 12.434893 | 12.434893 | 0 |

**(b) Selected enhancement and justification:**

I selected **Two-point crossover**. It tied with Memetic GA and Diversity preservation for the lowest screening mean gap; none of these three reduced the mean gap relative to the baseline. Unlike uniform crossover—which exchanges individual bits independently and can disrupt contiguous facility patterns—two-point crossover preserves contiguous chromosome segments between two cut points while adding minimal computational overhead.

Runtime was used to break this tie. Two-point crossover had the lowest mean runtime among the tied enhancements: **26.735214 s**, compared with **51.215463 s** for Memetic GA and **67.615916 s** for Diversity preservation. The baseline runtime was slightly lower at **26.218777 s**.

For convergence, Two-point crossover first reached its final best solution at generation **4.6875** on average in the screening subset, compared with **6.8125** for the baseline. Thus, it reached its final best solution earlier in generation count, although its overall mean runtime was slightly higher.

**(c) Full 144-instance results:**

| Metric | Original GA | Improved GA |
|---|---|---|
| Mean gap (%) | 10.964484 | 10.950637  |
| Mean runtime (s) | 19.701676 | 19.792961 |

**Improved results files (names in your repository):**
bonusB_corrected_full_student_447540100_details.csv
bonusB_corrected_full_student_447540100_summary.csv
bonusB_corrected_full_student_447540100.xlsx

---

## Additional Comments (Optional)


---

## Declaration

I hereby declare that:
- I have completed this assignment independently
- All numerical results are from MY experiments using MY student ID
- All analysis and interpretations are MY own work
- I have not copied results or answers from other students
- I understand the consequences of academic dishonesty

**Signature:** ATHEER JABER

**Date:** 22/11/2026

**END OF MY ANSWERS**
