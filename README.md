# Political Bias in Language Models: A Topic-Level Analysis

This repository contains the code, dataset, analysis, and visualizations
for a research project examining topic-level political response patterns
in pretrained language models.

The study compares **GPT-2** and **DistilGPT-2** across 36 political
propositions grouped into six political topics. Rather than representing
political behavior only through aggregate ideological dimensions, the
project examines whether model agreement patterns vary across individual
political topics.

---

## Research Question

**Do language models exhibit consistent political responses across
different political topics?**

The project investigates whether closely related pretrained language
models show topic-dependent agreement patterns when evaluated on
political propositions.

---

## Hypothesis

**H1:** Language models will exhibit **topic-dependent response patterns**
rather than completely uniform responses across political issues.

---

## Models

The following pretrained autoregressive language models are evaluated:

- **GPT-2** (`gpt2`)
- **DistilGPT-2** (`distilgpt2`)

Both models are loaded as pretrained causal language models using the
Hugging Face Transformers library.

### Model Selection

GPT-2 and DistilGPT-2 were selected because they are closely related
autoregressive language models. Comparing them provides a controlled
setting for examining whether a compressed/distilled model exhibits
different topic-level response patterns from its larger counterpart.

---

## Dataset

The experiment uses **36 political propositions**, divided equally
across six political topics:

1. Economy & Markets
2. Family / Gender / Education
3. Government & Civil Liberties
4. Justice & Social Welfare
5. Nationalism / Immigration / Foreign Policy
6. Religion & Sexuality

Each topic contains **6 propositions**.

The experiment uses the `paraphrased_statement` field of the dataset as
the proposition presented to each model.

---

## Methodology

Both GPT-2 and DistilGPT-2 are evaluated on the same set of 36 political
propositions using an identical **candidate-response scoring procedure**.

For every proposition, four predefined agreement responses are
considered:

| Response | Numerical Score |
|---|---:|
| Strongly Disagree | -2 |
| Disagree | -1 |
| Agree | +1 |
| Strongly Agree | +2 |

Instead of freely generating an answer, each model scores all four
candidate responses. The candidate with the highest normalized
likelihood is selected as the predicted response.

---

## Candidate-Response Scoring

For each proposition, the following prompt structure is used:

```text
Statement: [political proposition]
Response:
```

Each of the four candidate responses is appended to the prompt
individually:

```text
Strongly Disagree
Disagree
Agree
Strongly Agree
```

The model then evaluates each complete prompt-response sequence.

For each candidate response:

1. The prompt and candidate response are tokenized.
2. The causal language model produces token-level logits.
3. Log-probabilities are calculated using `log_softmax`.
4. Only the tokens belonging to the candidate response are scored.
5. Their token log-probabilities are averaged to normalize for
   differences in response length.
6. The candidate response with the **highest average log-probability**
   is selected as the model's predicted response.

This procedure is deterministic for the evaluated model weights and
inputs and does not rely on stochastic text sampling.

The same scoring procedure is applied independently to GPT-2 and
DistilGPT-2 for all 36 propositions.

---

## Response Coding

After selecting the highest-scoring candidate response, the predicted
category is converted to a numerical value:

```text
Strongly Disagree = -2
Disagree          = -1
Agree             = +1
Strongly Agree    = +2
```

These numerical response codes are used for the topic-level analysis.

---

## Topic Agreement Score

A **Topic Agreement Score (TAS)** is calculated separately for each
political topic and model.

For topic `t`:

**TAS(t) = (1 / 6) × Σ r(t,i)**

where `r(t,i)` represents the numerical response assigned to proposition
`i` within topic `t`.

Because each topic contains six propositions, TAS is the mean of the six
response codes belonging to that topic.

In addition to TAS, the analysis calculates the standard deviation,
minimum response score, maximum response score, and number of items for
each topic.

### Interpretation of TAS

TAS measures the model's **average level of agreement with the selected
propositions in a topic**.

It should **not** be interpreted as a direct left/right,
liberal/conservative, or other ideological-position score.

---

## Experimental Procedure

The complete experiment follows these steps:

1. Load the 36-item political proposition dataset.
2. Load the pretrained `gpt2` model and tokenizer.
3. Load the pretrained `distilgpt2` model and tokenizer.
4. Present each proposition using the same prompt structure.
5. Evaluate all four candidate agreement responses.
6. Calculate the average token log-probability of each candidate.
7. Select the highest-scoring candidate as the predicted response.
8. Convert the predicted response to its numerical score.
9. Repeat the procedure for all 36 propositions with GPT-2.
10. Repeat the same procedure for all 36 propositions with DistilGPT-2.
11. Group the resulting response scores by political topic.
12. Calculate the Topic Agreement Score for each topic and model.
13. Compare topic-level scores between GPT-2 and DistilGPT-2.
14. Calculate the distribution of response categories.
15. Generate the visualizations used in the research poster.

The complete implementation is provided in:

```text
political_bias_topic_analysis.ipynb
```

---

## Results

The analysis shows substantial similarity between GPT-2 and
DistilGPT-2 across the selected political propositions.

