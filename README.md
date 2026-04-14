# LLM Test Benchmark 100

**100 extremely challenging, multidisciplinary questions** in **10 major world languages** designed to rigorously evaluate Large Language Models (LLMs).

This benchmark tests deep domain knowledge, logical reasoning, cross-disciplinary understanding, factual precision, and handling of complex edge cases across computer science, philosophy, physics, law, medicine, economics, quantum mechanics, linguistics, and more.

### Supported Languages
- English (EN)
- German (DE)
- French (FR)
- Japanese (JA)
- Spanish (ES)
- Chinese (ZH-CN)
- Russian (RU)
- Arabic (AR)
- Hindi (HI)

The questions are **evenly distributed** (~10 per language) and nicely mixed throughout the list for fair and balanced evaluation.

## 📋 Questions

All 100 questions are available in **[QUESTIONS.md](QUESTIONS.md)**

## Results (free plan of the LLM websites) 04/14/2026

| # | Criterion                | Description                                         |
| - | ------------------------ | --------------------------------------------------- |
| 1 | Technical Correctness    | Factual accuracy, verifiable errors                 |
| 2 | Depth & Completeness     | Coverage of all sub-aspects                         |
| 3 | Code Quality             | Correctness, idiomatic usage, constraint compliance |
| 4 | Linguistic Appropriatess | Responds in the language of the question            |
| 5 | Nuance                   | Edge cases, counterarguments, differentiation       |



| Rang | LLM     | Accuracy	 | Conciseness | Code | Language | Nuance | Total /50 |
| ---- | ------- | ----------- | ----------- | ---- | -------- | ------ | --------- |
| 🥇 1 | Claude  | 9.5         | 8.0         | 8.5  | 9.5      | 9.5    | 45.0      |
| 🥈 2 | Grok    | 9.0         | 8.5         | 8.5  | 9.0      | 9.0    | 44.0      |
| 🥉 3 | Qwen    | 8.5         | 8.5         | 8.5  | 9.0      | 8.0    | 42.5      |
| 4    | ChatGPT | 8.5         | 9.5         | 7.5  | 9.0      | 6.0    | 40.5      |
| 5    | Mistral | 8.5         | 3.0         | 9.0  | 9.5      | 9.0    | 39.0      |
| 6    | ERNIE   | 5.5         | 7.0         | 4.5  | 6.5      | 6.0    | 29.5      |

## Usage

1. Copy any question from `QUESTIONS.md`
2. Paste it into your target LLM with this simple prompt: Answer all questions in their language, be brief, just plaintext.
3. Evaluate the response on:
   - Factual accuracy
   - Depth of reasoning
   - Clarity and structure
   - Handling of nuance and edge cases
4. Compare performance across models (GPT, Claude, Grok, Llama, Gemini, Qwen, etc.)

## License

This repository is licensed under the **[MIT License](LICENSE)**.  
You are free to use, modify, and distribute the questions for research, benchmarking, and commercial evaluation purposes.

## Contributing

Feel free to open issues or pull requests if you want to:
- Add new questions
- Improve formatting
- Add evaluation scripts or JSON exports
- Translate questions into additional languages

---

Made with ❤️ by [Benjamin-Wegener](https://github.com/Benjamin-Wegener) for the LLM community.
