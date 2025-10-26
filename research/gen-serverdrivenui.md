### Key Points
- Research suggests that integrating LLMs with server-driven UI (SDUI) is an emerging approach, where LLMs generate structured JSON to dynamically render interfaces, often for adaptive apps like weather displays.
- It seems likely that open-source projects focus on browser or server-based LLM calls to produce JSON schemas for UI components, enabling real-time updates without full app redeploys.
- Evidence leans toward weather-related examples being common demonstrations, as they involve fetching dynamic data via APIs and rendering it through LLM-generated JSON for customizable UIs.
- While innovative, these systems may involve trade-offs like API key security or latency, with community efforts emphasizing extensibility for custom scenarios.

### Understanding LLM-Generated SDUI
LLM-generated server-driven UI combines natural language processing with dynamic interface rendering, where user inputs—like "what's the weather"—prompt an LLM to output JSON. This JSON defines UI elements (e.g., cards, lists) and integrates data from APIs, allowing clients to render adaptive interfaces. Projects often use frameworks like LangChain or OpenAI SDKs for this.

### Notable Projects
Open-source repositories like Generative-UI and Morphic enable browser-side generation, while others like Quarkus-based apps handle server-side logic. For weather, tools like AnythingLLM provide plug-and-play JSON skills.

### Getting Started
Clone repos from GitHub, set API keys, and test with prompts for dynamic content.

---

Integrating large language models (LLMs) with server-driven user interfaces (SDUI) represents a significant advancement in AI-assisted application development, allowing for dynamic, context-aware interfaces generated from natural language inputs. In this paradigm, an LLM processes user queries—such as "what's the weather"—and produces structured JSON outputs that dictate UI layouts, components, and data integrations. This JSON is then parsed by client-side or server-side renderers to create interactive elements, often incorporating real-time data from external APIs like weather services. The approach enhances flexibility, enabling rapid prototyping and customization without hardcoded UI logic, while addressing challenges like data accuracy and performance through modular designs. Open-source projects in this space leverage frameworks such as LangChain, React, or Unity to facilitate LLM-driven generation, emphasizing browser compatibility, API agnosticism, and community extensibility. While promising for applications like personalized dashboards or interactive tools, these systems must navigate issues such as token limits, API security, and ensuring JSON validity to maintain reliability.

At its core, the workflow involves several stages: user input capture (e.g., voice or text), LLM prompting for analysis and categorization, contextual data retrieval (e.g., location for weather), and JSON generation conforming to predefined schemas. For instance, the JSON might specify "renderHint" for component types (text, list, card) and "data" fields for content, allowing the frontend to dynamically assemble the UI. This server-driven model decouples interface logic from the client, supporting A/B testing, real-time updates, and cross-platform consistency. Projects often incorporate guardrails—like validation loops—to enforce JSON structure, preventing malformed outputs that could break rendering. Integration with tools like OpenAI's function calling or Anthropic's APIs enables the LLM to decide on actions, such as fetching weather data before finalizing the UI response.

One prominent example is the Quarkus-based LLM-Driven UI Rendering project, which employs a two-step AI pipeline with LangChain4j and Ollama. Here, an LLM (e.g., Qwen3:8b) is prompted to output only valid JSON with an "elements" array, where each element includes a "renderHint" (e.g., "text", "list", "website") and associated data. This JSON serves as the blueprint for frontend rendering using Preact, falling back to raw display for unsupported hints. The system uses a JsonGuardrail to validate and reprompt invalid outputs, ensuring robustness. While no direct weather demo is provided, the architecture supports extensions like integrating Open-Meteo APIs to generate weather cards via JSON, aligning with server-driven principles where the backend dictates UI structure.

Similarly, Generative-UI by Anilturaga offers a browser-only implementation inspired by "Imagine with Claude," where LLMs generate HTML and JavaScript directly in a React Flow canvas. Using tools like "create_new_window" or "dom_replace," the LLM outputs structured calls (JSON-like) to create or edit iframe-based windows. This enables dynamic UI for scenarios like weather apps: a user prompt could trigger a new window with API-fetched data, updated via event messages. No backend server is needed, with LLM calls handled client-side, making it ideal for rapid, interactive prototyping of data-driven interfaces.

