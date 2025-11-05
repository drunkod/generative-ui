### Detailed Migration Guide: Integrating Crayon SDK into Tilly (Switching from Gemini to OpenAI/Crayon)

This guide assumes you're migrating the AI assistant ("Tilly") in your codebase (ccssmnn/tilly) to use the Crayon Generative UI SDK, inspired by the Fino-Crayon example you provided. Fino-Crayon is a Next.js-based finance assistant using OpenAI for the backend, Prisma for persistence, and Crayon for agentic UIs with custom response templates (e.g., charts, consent forms).

#### Key Assumptions and Scope
- **Goal**: Refactor Tilly's assistant to use Crayon for dynamic UI rendering (e.g., templates for people/notes/reminders breakdowns), switch the AI backend from Google Gemini (via Vercel AI SDK) to OpenAI, and adapt tools/CRUD operations. Preserve Tilly's core features like offline sync, speech recognition, and usage limits.
- **Challenges**:
  - Framework: Tilly uses Astro (marketing) + React PWA (app) monorepo; Fino-Crayon uses Next.js. We'll integrate into Tilly's React PWA without switching frameworks.
  - Data: Tilly uses Jazz (CRDT for real-time/offline sync); Fino uses Prisma (SQLite). We'll keep Jazz for app data; use local storage or Jazz for chat persistence (instead of Prisma).
  - AI: Switch from Gemini to OpenAI; adapt Vercel AI's `useChat` to Crayon's hooks/components.
  - UI: Tilly uses Radix UI + Tailwind; Fino uses Crayon UI. We'll layer Crayon on top, customizing with Tailwind.
- **Prerequisites**:
  - Backup your repo: `git commit -m "Pre-Crayon migration backup"`.
  - Node.js >=18, pnpm/yarn/npm.
  - Add OpenAI API key to `.env`: `OPENAI_API_KEY=sk-...`.
  - Test locally with `npm run dev` after each step.
- **Non-Goals**: Full rewrite to Next.js; migrating non-assistant features (e.g., marketing site remains Astro).

The migration is divided into phases. Each includes code diffs/examples tailored to Tilly's structure.

---

### Phase 1: Install Dependencies and Setup Environment
Update Tilly's `package.json` to include Crayon and OpenAI packages from Fino-Crayon.

1. **Install Packages**:
   ```
   npm install @crayonai/react-core@^0.7.4 @crayonai/react-ui@^0.7.9 @crayonai/stream@^0.6.4 openai@^4.81.0 zod-to-json-schema@^3.24.1 best-effort-json-parser@^1.1.2 tiny-invariant@^1.3.3 react-markdown@^9.0.3
   ```
   - These match Fino-Crayon's deps for Crayon, OpenAI, and utilities.
   - Remove or keep Vercel AI/Google packages; we'll phase them out.

2. **Update Environment Variables**:
   In `.env` and Vercel deployment:
   ```
   OPENAI_API_KEY=sk-...
   # Remove GOOGLE_GENERATIVE_AI_API_KEY if fully switching
   ```

3. **Tailwind Config Alignment**:
   Tilly uses Tailwind v4; Fino uses v3. Update Tilly's `tailwind.config.ts` to include Fino's content paths if needed (e.g., for Crayon styles):
   ```ts
   // tailwind.config.ts (add to content)
   content: [
     './src/**/*.{astro,js,jsx,ts,tsx}',
     // Add for Crayon
     './node_modules/@crayonai/react-ui/**/*.{js,ts,jsx,tsx}',
   ],
   ```
   Import Crayon styles in Tilly's global CSS (e.g., `src/app/globals.css` or equivalent):
   ```css
   @import '@crayonai/react-ui/styles/index.css';
   ```

---

### Phase 2: Refactor AI Backend (Switch Gemini to OpenAI)
Tilly's backend is in `src/server/features/chat-messages.ts` (Hono API). Fino's is in `src/app/api/ask/route.ts` (Next.js API route with OpenAI streaming).

1. **Create OpenAI Instance**:
   In `src/server/features/chat-messages.ts`, replace Gemini with OpenAI:
   ```diff
   -import { createGoogleGenerativeAI, streamText } from 'ai';
   +import OpenAI from 'openai';
   +import { fromOpenAICompletion } from '@crayonai/stream'; // For streaming compatibility

   // ...
   -const google = createGoogleGenerativeAI({ apiKey: process.env.GOOGLE_GENERATIVE_AI_API_KEY });
   +const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });

   -const result = await streamText({
   -  model: google('gemini-1.5-flash'),
   +const completion = await openai.beta.chat.completions.runTools({
   +  model: 'gpt-4o-mini', // Or 'gpt-4o' for better quality
      messages: [...systemPrompt, ...modelMessages],
   +  temperature: 0.7,
   +  stream: true,
   +  tools: allTools, // Adapt tools (see below)
   +  response_format: { type: 'json_object' }, // For structured outputs like Fino
   +});

   +// Use Crayon's stream adapter for Vercel AI compatibility
   +const stream = fromOpenAICompletion(completion, { onFinish: handleUsage });

   -return result.toUIMessageStreamResponse();
   +return new Response(stream, { // Adapt to Hono response
   +  headers: {
   +    'Content-Type': 'text/event-stream',
   +    'Cache-Control': 'no-cache',
   +  },
   +});
   ```

2. **Adapt System Prompt**:
   Fino's prompt is concise and structured for templates. Update Tilly's `makeStaticSystemPrompt()` to match:
   ```ts
   const SYSTEM_PROMPT = `You are Tilly, a friendly AI PRM assistant. ... (keep core logic)
   IMPORTANT: Respond in JSON with { response: [text or template objects] } like Fino. Use templates for breakdowns.`;
   ```

