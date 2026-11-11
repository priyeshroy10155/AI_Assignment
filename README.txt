AI Assignment 2026 - Classical Reasoning Agent
==============================================

Contents
--------
reasoning_agent.ipynb  Complete implementation (executed, with all outputs). Prints accuracy and inference
                       time per dataset, reasoning traces and the final results table, and writes submission.csv.
report.pdf             Representation, reasoning algorithms, experiments on the public data, reasoning traces.
submission.csv         Predictions, columns: id,label  (label = True / False).
README.txt             This file.

The knowledge base and query are read directly in logic (first-order logic in Unicode/ASCII/TPTP, Prolog
programs, RuleTaker triples) and decided by classical reasoning: forward chaining with unification and
stratified negation as failure, set-of-support resolution refutation with uniform-cost clause selection, and
model enumeration for causal (counterfactual) questions. No generative AI is used. Only the Python standard
library is used for reasoning; `datasets` and `huggingface_hub` are used only to download the public data.

Requirements
------------
Python 3.10+ (tested with 3.12.3 on Linux), plus:
    pip install datasets huggingface_hub jupyter nbconvert ipykernel

How to run (in a separate virtual environment)
----------------------------------------------
    python3 -m venv .venv
    source .venv/bin/activate          # Windows: .venv\Scripts\activate
    pip install datasets huggingface_hub jupyter nbconvert ipykernel
    jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=-1 reasoning_agent.ipynb
or open the notebook in Jupyter and choose Kernel -> Restart & Run All.

On the first run the public datasets are downloaded from Hugging Face (internet access is needed):
    RuleTaker     tasksource/ruletaker      (test split)
    CounterBench  CounterBench/CounterBench (data_balanced_alpha_V1.json, data_balanced_backdoor_V2.json)
    FOLIO         tasksource/folio          (validation split, expert premises-FOL / conclusion-FOL)
Offline copies can be placed in data/ (data/ruletaker_test.jsonl, data/counterbench.json,
data/folio_validation.jsonl).

Predicting the private test set
-------------------------------
In the first code cell set
    TEST_FILE = 'test.csv'        # .csv / .json / .jsonl with columns id, context, statement
and run all cells. submission.csv will then contain id,label for that file.
SEMANTICS can be 'auto' (default), 'closed' (negation as failure) or 'open' (classical FOL entailment).

Results of the included run (all public queries, SEMANTICS = 'auto')
--------------------------------------------------------------------
    Dataset        Accuracy    Queries   Inference time
    RuleTaker       91.73 %    151911        43.89 s
    CounterBench    87.92 %      1200         0.27 s
    FOLIO           83.74 %       203         2.62 s
    ALL (micro)     91.69 %    153314        46.78 s
    ALL (macro)     87.80 %