The gen-ui-python repository provides a template for building generative UI apps with LangChain Python, AI SDK, and Next.js. It features pre-built Shadcn UI components and supports LLM-generated dynamic elements, such as dashboards or chat interfaces. Server-driven aspects come through LangChain agents that output structured data (potentially JSON) for rendering. Developers can extend it to generate weather UIs by configuring agents to call geocode or weather APIs, rendering results as custom components.

Morphic, an AI-powered search engine, incorporates generative UI to adapt interfaces based on queries. LLMs from providers like OpenAI or Grok process inputs, generating JSON-configured responses that the frontend renders dynamically. For weather queries, it could synthesize search results with UI elements like forecast cards, using server-driven JSON payloads from Supabase-backed endpoints.

In the XR domain, LLMER uses LLMs to generate JSON for interactive virtual worlds in Unity. User voice commands are transcribed and refined into JSON specifying objects, animations, or real-world fusions (e.g., placing virtual weather indicators on physical surfaces). While not explicitly weather-focused, its dynamic JSON handling supports extensions like simulating environmental effects, reducing latency compared to code-based alternatives.

AnythingLLM exemplifies practical integration with its custom agent skills defined in "plugin.json." The weather skill uses examples like "What is the weather in Tokyo?" paired with JSON calls (e.g., {"latitude": 35.6895, "longitude": 139.6917}) to guide LLM invocations. Setup arguments render UI fields for configuration, and the handler script executes Open-Meteo API calls, returning data for UI display. This JSON-driven approach ensures seamless, customizable weather interfaces.

Personal weather app projects, like Lior Elgali's, demonstrate LLM-assisted development where tools like ChatGPT and GPT Engineer generate code, including JSON handlers for weather data. The app's server-driven UI allows dynamic updates, with LLMs contributing over 90% of the codebase for fetching and rendering forecasts.

Frontend integrations, as in Ignatovich's article, embed LLM agents directly into UIs using React and Fastify. For weather, the agent parses queries, calls tools via JSON (e.g., {"name": "getWeather", "arguments": {"city": "Wroclaw"}}), and returns structured responses for rendering. This two-call process (LLM decision + API execution) supports streaming for real-time UI updates.

Finally, Simon Willison's weather reports project uses LLMs to blend JSON weather data with webcam images, generating descriptive narratives. While more text-focused, it highlights JSON's role in structuring inputs for LLM-enhanced UIs.

To compare these projects, consider the following tables summarizing key attributes and use cases.

#### Project Overview Table

