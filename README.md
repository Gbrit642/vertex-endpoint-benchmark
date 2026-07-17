# Vertex AI Online Prediction Benchmarking Suite for Gemma 4

This repository contains a guided Jupyter Notebook to load-test, benchmark, and evaluate different model sizes (Gemma 4 12B, 27B, 31B) and Google Cloud hardware options (e.g., L4, RTX PRO 6000, H100 GPUs) on Vertex AI Online Prediction Endpoints.

It is specifically tailored to analyze performance for **agentic, multi-turn workloads** (such as tool-calling loops).

---

## Key Features

1. **Multi-Turn Agentic Simulator ($N$-turns)**:
   * Simulates realistic agent conversation histories with dynamic, alternating message chains.
   * Dynamically tracks context growth and measures the exact latency differences between initial reasoning turns and final summary turns.
2. **Key Performance Indicators (KPIs)**:
   * **Time to First Token (TTFT)**: Tracked separately for Turn 1 (Initial prompt) and Turn $N$ (Large conversation history) to evaluate serving engine efficiency.
   * **Tokens Per Second (TPS)**: Output generation speed per request.
   * **Normalized Time Per Output Token (NTPOT)**: Aggregate latency normalized by the generated token count.
   * **Throughput (QPS)**: Aggregate query throughput under concurrent loads.
   * **Cost-Efficiency (Tokens per Dollar)**: Quantifies exactly how many output tokens each dollar of cloud spending yields.
3. **AI-Powered Recommendation Engine**:
   * Automatically serializes benchmark history and invokes **Gemini 3.5 Flash** to write a cloud infrastructure sizing report and cost recommendation.
4. **Automated Resource Cleanup**:
   * Programmatic checks to undeploy all models and delete active endpoints, preventing accidental ongoing billing charges.

---

## Setup & Prerequisites

### 1. Authentication & GCP Project Setup
Ensure your local environment or workbench is authenticated with Google Cloud using Application Default Credentials (ADC):

```bash
# Login to gcloud and set quota project
gcloud auth login
gcloud auth application-default login
gcloud auth application-default set-quota-project <your-project-id>

# Configure billing quota project
gcloud config set billing/quota_project <your-project-id>
```

### 2. Environment Configuration
Create a `.env` file in the root directory to store your project-specific variables:

```env
PROJECT_ID=your-gcp-project-id
REGION=us-central1
EXISTING_ENDPOINT_ID=optional-existing-endpoint-id
```

*(Note: The `.env` file is automatically ignored by Git to prevent checking in sensitive credentials).*

---

## Running the Benchmark

1. Open [find_ideal_machine_type_llm.ipynb](find_ideal_machine_type_llm.ipynb) in your Jupyter environment.
2. Run the **Install Dependencies** cell to install required packages (`google-genai`, `python-dotenv`, `google-cloud-aiplatform`, etc.).
3. Set your parameters (concurrency levels, total requests, model size, and machine cost).
4. Run the benchmark suite to generate performance plots and comparative tables.
5. Execute the **AI-Powered Performance Analysis** cell to get sizing recommendations.
6. Toggle `CLEANUP_ENDPOINT = True` in the final cell to safely shut down Vertex AI endpoints.
