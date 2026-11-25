# Grok ResearchBot

Grok ResearchBot is a Google Colab notebook that helps users explore
research-paper literature with a retrieval-augmented generation (RAG)
workflow. It extracts text from uploaded PDF papers, splits the text
into chunks, creates embeddings, retrieves relevant excerpts, identifies
potential research gaps, and proposes a technique grounded in the
retrieved literature.

> **Important:** This notebook uses the **Groq API** for language-model
> responses. Each user must provide their own Groq API key. The notebook
> does not include a shared API key.

## Features

-   Upload one or more research-paper PDFs in Google Colab.
-   Extract PDF text with PyMuPDF.
-   Split extracted text into overlapping chunks.
-   Generate embeddings using `sentence-transformers`
    (`all-MiniLM-L6-v2`).
-   Store and retrieve chunks with ChromaDB.
-   Use a Groq-hosted language model to identify research gaps and
    propose a technique based on retrieved excerpts.

## Requirements

-   A Google account and access to [Google
    Colab](https://colab.research.google.com/).
-   Your own Groq API key.
-   Research papers in PDF format.

No local Python installation is required if you run the notebook in
Google Colab.

## Setup and run

### 1. Get your own Groq API key

1.  Visit the [Groq Console](https://console.groq.com/).
2.  Sign in or create an account.
3.  Open the API Keys section: [Groq API
    Keys](https://console.groq.com/keys).
4.  Create a new API key and copy it.
5.  Keep the key private. Do not post it in a GitHub repository,
    notebook cell, screenshot, or public discussion.

Each reader must use their own key. API usage is subject to Groq account
limits, availability, and applicable pricing or usage policies.

### 2. Open the notebook in Google Colab

1.  Open [Google Colab](https://colab.research.google.com/).
2.  Select **File → Upload notebook**.
3.  Upload `GrokResearchbot.ipynb` from this repository.

Alternatively, open the notebook from GitHub using Colab's GitHub tab.

### 3. Add your API key to Colab Secrets

1.  In the Colab left sidebar, click the **key icon** (Secrets).

2.  Click **Add new secret**.

3.  Set the secret name to exactly:

    `GROQ_API_KEY`

4.  Paste your own Groq API key as the value.

5.  Enable notebook access for this secret using the toggle beside it.

The notebook reads the key using:

``` python
from google.colab import userdata
import os

os.environ["GROQ_API_KEY"] = userdata.get("GROQ_API_KEY")
```

**Do not replace this with a key written directly in the notebook.**
Colab Secrets keeps the key out of the notebook source and GitHub
repository.

### 4. Run the notebook

1.  Run the dependency-installation cell and wait for installation to
    finish.
2.  Run the import and configuration cell. If you see an authentication
    error, check that the secret is named `GROQ_API_KEY` and notebook
    access is enabled.
3.  Run the PDF upload cell and select your research-paper PDFs.
4.  Run the remaining cells in order.
5.  Enter or edit the research question in the pipeline cell, then run
    it to retrieve literature, identify research gaps, and generate a
    proposed technique.

Run the notebook cells from top to bottom in a fresh Colab runtime.

## Configuration

The current notebook calls the Groq API with the model identifier:

``` python
model="openai/gpt-oss-120b"
```

If this model is unavailable to your Groq account or has been
retired/renamed, check Groq's current model documentation and replace
the identifier with a model currently available to your account.

The retrieval step currently requests the top 5 matching chunks:

``` python
context, sources = retrieve_context(research_query, top_k=5)
```

You can adjust `top_k` to retrieve a different number of chunks.

## Important notes and limitations

-   **Your own API key:** Every user is responsible for obtaining and
    protecting their own Groq API key.
-   **API costs and limits:** Requests use the user's own Groq account
    and are subject to its rate limits, quotas, and pricing.
-   **Colab runtime:** This notebook imports `google.colab` utilities
    and is designed to run in Google Colab. It is not a ready-to-run
    standalone local Python application.
-   **Temporary vector database:** The notebook creates an in-memory
    ChromaDB client. Its indexed papers are not saved permanently and
    will be lost when the Colab runtime is reset or disconnected.
    Re-upload and reprocess the PDFs in a new session.
-   **PDF quality:** Scanned PDFs may not yield usable text because the
    current notebook extracts embedded text and does not include OCR.
-   **Research validation:** Generated research gaps and technique
    proposals are suggestions based on retrieved excerpts. Verify them
    against the original papers and broader literature before relying on
    them in academic work. The notebook's novelty check only compares
    against the excerpts supplied to the model; it does not establish
    that an idea is globally novel.
-   **Privacy:** Uploaded papers and prompts are processed in your Colab
    runtime, and prompts sent for generation are sent to Groq. Do not
    upload or submit confidential material unless you are authorized to
    do so and have reviewed the relevant service terms.

## Troubleshooting

**`GROQ_API_KEY` is missing or authentication fails** - Confirm that the
secret name is exactly `GROQ_API_KEY`. - Confirm that notebook access is
enabled for the secret. - Check that the key is valid and has not been
revoked. - Do not share your key when asking for help; share the error
message with the key removed.

**The model is not available** - Check the models currently available in
your Groq account and the Groq documentation. - Update the `model` value
in the `call_llm()` function.

**No text is extracted from a PDF** - Check whether the PDF contains
selectable text. - The current notebook does not perform OCR on scanned
pages.

**The results disappear after restarting Colab** - This is expected with
the current in-memory ChromaDB setup. Run the notebook again and
re-upload the PDFs.

## Repository contents

``` text
.
├── GrokResearchbot.ipynb
└── README.md
```

## Contributing

Suggestions, bug reports, and improvements are welcome. Please do not
include API keys, private research papers, or other sensitive
information in issues or pull requests.

## Disclaimer

Grok ResearchBot is an experimental research-assistance tool. It does
not replace reading the source literature, expert judgment, or a
systematic literature review. Users should independently verify
citations, claims, research gaps, and novelty.
