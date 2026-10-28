# Week 2 Lab: Mini Semantic Search

A small search engine for student-helpdesk FAQs that finds answers by **meaning**, not exact keywords.
Built for the AI/ML AWS AASTU Cohort, Week 2 (Data, APIs & Embeddings).

## How it works

1. **Load** `faqs.csv` with Pandas
2. **Clean** it with functions (extra spaces, blank rows, duplicate questions)
3. **Chunk** each FAQ into overlapping word chunks
4. **Embed and store** the chunks in a Chroma collection
5. **Query** with a natural-language question and print the **top 3** matches

## Project structure

```
week-02/
├── week2.ipynb        # the notebook (run this)
├── faqs.csv           # sample dataset (33 clean FAQs + a few deliberately messy rows)
├── requirements.txt   # pandas, numpy, chromadb, jupyter
├── .gitignore         # keeps .venv, __pycache__ and .ipynb_checkpoints out of Git
└── README.md
```

## How to run

### Option A: Google Colab (easiest)

1. Open `week2.ipynb` in Colab (upload it, or open it from GitHub).
2. Upload `faqs.csv` when the notebook asks.
3. Click **Runtime > Run all**.

### Option B: On your computer (with a virtual environment)

```bash
git clone <your-repo-url>
cd <your-repo>/week-02

python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS / Linux:
source .venv/bin/activate

pip install -r requirements.txt
jupyter notebook week2.ipynb
```

Then choose **Run > Run All Cells**. The first run downloads Chroma's embedding model, so it needs internet.

The virtual environment (`.venv/`) is listed in `.gitignore` and is never committed.

## Example queries

Each query is worded differently from the FAQ it should find.

| Query | Expected FAQ topic |
|---|---|
| "I forgot my password" | Resetting the portal password |
| "when can I sign up for classes?" | When course registration opens |
| "internet is very slow in the library" | Reporting slow campus Wi-Fi |
| "I lost my ID" | Replacing a lost student ID card |
| "can I pay tuition in parts?" | Paying fees in installments |
| "how do I challenge a bad mark?" | Appealing an exam grade |

### Actual output from my run

```
Query: I forgot my password
  1. [Accounts] How do I reset my student portal password?
  2. [Accounts] I can't log in to the student portal. What should I do?
  3. [Accounts] How can I change the email address linked to my account?

Query: can I pay tuition in parts?
  1. [Fees] How do I pay my tuition fee?  (distance: 0.931)
  2. [Fees] Can I pay my fees in installments?  (distance: 1.116)
  3. [Registration] How do I add or drop a course?  (distance: 1.418)

Query: I lost my ID
  1. [Student Services] What should I do if I lose my student ID card?
  2. [Accounts] How do I reset my student portal password?
  3. [Registration] What happens if I miss the registration deadline?
```

## Notes

- Distance is shown next to each result. **Smaller means closer in meaning.**
- The dataset is a sample made for this lab. To use your own, replace `faqs.csv` with a file that has the columns `id`, `category`, `question`, `answer`.
