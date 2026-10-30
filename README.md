# Product Task: Industry Classification

## Overview

You have 300 real companies, each anonymised. Assign each one the five-digit NAICS 2022 code that best describes its primary business. You'll work from the company's web pages and job posts. You also get an API that tells you whether a guess is correct.

This is a hard task, and we don't expect anyone to get every company right. Give it your best shot.

We care far more about your approach and how you work than about your score. We want to understand how you broke the problem down, which decisions you made along the way, what you tried that didn't work, and what you trusted or didn't, and why.

## What you'll receive

Download the data: [product-task-v2.zip](https://github.com/generatemomentum/product-task/releases/download/data-v2/product-task-v2.zip) (132 MB, about 800 MB unzipped).

```
product-task-v2/
├── naics_2022_level5.json      # 690 valid codes: [{ "code": "11111", "title": "Soybean Farming" }, ...]
└── accounts/
    └── acc_xxxxxxxx/           # one folder per company (300 total)
        ├── site/               # pages from the company's own website
        ├── web/                # pages about the company from other sites
        └── jobs/               # the company's job postings
```

- Each company has between 4 and about 220 pages, with a median of about 45.
- Every file is HTML. Some pages are messy or broken, the way scraped pages usually are.

**Anonymisation.** Names, brands, people, URLs, emails, phone numbers and addresses have been replaced with placeholders such as `[COMPANY]`, `[PERSON_3]` and `[EMAIL_2]`. Within a company, the same placeholder always means the same thing. Some text has been reworded, generalised ("a city in [COUNTRY]") or translated. Descriptions of products, services, job duties and customers have been kept as they were, and that's the evidence you should rely on.

You'll also get an **OpenAI API key** and a key for the check endpoint.

## Model

Your code may only call one model: **Luna 6**, through the OpenAI API, using the key we give you. We'll run your code with the same model when we evaluate it. Don't use other models, other providers or local models in the submitted project. You can still use any tools you like while you work, such as coding assistants.

## The check endpoint

```
POST https://api-dev.kernel.ai/api/v1/naics-check
x-api-key: <your key>
Content-Type: application/json

{ "id": "acc_xxxxxxxx", "code": "12345" }
```

The response is `{ "match": true }` or `{ "match": false }`.

- **Rate limits:** 10 requests per second and 10,000 per day.
- **Every call is logged** against your key, and how you use the endpoint is part of the evaluation. Use it to check your work, not to brute-force answers.
- **The endpoint is wrong for exactly 15 of the 300 companies.** If strong evidence contradicts it, tell us. Don't silently go along with it.

## What to submit

1. **Your code, written in <ins>TypeScript</ins>.** Send it as a zip of the project or a GitHub link. It must classify companies from a folder laid out like `accounts/` and write a CSV. Include:
   - A single command to run it, for example `npm run classify -- --accounts <path> --naics <path> --out answers.csv`.
   - Setup instructions and dependencies, including the Node version you used.
   - Code that reads the OpenAI key from the `OPENAI_API_KEY` environment variable. Don't hardcode the key or include it in your submission.

2. **`answers.csv`** for the 300 companies, with one row per company:

   | column | description |
   |---|---|
   | `id` | `acc_xxxxxxxx` |
   | `code` | Your five-digit NAICS 2022 code. It must be in `naics_2022_level5.json`. |
   | `confidence` | `high`, `medium` or `low` |
   | `notes` | Optional. Your key evidence, or anything that looked off. |

3. **A write-up, one or two pages**, covering:
   - Your approach and why you chose it.
   - Any companies where you disagree with the endpoint, with the evidence.
   - Any content you thought was misleading or unreliable, and how you handled it.
   - What you would do differently with more time.

## How we'll evaluate it

We'll score `answers.csv` for the 300 companies. We'll also run your code on 50 more companies you haven't seen. They're prepared the same way and use the same folder layout. You won't see these companies or your results on them.

Anything you learned from the 300, such as rules, prompts or examples, can go into your code. A lookup table of the 300 answers won't help on companies you haven't seen.

Remember that the score matters less than your approach. We mainly care about how you broke the problem down, which decisions you made along the way, what you tried that didn't work, and what you trusted or didn't, and why.
