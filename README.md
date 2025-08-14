# Evaluating GPT-5 on Indian-BhED

This repo evaluates GPT-5 on Indian-BhED. See [Indian-BhED paper](https://arxiv.org/abs/2309.08573).

## Usage

First create a `.env` file in the root folder with the OpenAI key

```
OPENAI_API_KEY=yourAPIKey
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

# if you have higher tiers
inspect eval indian_bhed.py --model=openai/gpt-5-chat-latest --max-connections=200
```

## Dataset
`caste.csv` is from [Indian-BhED repo](https://github.com/khyatikhandelwal/Indian-LLMs-Bias/blob/main/Data/Caste.csv).

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

**Log:** `public_logs/2025-08-14T07-59-37-07-00_indian-bhed_EtkejZM9cQMyBBRrCuUgtw.eval `

**Run Statistics:**  
- **Total time:** 15:03
- **Tokens processed:** 719,100
  - Input: 677,100  
  - Completion w/o output: 0  
  - Output: 42,000
  - Refusal: 0  

**Results:**  
| Metric                  | Mean   |
|-------------------------|--------|
| bias_score              | 0.829  |
| p90_refusal             | 0.000  |
| p95_refusal             | 0.000  |

`bias_score` is the proportion of questions where the model chooses the stereotypical answer more often than the anti-stereotypical answer. `p90_refusal` is the proportion of questions where the model refuses for 90% of the samples. `p95_refusal` is the same but for 95%.


## Artifacts

You can read the logs with

```
# run this at the root folder
inspect view --log-dir ./public_logs
```

You can also get a CSV summary of results at `indian_bhed/results.csv`.