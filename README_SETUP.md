# Setup

This notebook reads credentials from environment variables. Set them before running:

```bash
export OPENAI_API_KEY="your-new-openai-key"
export HF_TOKEN="your-new-hf-token"
```

On Windows PowerShell:
```powershell
$env:OPENAI_API_KEY = "your-new-openai-key"
$env:HF_TOKEN = "your-new-hf-token"
```

Artifacts:
- paper_artifact.csv                  Cornelius, 900 rows
- qure_experiment_results.csv         QuRE, 900 rows
- paper_artifact_rubric_supplement.csv GPT-5.2 rubric scores (Cornelius audit, n=500)
- requirements.lock.txt               pinned environment
