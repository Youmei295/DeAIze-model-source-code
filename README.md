# DeAIze Automated Fine-Tuning Pipeline

Welcome to the **DeAIze** training infrastructure repository! 

## Purpose of this Project
This project was created as a personal learning exercise to familiarize myself with building a fully automated Continuous Integration and Continuous Deployment (CI/CD) pipeline for fine-tuning Large Language Models (LLMs). 

> **Note:** As this is my first project exploring MLOps and LLM fine-tuning, the codebase (including the training notebook and GitHub Actions workflows) was mostly **AI-generated**. It serves as a practical, working example of how to connect various AI platforms together.

## The Automated Workflow
This repository automates the entire process of training a model from a simple git push, without needing to manually spin up servers. 

```mermaid
graph TD
    A[Developer] -->|Git Push| B(GitHub Repository)
    B -->|Triggers| C{GitHub Actions}
    C -->|Kaggle API Push| D[Kaggle GPU Environment]
    D -->|1. Setup: Install Libs & Downgrade PyTorch| E(Training Process)
    D -->|2. Download Data| E
    E -->|3. Fine-Tune with LoRA| E
    E -->|4. Push Adapters| F[(Hugging Face Model Hub)]
```

Here is how the pipeline works:
1. **Trigger:** Whenever new code or training data (`dataset.json`) is pushed to the `main` branch on GitHub, a GitHub Actions workflow is triggered.
2. **Compute Allocation (Kaggle):** The GitHub Action uses the Kaggle API and `kernel-metadata.json` to automatically push the code to Kaggle and request a backend GPU instance (Tesla P100).
3. **Fine-Tuning:** The Kaggle container runs `train.ipynb`, which loads the base model (`Qwen/Qwen2.5-3B-Instruct`), applies LoRA (Low-Rank Adaptation) via the `peft` library, and trains the model in FP16 precision using the `trl` library's `SFTTrainer`.
4. **Deployment (Hugging Face):** Once training completes, the notebook automatically uploads the finalized LoRA adapter weights directly to the Hugging Face Model Hub.

## Links & Resources
* **Kaggle Notebook / Execution Logs:** [deaize-training-infrastructure](https://www.kaggle.com/code/youmei1/deaize-training-infrastructure)
* **Hugging Face Model (Adapters):** [Youmei295/deAIze](https://huggingface.co/Youmei295/deAIze)