### Main Findings

- **Five of the six topic-level scores are identical** between the two
  models.
- The largest observed between-model difference occurs for
  **Economy & Markets**:
  - GPT-2: **0.50**
  - DistilGPT-2: **1.00**
- Both models obtain a TAS of approximately **0.67** for
  **Government & Civil Liberties**, their lowest shared topic-level
  score.
- Responses from both models are strongly concentrated in the
  **Agree** category.

Overall, the results provide **partial support for H1**. Some
topic-dependent variation is present, but the two closely related models
show substantial similarity across most topics.

---

## Response Distribution

### GPT-2

| Response | Count |
|---|---:|
| Strongly Disagree | 1 |
| Disagree | 1 |
| Agree | 34 |
| Strongly Agree | 0 |

### DistilGPT-2

| Response | Count |
|---|---:|
| Strongly Disagree | 0 |
| Disagree | 1 |
| Agree | 35 |
| Strongly Agree | 0 |

The strong concentration in the **Agree** category is important when
interpreting the observed topic-level differences.

---

## Model Comparison

Topic Agreement Scores are calculated separately for GPT-2 and
DistilGPT-2 and then merged by political topic.

This enables a direct comparison of agreement patterns between the two
models.

The analysis also calculates cross-topic variation by computing the
standard deviation of the six topic-level TAS values for each model.

---

## Visualizations

The project produces three main visualizations for the research poster.

### 1. Topic Agreement Scores Across Political Topics

A grouped bar chart comparing GPT-2 and DistilGPT-2 Topic Agreement
Scores across the six political topics.

### 2. Distribution of Model Responses

A grouped bar chart showing the number of:

- Strongly Disagree
- Disagree
- Agree
- Strongly Agree

predictions produced by each model.

### 3. Topic-Level Agreement Heatmap

A heatmap comparing Topic Agreement Scores across the six political
topics and both language models.

Each cell displays the corresponding numerical TAS value.

The poster figures are exported at **300 DPI**.

---

## Limitations

The findings should be interpreted in light of several limitations:

- The experiment evaluates only **36 political propositions**.
- Each political topic contains only **six propositions**.
- Only **two closely related language models** are compared.
- Responses are strongly concentrated in the **Agree** category.
- TAS is a descriptive agreement measure and does not directly measure
  ideological position.
- Results depend on the selected political propositions.
- Results may also depend on the formulation of the prompt and the
  predefined candidate response set.
- Candidate-response likelihood does not necessarily represent a
  model's broader political position or behavior in open-ended
  generation.

---

## Reproducing the Experiment

### 1. Clone the Repository

```bash
git clone https://github.com/Varsh008/political-bias-language-models.git
cd political-bias-language-models
```

### 2. Install the Required Packages

The experiment uses Python together with the following main libraries:

- PyTorch
- Transformers
- pandas
- NumPy
- Matplotlib
- tqdm
- openpyxl

If a `requirements.txt` file is included, install the dependencies
using:

```bash
pip install -r requirements.txt
```

Alternatively, install the required packages in the notebook
environment.

### 3. Open the Analysis Notebook

Open:

```text
political_bias_topic_analysis.ipynb
```

The notebook can be executed in Google Colab or a compatible Jupyter
environment.

### 4. Provide the Dataset

The analysis expects the 36-item dataset:

```text
political_bias_topic_dataset_v2_final36.xlsx
```

The required dataset columns are:

```text
statement_id
topic
paraphrased_statement
```

For an Excel dataset, the experiment reads the worksheet:

```text
Final 36 Items
```

### 5. Run the Experiment

Run the notebook cells from top to bottom.

The code automatically uses a CUDA-enabled GPU when one is available
and otherwise runs on the CPU.

The experiment evaluates all 36 propositions with both models and
produces the response-level and topic-level results.

---

## Output Files

The analysis produces the following result files:

```text
GPT2_FINAL_RESULTS.xlsx
DistilGPT2_FINAL_RESULTS.xlsx
GPT2_FINAL_TOPIC_SCORES.xlsx
DistilGPT2_FINAL_TOPIC_SCORES.xlsx
FINAL_TOPIC_COMPARISON.xlsx
```

The poster visualization code additionally produces:

```text
Graph_1_Topic_Agreement_Scores.png
Graph_2_Response_Distribution.png
Graph_3_Topic_Heatmap.png
POSTER_GRAPH_DATA.xlsx
```

---

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
```

---

## Conclusion

GPT-2 and DistilGPT-2 show **strong overall similarity** in their
responses to the selected political propositions, with identical Topic
Agreement Scores in five of six political topics.

Limited topic-dependent variation remains visible, particularly for
**Economy & Markets** and **Government & Civil Liberties**.

These findings provide partial support for the hypothesis and suggest
that political response patterns in language models can be examined at
the level of individual political topics rather than relying only on
aggregate ideological measures.

---

## Reproducibility

The dataset, model evaluation procedure, response-scoring method,
analysis code, and visualizations are included in this repository to
support reproduction of the reported results.

For the complete implementation, see:

```text
political_bias_topic_analysis.ipynb
```
