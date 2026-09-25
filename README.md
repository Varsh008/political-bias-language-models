# political-bias-language-models

This repository contains the code, dataset, analysis, and visualizations for a research project examining topic-level political response patterns in pretrained language models.

The project compares **GPT-2** and **DistilGPT-2** across a set of political propositions grouped into six political topics.

## Research Question

**Do language models exhibit consistent political responses across different political topics?**

Rather than representing political behavior only through aggregate ideological dimensions, this project examines whether model responses vary across individual political topics.

## Models

The following pretrained autoregressive language models are evaluated:

- GPT-2
- DistilGPT-2

## Dataset

The experiment uses **36 political propositions**, divided equally across six political topics:

1. Economy & Markets
2. Family / Gender / Education
3. Government & Civil Liberties
4. Justice & Social Welfare
5. Nationalism / Immigration / Foreign Policy
6. Religion & Sexuality

Each topic contains **6 propositions**.

## Methodology

Each political proposition is evaluated using both language models.

Model responses are classified into four agreement categories:

| Response | Numerical Score |
|---|---:|
| Strongly Disagree | -2 |
| Disagree | -1 |
| Agree | +1 |
| Strongly Agree | +2 |

A **Topic Agreement Score (TAS)** is calculated for each political topic:

TAS(t) = (1 / 6) × Σ r(t,i)

where `r(t,i)` represents the numerical response assigned to proposition `i` within topic `t`.

TAS should be interpreted as an **agreement score for the selected propositions**, not as a direct left/right or liberal/conservative ideological score.

## Results

The analysis shows strong similarity between GPT-2 and DistilGPT-2.

- Five of the six topic-level scores are identical between the two models.
- The largest observed between-model difference occurs for **Economy & Markets**, where GPT-2 obtains a TAS of **0.50** and DistilGPT-2 obtains **1.00**.
- Both models obtain a TAS of approximately **0.67** for **Government & Civil Liberties**.
- Responses are strongly concentrated in the **Agree** category.

These results indicate limited topic-dependent variation while also showing substantial similarity between the two related language models.

## Visualizations

The project generates three main visualizations:

1. **Topic Agreement Scores Across Political Topics**
   - Compares GPT-2 and DistilGPT-2 TAS values across the six topics.

2. **Distribution of Model Responses**
   - Shows the number of Strongly Disagree, Disagree, Agree, and Strongly Agree predictions produced by each model.

3. **Topic-Level Agreement Heatmap**
   - Provides a compact comparison of topic-level agreement scores across both models.

## Repository Structure

```text
political-bias-language-models/
│
├── README.md
├── political_bias_topic_analysis.ipynb
│
├── data/
│   └── political_bias_topic_dataset_v2_final36.xlsx
│
└── graphs/
    ├── Graph_1_Topic_Agreement_Scores.png
    ├── Graph_2_Response_Distribution.png
    └── Graph_3_Topic_Heatmap.png
