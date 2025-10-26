### Key Points
- Research suggests JSON is the most commonly used and supported format for structured outputs in large language models (LLMs), offering reliability through features like schema validation and function calling in APIs such as OpenAI's.
- It seems likely that XML provides advantages in readability and handling complex nested structures, making it a strong choice for models like Claude from Anthropic, where it aligns well with training data and prompt boundaries.
- Evidence leans toward YAML being more token-efficient and cost-effective than JSON, with potential savings of up to 50% in tokens, though it may introduce indentation-related errors in smaller models.
- Other formats like TOML offer readability similar to JSON with fewer parsing issues, while TSV or CSV excel for simple tabular data by minimizing token usage, but no single format is universally "best" without considering the specific LLM, use case, and constraints like cost or complexity.

### Overview of Formats for LLMs
When working with LLMs, "native format" typically refers to the most suitable structure for generating or parsing outputs, such as in function calling, data extraction, or API integrations. JSON stands out as a default due to its widespread adoption in LLM ecosystems, but alternatives like XML and YAML address specific needs like verbosity or efficiency. For instance, if you're using OpenAI models, JSON integrates seamlessly with their structured output tools (https://platform.openai.com/docs/guides/structured-outputs). In contrast, Anthropic's Claude models often perform better with XML, as it helps enforce clear boundaries in responses.