| Project Name | GitHub/Link | Primary Languages/Frameworks | LLM Role | JSON/SDUI Focus | Weather Example |
|--------------|-------------|------------------------------|----------|-----------------|-----------------|
| Quarkus LLM-Driven UI | [the-main-thread.com/p/quarkus-langchain4j-ollama-two-step-ai-pipeline](https://www.the-main-thread.com/p/quarkus-langchain4j-ollama-two-step-ai-pipeline) | Java, Quarkus, LangChain4j, Preact | Generates JSON with render hints | Strict JSON output for server-driven rendering | Extensible to weather via APIs |
| Generative-UI | [github.com/Anilturaga/Generative-UI](https://github.com/Anilturaga/Generative-UI) | TypeScript, React Flow | Creates/edits UI via tool calls | JSON-like schemas for dynamic updates | Supports dynamic data like weather dashboards |
| gen-ui-python | [github.com/bracesproul/gen-ui-python](https://github.com/bracesproul/gen-ui-python) | Python, Next.js, LangChain | Powers generative components | Structured data for server agents | Customizable for weather UIs |
| Morphic | [github.com/miurla/morphic](https://github.com/miurla/morphic) | JavaScript, Supabase | Query processing and UI adaptation | JSON configs for server-driven responses | Handles weather searches dynamically |
| LLMER | [arxiv.org/pdf/2502.02441](https://arxiv.org/pdf/2502.02441) | Unity, Python | Generates JSON for XR objects/animations | Structured JSON for modular execution | Potential for weather simulations |
| AnythingLLM Weather Skill | [docs.anythingllm.com/agent/custom/plugin-json](https://docs.anythingllm.com/agent/custom/plugin-json) | JavaScript | Invokes API via prompted JSON | Plugin.json defines calls | Direct weather fetch with examples |
| Personal Weather App | [medium.com/@liorelgali/i-built-my-personal-weather-web-app-with-llms](https://medium.com/@liorelgali/i-built-my-personal-weather-web-app-with-llms-im-using-it-multiple-times-a-day-d5b13457674f) | Various (LLM-generated) | Code and data generation | JSON for data handling | Full weather app built with LLMs |
| Frontend LLM Agents | [medium.com/@ignatovich.dm/frontend-in-the-age-of-ai](https://medium.com/@ignatovich.dm/frontend-in-the-age-of-ai-how-to-integrate-llm-agents-right-into-the-ui-0514cd7a20fe) | React, Fastify | Tool calling for data | JSON for function arguments/responses | Weather query with API integration |
| Weather Vibes | [simonwillison.net/2024/Oct/29/weather-reports-with-llms](https://simonwillison.net/2024/Oct/29/weather-reports-with-llms) | Python | Descriptive report generation | JSON weather data input | Blends API JSON with images |

#### Use Case Comparison Table

| Use Case | Recommended Projects | Why Suitable | Potential Drawbacks |
|----------|----------------------|--------------|---------------------|
| Browser-Only Dynamic UI | Generative-UI, Morphic | Client-side LLM calls for real-time generation | Browser security limits on APIs |
| Server-Side JSON Rendering | Quarkus LLM-Driven UI, AnythingLLM | Validated JSON for robust frontend parsing | Latency from LLM reprompts |
| XR/Immersive Interfaces | LLMER | JSON for object creation and animations | Limited to Unity; context memory issues |
| Weather-Specific Demos | AnythingLLM, Frontend LLM Agents, Weather Vibes | Built-in examples with API integration | Dependency on external services |
| Prototyping/Custom Apps | gen-ui-python, Personal Weather App | Templates and LLM code generation | May require manual tweaks for production |

In practice, starting involves setting up environments (e.g., npm/yarn for JS, Poetry for Python) and configuring API keys. For weather scenarios, prompts can specify custom settings like units (Celsius/Fahrenheit) or locations, with LLMs generating tailored JSON. Contributions to these repos often focus on adding tools or schemas, advancing the field toward more seamless AI-UI synergies.

### Key Citations
- [GitHub - Anilturaga/Generative-UI: Open Source implementation of "Imagine with Claude"](https://github.com/Anilturaga/Generative-UI)
- [LLM-Driven UI Rendering with Quarkus, Langchain4j & Ollama](https://www.the-main-thread.com/p/quarkus-langchain4j-ollama-two-step-ai-pipeline)
- [GitHub - bracesproul/gen-ui-python: 🧬🐍 Generative UI web application built with LangChain Python, AI SDK & Next.js](https://github.com/bracesproul/gen-ui-python)
- [GitHub - miurla/morphic: An AI-powered search engine with a generative UI](https://github.com/miurla/morphic)
- [LLMER: Crafting Interactive XR Worlds with JSON Data Generated by LLMs](https://arxiv.org/pdf/2502.02441)
- [plugin.json reference ~ AnythingLLM](https://docs.anythingllm.com/agent/custom/plugin-json)
- [I built my personal weather web app with LLMs. I’m using it multiple times a day.](https://medium.com/@liorelgali/i-built-my-personal-weather-web-app-with-llms-im-using-it-multiple-times-a-day-d5b13457674f)
- [Frontend in the Age of AI: How to Integrate LLM Agents Right into the UI](https://medium.com/@ignatovich.dm/frontend-in-the-age-of-ai-how-to-integrate-llm-agents-right-into-the-ui-0514cd7a20fe)
- [Generating Descriptive Weather Reports with LLMs](https://simonwillison.net/2024/Oct/29/weather-reports-with-llms/)