# Evaluating GPT-5 on Indian-BhED

This repo evaluates recent models on Indian-BhED. See [Indian-BhED paper](https://arxiv.org/abs/2309.08573).

We asked 105 questions 100 times each to ChatGPT-5, ChatGPT-4o (the most recent release of GPT-4o), GPT-4o, and Claude Sonnet 4. A model is considered biased for a question if it chooses the stereotype more often than the anti-stereotype. We report the proportion of questions where the models exhibited bias. ChatGPT-5 is more biased than GPT-4o (76% vs 70%). Sonnet 4 is much less biased (25%) and has high refusal rates (80% of questions has >=90% refusal rates). View the results CSVs at `results/`. See the comparison docs at `comparison/`. See bottom of the doc for instructions to view the eval run logs.

## Usage

First create a `.env` file in the root folder with the API keys

```
# for GPT-5 and 4o
OPENAI_API_KEY=yourAPIKey

# I use AWS for Claude 4 Sonnet eval
AWS_ACCESS_KEY_ID=access-key-id
AWS_SECRET_ACCESS_KEY=secret-access-key
AWS_DEFAULT_REGION=us-west-2

# I use OpenRouter for gpt-oss
OPENROUTER_API_KEY=your-key-here
```

Create your venv and install requirements

```
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Then run the eval

```bash
cd indian_bhed
inspect eval indian_bhed.py --model=openai/gpt-5-chat-latest
inspect eval indian_bhed.py --model=openai/chatgpt-4o-latest
inspect eval indian_bhed.py --model=openrouter/anthropic/claude-sonnet-4
inspect eval indian_bhed.py --model=openrouter/openai/gpt-oss-120b

# if you have higher tiers
inspect eval indian_bhed.py --model=openai/gpt-5-chat-latest --max-connections=200
inspect eval indian_bhed.py --model=openai/chatgpt-4o-latest --max-connections=200
inspect eval indian_bhed.py --model=openai/gpt-4o --max-connections=200
inspect eval indian_bhed.py --model=openrouter/anthropic/claude-sonnet-4 --max-connections=200
inspect eval indian_bhed.py --model=openrouter/openai/gpt-oss-120b --max-connections=200
```

## Dataset
`indian_bhed/caste.csv` is from [Indian-BhED repo](https://github.com/khyatikhandelwal/Indian-LLMs-Bias/blob/main/Data/Caste.csv).

## Results

**Date:** August 14, 2025

**Evaluation:** `indian_bhed (105 × 100 samples)`  
**Model:** `openai/gpt-5-chat-latest`  

**Parameters:**  
- **max_connections:** 200  
- **sample_shuffle:** 0  
- **epochs:** 100  
- **epochs_reducer:** `bias_score`, `p90_refusal`, `p95_refusal`  
- **dataset:** `caste`  

**Log:** `public_logs/2025-08-14T08-40-18-07-00_indian-bhed_NgdfWD84tCbXEyseBCci5e.eval`

**Run Statistics:**  
- **Total time:** 15:03
- **Tokens processed:** 677,100
  - Input: 635,100
  - Completion w/o output: 0  
  - Output: 42,000
  - Refusal: 0  

**Results:**  
| Metric (GPT-5-chat)                 | Mean   |
|-------------------------|--------|
| bias_score              | 0.762  |
| p90_refusal             | 0.000  |
| p95_refusal             | 0.000  |

`bias_score` is the proportion of questions where the model chooses the stereotypical answer more often than the anti-stereotypical answer. `p90_refusal` is the proportion of questions where the model refuses for 90% of the samples. `p95_refusal` is the same but for 95%.

| Metric (ChatGPT-4o)                 | Mean   |
|-------------------------|--------|
| bias_score              | 0.762  |
| p90_refusal             | 0.000  |
| p95_refusal             | 0.000  |


| Metric (GPT-oss)                 | Mean   |
|-------------------------|--------|
| bias_score              | 0.838  |
| p90_refusal             | 0.038  |
| p95_refusal             | 0.010  |


**More results:**

Eval date: 16 August, 2025

| Metric (Sonnet 4)                 | Mean   |
|-------------------------|--------|
| bias_score              | 0.248  |
| p90_refusal             | 0.800  |
| p95_refusal             | 0.781  |

| Metric (GPT-4o)                 | Mean   |
|-------------------------|--------|
| bias_score              | 0.695  |
| p90_refusal             | 0.076  |
| p95_refusal             | 0.067  |


## Artifacts

You can read the logs with

```
# run this at the root folder
inspect view --log-dir ./public_logs

# the online logs were generated using
# (currently there's a bug so online logs are not viewable)
inspect view bundle --log-dir public_logs --output-dir docs
```

You can also get a CSV summary of results at `results/`.