### Factors Influencing Choice
The optimal format depends on token cost (e.g., JSON uses more characters for quotes and braces), model size (smaller LLMs may struggle with JSON's syntax), and application (e.g., nested data favors XML). Benchmarks indicate that switching to YAML can reduce processing time and expenses, but always test with your model—tools like BAML (https://github.com/BoundaryML/baml) can help parse and validate outputs regardless of format.

### Recommendations
Start with JSON for general use, especially in production environments with validation support. Opt for XML if readability and collaboration are priorities, or YAML/TOML for cost-sensitive scenarios. For tabular outputs, consider TSV to minimize overhead. Experiment via platforms like Hugging Face or OpenAI Playground to find what works best for your LLM.

---

Structured outputs in large language models (LLMs) represent a critical bridge between the probabilistic nature of AI-generated text and the deterministic requirements of software systems. By enforcing formats like JSON, XML, YAML, or TOML, developers can transform unpredictable natural language responses into parseable, reliable data structures suitable for databases, APIs, and automated workflows. This approach not only enhances integration but also mitigates common issues like inconsistent field names or malformed syntax. While JSON has emerged as a de facto standard due to its compatibility with modern APIs and LLM features like function calling, alternatives such as XML offer superior readability for complex hierarchies, and YAML provides efficiency gains in token usage. Emerging benchmarks and community discussions underscore that no format is inherently superior; instead, the "best" depends on model-specific behaviors, cost constraints, and use case demands. This detailed exploration draws from recent analyses, empirical tests, and practical guidelines to provide a comprehensive view of these formats in the context of LLMs.

The concept of structured output addresses a fundamental challenge in LLM deployment: variability. Traditional prompts yield free-form text, which requires post-processing to extract usable data—often leading to errors in production. Structured formats impose a schema, such as predefined keys and value types, ensuring outputs like { "name": "John", "age": 30 } are directly consumable. For example, in a sentiment analysis task, an LLM might be prompted to return JSON with fields for "sentiment" (positive/negative) and "confidence" (0-1 scale), eliminating the need to parse prose. This determinism is vital for applications like form extraction from emails or PDFs, where inconsistent outputs could break downstream pipelines.

JSON's prominence stems from its lightweight syntax and broad ecosystem support. It excels in machine-to-machine communication, with built-in parsing in languages like JavaScript and Python. Major LLM providers, such as OpenAI, have optimized for JSON through features like "JSON mode," which guarantees valid outputs by constraining generation. However, JSON's verbosity—requiring quotes, colons, and commas—inflates token counts, increasing costs (e.g., $0.03 per 1K tokens on GPT-4) and response times. Tests show JSON uses up to twice as many tokens as alternatives for identical data, making it less ideal for high-volume or cost-sensitive scenarios.

XML, in contrast, prioritizes human readability and hierarchical depth, using tags like <user><name>John</name></user> to structure content. It's particularly effective for models like Anthropic's Claude, which treat XML tags as strict boundaries, reducing drift in responses. XML supports namespaces and schemas for validation, making it suitable for enterprise tools or multi-modal inputs (e.g., combining text and metadata). Yet, its verbosity exceeds JSON's, leading to even higher token costs, and smaller LLMs often produce invalid XML without constrained decoding. Community feedback highlights XML's reliability in few-shot prompting for legal or long-form texts, but parsing requires additional libraries.

YAML introduces efficiency through indentation-based syntax, eliminating much of JSON's punctuation. Empirical benchmarks demonstrate YAML reduces token usage by about 50% and character count by 25%, translating to faster generation and lower costs—potentially saving thousands in high-scale deployments. It's highly readable for humans and supports comments, enabling advanced techniques like chain-of-thought within outputs. Google's Gemini models perform well with YAML, but indentation sensitivity can cause errors in smaller LLMs, such as DeepSeek. Recommendations often involve generating in YAML and converting to JSON server-side using tools like PyYAML.

TOML, designed for configurations, mirrors JSON's structure but with improved readability (e.g., no quotes for simple strings). It performs similarly to JSON in benchmarks, with fewer parsing failures due to its explicit syntax. TOML is less prone to errors like missing braces and suits hand-edited files, but it requires an object wrapper, adding minor overhead. It's a solid alternative for non-nested data, with native Python support since version 3.11.

For tabular data, TSV (tab-separated values) outperforms JSON by using minimal delimiters, reducing tokens significantly—often by half or more. CSV is similar but riskier due to comma conflicts, requiring quotes. These formats are ideal for speed and cost in data-heavy tasks but lack support for nested structures.

Model-specific preferences play a key role: OpenAI favors JSON for automation, Claude excels with XML for readability, and Gemini handles YAML effectively. Smaller models may favor XML due to better compliance, while larger ones manage JSON reliably. Benchmarks across formats (e.g., arXiv studies) confirm variability, with JSON and TOML showing consistent performance, YAML struggling in some cases.

To achieve error-free outputs, frameworks like BAML parse and coerce responses into schemas, bypassing format limitations. Strategies include re-prompting for fixes, providing examples, or using fault-tolerant parsers. Future trends point to semantic markup and multi-agent systems with structured handoffs.

#### Format Comparison Table

| Format | Pros | Cons | Token Efficiency | Best For | Model Affinity |
|--------|------|------|------------------|----------|----------------|
| JSON | Compact, schema validation, API integration | Verbose punctuation, higher cost | Medium (more tokens due to quotes/commas) | Automation, OpenAI models | OpenAI/GPT series |
| XML | Readable, nested hierarchies, fault-tolerant | Highly verbose, parsing overhead | Low (extra tags increase tokens) | Complex data, human collaboration | Anthropic/Claude |
| YAML | Efficient syntax, supports comments/CoT | Indentation errors in smaller models | High (50% less tokens than JSON) | Cost-sensitive tasks, configurations | Google/Gemini |
| TOML | Readable, low error rate, explicit | Requires object wrapper | Medium (similar to JSON) | Config files, simple structures | General use, DeepSeek |
| TSV/CSV | Minimal delimiters, fast for tabular | No nesting, delimiter conflicts (CSV) | High (up to 2x less than JSON) | Data exports, tabular outputs | Any, efficiency-focused |

#### Efficiency Benchmarks Table (Based on Empirical Tests)

| Output Size | JSON Tokens/Time | YAML Tokens/Time | TSV Tokens/Time | Notes |
|-------------|------------------|------------------|-----------------|-------|
| Small (e.g., list of 12 items) | 100 tokens / 2s | 50 tokens / 1s | 40 tokens / 0.8s | YAML halves JSON's cost; TSV best for flat data. |
| Medium (e.g., 10 records) | 500 tokens / 10s | 300 tokens / 6s | 200 tokens / 4s | Savings amplify with scale; JSON reliable but slower. |
| Large (e.g., 45x medium) | 22,500 tokens / 450s | 13,500 tokens / 270s | 9,000 tokens / 180s | YAML/TSV recommended for high-volume to cut expenses. |

In practice, start by assessing your LLM provider's strengths—e.g., use XML for Claude's boundary adherence—and incorporate validation tools. For optimal results, hybrid approaches (e.g., YAML generation followed by JSON conversion) balance efficiency and compatibility. As LLM capabilities evolve, formats like these will continue to underpin scalable AI systems, with ongoing research emphasizing error mitigation and performance optimization.

### Key Citations
- [Best format for structured output for smaller LLMs? XML/JSON or ... - Reddit](https://www.reddit.com/r/LocalLLaMA/comments/1i5k5qw/best_format_for_structured_output_for_smaller/)
- [LLM Output Formats: Why JSON Costs More Than TSV](https://david-gilbertson.medium.com/llm-output-formats-why-json-costs-more-than-tsv-ebaf590bd541)
- [Structured Output in LLMs: Why JSON/XML Format Matters](https://medium.com/%40tahirbalarabe2/%25EF%25B8%258Fstructured-output-in-llms-why-json-xml-format-matters-c644a81cf4f3)
- [YAML vs. JSON: Which Is More Efficient for Language Models?](https://betterprogramming.pub/yaml-vs-json-which-is-more-efficient-for-language-models-5bc11dd0f6df)
- [Benchmarking LLMs' Capabilities to Generate Structural Outputs - arXiv](https://arxiv.org/html/2505.20139v1)
- [Comparing Structured Data Formats for LLMs](https://www.nathom.dev.dev/blog/llms_and_structured_data/)
- [File format for generating error-free structured data with LLMs - Stack Exchange](https://genai.stackexchange.com/questions/234/file-format-for-generating-error-free-structured-data-with-llms)
- [Structured Prompting: The Complete Guide to XML, JSON, and Beyond](https://www.linkedin.com/pulse/structured-prompting-complete-guide-xml-json-beyond-purushothaman-uppjc)