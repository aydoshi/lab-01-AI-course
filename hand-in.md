# Tokenizer Predictions (Complaint Item)

## Predictions for Claude Tokenizer:
- **RU / EN:** `1.60×`
- **KK / EN:** `2.80×`

## Reasoning & Basis:
1. **Basis for Prediction:** Predictions are based on **bytes** (UTF-8 byte length of Cyrillic and Kazakh extended characters) and subword frequency statistics.
2. **Tokenizer Mechanics:** Tokenizers follow **bytes / subword byte sequences** (BPE algorithm). They do not follow character count or full word count because non-Latin characters (like Cyrillic and Kazakh-specific letters) consume 2 bytes per character, requiring more subword tokens to represent.
3. **Range Selection:** Since Claude's tokenizer vocabulary efficiency falls between OpenAI's `cl100k_base` (older, higher token count) and `o200k_base` (newer, highly optimized for multilingual text):
   - For **RU / EN**, the value is chosen between 1.23× and 1.93× → **1.60×**
   - For **KK / EN**, the value is chosen between 1.68× and 3.92× → **2.80×**