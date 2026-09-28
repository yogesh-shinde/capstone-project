# Clinical Report Assistant - Single-Notebook Upgrade
Everything the audit recommended is in **one file**: `clinical_report_assistant.ipynb`.
## Layout
```
capstone_project_notebook/
  clinical_report_assistant.ipynb   # <-- open this in Jupyter/VS Code
  build_notebook.py                 # regenerates the .ipynb from source
  requirements.txt
  .env.example
  README.md
```
## Quick start
```
# edit .env to set GOOGLE_API_KEY (and optionally LANGSMITH_API_KEY)
jupyter notebook clinical_report_assistant.ipynb
```
Put patient PDFs under `./data/clinical_input_scenario_pack/patient_inputs/` and guideline PDFs under `./data/clinical_input_scenario_pack/rag_guidelines/`.
## Notebook contents (in order)
1. Audit + upgrade plan (markdown) - every real bug and design flaw called out.
2. Install / imports / typed settings - `pydantic-settings`, no `getpass`.
3. Logging + LangSmith + PHI redaction via `structlog`.
4. Prompt-injection defense - sanitize, size-cap, delimit, policy suffix.
5. Two-tier LLM factory + cached embeddings (`gemini-2.5-flash` / `pro`).
6. Pydantic schemas + LangGraph state.
7. PDF parsers - native then OCR fallback, size/page caps.
8. Document router - deterministic tool + fast-tier ReAct agent.
9. Report analysis - structured extraction + deterministic fallback.
10. Lab analysis - LLM extraction + fixed table parser + reference-range tool.
11. RAG - persistent Chroma + BM25 + dense MMR hybrid retrieval.
12. Summary agent - smart-tier chain + deterministic Markdown fallback.
13. Recommendation + revision agents.
14. LangGraph - nodes, edges, HITL interrupt, durable `SqliteSaver`.
15. Streaming runner - token-level output via `stream_mode=["updates","messages"]`.
16. Demo run + doctor-review submission cells (approve / revise).
17. Observability - logs, errors, metrics, evidence, router table.
18. Ragas evaluation - faithfulness, answer_relevancy, context_precision, context_recall.
## Regenerating the notebook
Edit `build_notebook.py` and run:
```powershell
python build_notebook.py
```
## Bugs fixed vs. the original `capstone_project.txt`
- `class ReportAnalysisOutput` leading-space IndentationError.
- Lab table parser off-by-one that dropped the final row.
- 3x duplicated HITL block + syntax error at end of file.
- Non-existent model IDs (`gemini-3-flash-preview`, `gemini-embedding-2`).
- `getpass` at import time.
- FAISS index built at import time.
- Undefined `doctor_review_submission` in the resume cell.
- Retry decorators catching all exceptions.
- Prompt-injection surface via raw PDF interpolation.
- No PHI redaction on logs / traces.
 