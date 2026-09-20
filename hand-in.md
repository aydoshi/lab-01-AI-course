# Hand-in Report: Tokenizer & Cost Analysis

## 1. Predictions vs. Measured Values (Complaint Item)

| Language Ratio | Predicted Ratio | Measured Ratio (request_tokens) |
| :--- | :---: | :---: |
| RU / EN | 1.60x | 1.93x |
| KK / EN | 2.80x | 3.92x |

## 2. Annual Cost Table (5,000 Requests / Day)

*Values from `part3_5000.txt`:*

| Model | Language | Daily Cost (USD) | Annual Cost (USD) |
| :--- | :---: | :---: | :---: |
| Claude 3.5 Haiku | EN | [INSERT] | [INSERT] |
| Claude 3.5 Haiku | RU | [INSERT] | [INSERT] |
| Claude 3.5 Haiku | KK | [INSERT] | [INSERT] |
| Claude 3.5 Opus | KK | [INSERT] | [INSERT] |

The daily volume of 5,000 requests was selected as a realistic support queue load for a medium-to-large business or financial service in Kazakhstan.

## 3. Model Recommendation for Kazakh Support Queue

I recommend deploying **Claude 3.5 Haiku** for the Kazakh customer support queue.

- **Cost:** Haiku is 5 times cheaper than Opus according to the README, which is critical given that Kazakh text incurs a heavy token penalty (KK/EN = 3.92x).
- **Quality:** In the complaint text, the customer mentions "attaching the contract," but no document is provided; a high-quality response must ask for the missing document and refuse to invent a rate-change reason, which Haiku executes reliably without hallucination.

## 4. Additional Cost Savings Leverage

A major cost-saving leverage not included in this lab is **prompt caching**, which allows reusing static system prompts and context at up to a 90% discount on input token costs.