3. **Tool Adaptation**:
   Tilly's tools (e.g., `listPeopleTool`) are Vercel AI format. Fino uses Zod schemas for OpenAI tools.
   - In `src/shared/tools/tools.ts`, refactor each tool:
     ```ts
     import { z } from 'zod';

     // Example for listPeople
     const ListPeopleSchema = z.object({ query: z.string().optional() });
     export const listPeopleTool = {
       type: 'function',
       function: {
         name: 'list_people',
         description: 'List people...',
         parameters: zodToJsonSchema(ListPeopleSchema),
         parse: (input: string) => ListPeopleSchema.parse(JSON.parse(input)),
         function: listPeopleExecute, // Your existing executor
       }
     };
     ```
   - Update `allTools` array to include these.

4. **Usage Tracking**:
   Fino doesn't have limits; keep Tilly's `checkUsageLimits` and adapt `onFinish` callback for token counting.

---

### Phase 3: Integrate Crayon UI and Hooks in Client-Side Assistant
Tilly's UI is in `src/app/routes/_app.assistant.tsx` using `useChat`. Replace with Crayon's hooks/components like in Fino's `src/app/page.tsx`.

1. **Import Crayon Components**:
   ```ts
   import { ChatProvider, useThreadManager, useThreadListManager, processStreamedMessage } from '@crayonai/react-core';
   import { Container, Messages, Composer, ... } from '@crayonai/react-ui/Shell'; // As in Fino
   ```

2. **Refactor Assistant Component**:
   Replace `useChat` with Crayon managers. Adapt to Tilly's state (Zustand for chat, Jazz for data).
   ```diff
   // src/app/routes/_app.assistant.tsx
   -const { messages, input, handleInputChange, handleSubmit, isLoading } = useChat(...);
   +const threadListManager = useThreadListManager({
   +  fetchThreadList: async () => [], // No threads initially; or load from Zustand/Jazz
   +  createThread: async (firstMessage) => ({ threadId: crypto.randomUUID(), title: firstMessage.message }),
   +  // Stub others
   +});

   +const threadManager = useThreadManager({
   +  threadId: threadListManager.selectedThreadId,
   +  loadThread: async () => useAppStore.getState().chat, // From Zustand
   +  onProcessMessage: async ({ message, threadManager, abortController }) => {
   +    // Append user message
   +    threadManager.appendMessages({ role: 'user', message: message.message });
   +    const response = await fetch('/api/chat/messages', { // Your existing endpoint
   +      method: 'POST',
   +      body: JSON.stringify(message),
   +      signal: abortController.signal,
   +    });
   +    await processStreamedMessage({
   +      response,
   +      createMessage: threadManager.appendMessages,
   +      updateMessage: threadManager.updateMessage,
   +    });
   +    // Save to Zustand
   +    useAppStore.setState({ chat: threadManager.messages });
   +  },
   +  responseTemplates: templates, // Define below
   +});

   return (
   -  <div>Custom chat UI</div>
   +  <ChatProvider threadListManager={threadListManager} threadManager={threadManager}>
   +    <Container logoUrl="/tilly-logo.png" agentName="Tilly">
   +      {/* Sidebar if needed */}
   +      <ThreadContainer>
   +        <ScrollArea><Messages /></ScrollArea>
   +        <Composer /> {/* Handles input/submit */}
   +      </ThreadContainer>
   +    </Container>
   +  </ChatProvider>
   );
   ```

3. **Integrate Speech Recognition**:
   Attach Tilly's `useSpeechRecognition` to Crayon's Composer input.

---

### Phase 4: Define Custom Response Templates
Fino uses templates for finance viz (e.g., `breakdown_expenses`). Create Tilly-specific ones (e.g., for people breakdowns).

1. **Create Templates Folder**:
   In `src/app/responseTemplates/` (mimic Fino):
   - `templates.ts`: Define array like Fino's, but for Tilly (e.g., PersonBreakdown component using Radix UI).
     ```ts
     import { ResponseTemplate } from '@crayonai/react-core';
     // Example template
     const PersonBreakdown = ({ people }) => <Card>{/* Render list */}</Card>;
     export const templates: ResponseTemplate[] = [
       { name: 'person_breakdown', Component: PersonBreakdown },
       // Add more for notes, reminders
     ];
     ```
   - Schemas in `src/types/responseTemplates/` (Zod like Fino).

2. **Update Backend for Templates**:
   In chat endpoint, enforce JSON response with templates (like Fino's SYSTEM_MESSAGE).

---

### Phase 5: Handle Tools and Persistence
1. **Tools**:
   Adapt Tilly's CRUD tools to OpenAI format (Phase 2). Execute client-side like Tilly.

2. **Chat Persistence**:
   Fino uses Prisma for threads/messages. Tilly: Use Zustand/Jazz instead.
   - In `useThreadManager.loadThread`: Load from Zustand.
   - On finish: Save to Zustand.

3. **Usage Limits**:
   Keep Tilly's logic; integrate into OpenAI's `onFinish`.

---

### Phase 6: Testing and Deployment
1. **Local Test**: Run `npm run dev`. Test chat, tools, templates.
2. **Edge Cases**: Offline mode (Jazz), speech, limits.
3. **Deploy**: Update Vercel env vars. Test PWA install.
4. **Cleanup**: Remove unused Vercel AI/Google deps.

If issues arise (e.g., streaming mismatches), provide logs for refinements. This keeps Tilly's strengths while adding Crayon's agentic UIs!