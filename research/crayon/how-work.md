Crayon is a Generative UI SDK designed to help developers build AI-powered chat interfaces using React. It processes streaming responses from large language models (LLMs) and transforms them into interactive UI elements like charts, forms, or custom components, rather than just plain text. This makes it easier to create engaging, agentic applications that integrate with any backend framework, such as LangChain or FastAPI, while handling state management, theming, and responsiveness.

At a high level, Crayon works by:
1. Managing chat state with React hooks (e.g., for threads and messages).
2. Streaming LLM responses via tools like Server-Sent Events (SSE), converting them into structured events.
3. Rendering those events as dynamic UI components in the frontend, allowing for real-time, interactive experiences.
It's backend-agnostic, so you can connect it to any API that handles AI requests, and it's organized into packages like `@crayonai/react-core` for state/hooks, `@crayonai/react-ui` for components, and `@crayonai/stream` for processing streams.

Here's a simple example of how it works, based on a basic chat implementation in a Next.js app. This setup uses in-memory storage for messages, integrates with an LLM like OpenAI (or via a platform like Thesys C1), and streams responses to a React frontend. It demonstrates a foundational chat interface where user prompts are sent, processed, and rendered dynamically.

### Prerequisites
- Node.js and npm installed.
- A Next.js project (create one with `npx create-next-app@latest`).
- An API key for an LLM provider (e.g., OpenAI or Thesys).

### Step 1: Installation
Install the core packages:
```
npm install @crayonai/react-core @crayonai/react-ui @crayonai/stream openai
```
(You may need additional deps like `@thesysai/genui-sdk` if using Thesys.)

### Step 2: Backend Setup (API Route for Chat Handling)
In your Next.js app, create an API route at `pages/api/chat.ts` (or `app/api/chat/route.ts` in App Router). This handles incoming messages, stores them in memory, calls the LLM, and streams the response back.

```typescript
// In-memory storage (simple object for threads)
const messageStore: Record<string, DBMessage[]> = {};

// Type for messages (compatible with OpenAI)
interface DBMessage {
  id?: string;
  role: 'user' | 'assistant';
  content: string;
}

export default async function handler(req: Request) {
  const { prompt, threadId, responseId } = await req.json(); // prompt is a DBMessage

  // Get or initialize thread storage
  if (!messageStore[threadId]) messageStore[threadId] = [];
  const messages = messageStore[threadId];

  // Add user message
  messages.push(prompt);

  // Prepare OpenAI-compatible messages (remove custom 'id' if present)
  const openAIMessages = messages.map(({ id, ...msg }) => msg);

  // Configure OpenAI client (example with Thesys C1 model)
  const openai = new OpenAI({ apiKey: process.env.THESYS_API_KEY }); // Use your env var
  const stream = await openai.chat.completions.create({
    model: 'c1/anthropic/claude-3.5-sonnet/v-20250617', // Or any LLM model
    messages: openAIMessages,
    stream: true,
  });

  // Transform and stream response using Crayon's stream package
  const transformedStream = transformStream(stream); // From @crayonai/stream

  // Add assistant's response to storage (as it streams)
  const assistantMsg: DBMessage = { id: responseId, role: 'assistant', content: '' };
  messages.push(assistantMsg);

  // Pipe the stream to the response
  return new Response(transformedStream, {
    headers: { 'Content-Type': 'text/event-stream' },
  });
}
```
This route processes a user prompt, appends it to the thread's message list, calls the LLM for a streaming completion, transforms the stream into events, and sends it back in real-time. Messages are stored in a simple object keyed by `threadId` for conversation persistence (in-memory here, but you could swap for a database like Prisma).

### Step 3: Frontend Setup (React Component)
In your main app component (e.g., `pages/index.tsx`), use Crayon's chat component to render the interface. It handles state, user input, and displaying streamed responses.

```typescript
import { CrayonChat, ThemeProvider } from '@crayonai/react-ui';
import { useThreadManager } from '@crayonai/react-core'; // For state if needed

// Optional: Define custom response templates for structured UI (e.g., a chart)
const templates = [
  {
    name: 'breakdown_expenses',
    component: ({ data }) => <div>Custom Expense Chart: {JSON.stringify(data)}</div>, // Replace with real component like Recharts
  },
];

export default function App() {
  // Optional: Use hooks for advanced state management
  const thread = useThreadManager({ apiUrl: '/api/chat' });

  return (
    <ThemeProvider>
      <CrayonChat
        templates={templates}
        onSend={(message) => {
          // Send to backend via fetch or hook
          fetch('/api/chat', {
            method: 'POST',
            body: JSON.stringify({
              prompt: { role: 'user', content: message },
              threadId: 'example-thread',
              responseId: 'response-1',
            }),
          });
        }}
      />
    </ThemeProvider>
  );
}
```
Here, `<CrayonChat>` provides a ready-to-use chat UI. When a user types and sends a message, it's posted to the `/api/chat` endpoint. The streamed response is rendered in real-time, and if the LLM outputs structured data (e.g., for a template like "breakdown_expenses"), it renders as a custom component instead of text.

### How It Runs
1. User enters a message in the chat UI.
2. Frontend sends it to the API route.
3. Backend adds to storage, calls LLM, streams response.
4. Frontend receives stream events and updates the UI progressively (e.g., text appears as it's generated, or UI components render if templated).
5. Run with `npm run dev` and visit localhost:3000.

This is a minimal setup— for production, add persistent storage, error handling, and more templates for richer UIs. Crayon shines in turning plain LLM text into interactive elements, like auto-rendering a chart from a query about expenses.