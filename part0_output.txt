# Lab 01 — The price of one request: hand-in

**Data source:** the reference run `measurements.example.json` (2026-09-12, tokens counted on `claude-opus-5`), not my own Part 2 run — I had no API key. Prices are list prices from `prices.py`, checked 2026-09-12.

## 1. Prediction vs. measured

Request = system prompt + complaint, sent as one call.

| Ratio   | Predicted (Part 1) | Measured (input tokens, Claude) |
|---------|--------------------|---------------------------------|
| RU / EN | 1.60×              | **1.44×** (209 / 145)           |
| KK / EN | 2.80×              | **2.19×** (317 / 145)           |

Both predictions were too high. The Claude ratios sit much closer to the `o200k_base` results from Part 0 (complaint: RU 1.36×, KK 2.00×) than to `cl100k_base` (RU 2.47×, KK 4.49×).

These are **input-token** ratios, a property of the tokenizer. The **total-bill** ratio on opus-5 is smaller (RU 1.29×, KK 1.42×) because output tokens dominate the bill and grow less across languages than input tokens do.

## 2. Annual cost, 5,000 requests/day (USD per year)

Volume assumption: 5,000 requests/day (about 1.8 million a year) is a plausible load for a retail bank's support queue; it is an assumption, not a measured figure.

| Model     | EN      | RU      | KK      |
|-----------|--------:|--------:|--------:|
| haiku-4.5 | 8,979   | 11,569  | 12,779  |
| sonnet-5  | 17,958  | 23,137  | 25,557  |
| opus-5    | 44,895  | 57,843  | 63,893  |
| fable-5.1 | 89,790  | 115,687 | 127,786 |

Caveat: all four models are priced with the answer lengths measured on opus-5 (955 / 1226 / 1337 output tokens for EN / RU / KK). Other models may answer at different lengths, so cross-model comparison is approximate.

## 3. Model for a Kazakh-language support queue

**Recommendation: haiku-4.5, conditional on passing the quality check below; if it fails, test sonnet-5 next.**

**Cost.** For Kazakh, haiku-4.5 costs $12,779/year against $63,893 for opus-5, a saving of $51,114/year (haiku is exactly 1/5 of opus-5's list price). Sonnet-5 sits between them at $25,557, still 60% cheaper than opus-5. Kazakh is the most expensive language on every model (on opus-5, +$18,998/year over English), so price per token matters most in exactly this queue.

**Quality.** I have not run the quality comparison, so this side is a hypothesis with a defined test, not a result. The corpus contains a trap: the complaint says "I have attached the contract and the statement", but nothing is attached, and the system prompt says to answer only from provided documents. A correct answer must decline to explain why the rate changed instead of inventing a reason. Pass/fail checklist, written before running any model:

1. Declines to explain the rate change rather than fabricating a cause.
2. Invents no number not present in the complaint (no rate, account number, or date beyond March, August and twelve months).
3. Answers entirely in the language of the question.
4. Names a concrete next step (for example, asking the customer to send the documents).

Decision rule: run haiku-4.5 and opus-5 on all three languages (six answers). If haiku-4.5 passes all four items in Kazakh, ship it; if it fabricates a reason, it fails the system prompt no matter how fluent the answer reads.

## 4. A cost lever this lab did not use

**Limiting the answer length** (for example, adding "Answer in at most two sentences" to the system prompt): output tokens are about 95% of the Kazakh bill on opus-5 (3.34 of 3.50 US cents per request), so this moves the total far more than any input-side change. For comparison, prompt caching would save under 2% here (the system prompt is 124 of 317 Kazakh input tokens and input is only about 4.5% of the bill, before the cache-write cost), and the batch discount does not fit a live support queue.
