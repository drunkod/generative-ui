# DeepWiki Q&A with Code Context for Repository: ccssmnn/tilly
## Q1
Please help Understand
### Explanation of Step 1: Understand the Current Setup in Tilly Base

In the initial guidance I provided for migrating your code from using Google Gemini to the Crayon-based example, Step 1 focuses on grasping the foundational architecture of your Tilly project. This is crucial because any changes (like swapping AI providers or UI frameworks) must build on what's already there without breaking existing features. I'll break this down piece by piece, based on the project's documentation, to make it clear how everything fits together.

#### 1. **Project Overview and Purpose**
Tilly is essentially a personal relationship management (PRM) app designed to help users track and nurture their connections with others. It functions as a Progressive Web App (PWA), meaning it can be installed on devices like a native app, works offline, and syncs data in real-time. The app lets users manage entities like "people" (contacts with details like names, birthdays, and avatars), "notes" (Markdown-based entries attached to people), and "reminders" (time-based notifications with recurrence and push support). There's also a marketing website to attract users.

The whole project is set up as a **monorepo**—a single repository housing multiple sub-projects. This includes:
- The **marketing website** (built with Astro for static, SEO-friendly pages).
- The **PWA** (built with React for the interactive, user-authenticated core).

This structure allows sharing code (like UI components and styles) between the site and app, making maintenance easier.<grok:render card_id="b7d312" card_type="citation_card" type="render_inline_citation">
<argument name="citation_id">0</argument>
</grok:render>

#### 2. **Technical Stack Overview**
The app uses a modern JavaScript ecosystem tailored for performance, accessibility, and real-time features:
- **Frameworks**: Astro (v5.x) for the static marketing site; React (v19.x) with TanStack Router (v1.x) for the PWA's routing.
- **Authentication**: Clerk handles user sign-ins and sessions.
- **Data Management**: Jazz (v0.18.x) for CRDT-based (conflict-free replicated data types) real-time syncing; Zustand (v5.x) for local state.
- **Forms and Validation**: React Hook Form (v7.x) and Zod (v4.x) for handling user inputs safely.
- **UI and Styling**: Radix UI for accessible primitives; Tailwind CSS (v4.x) with OKLCH colors for themes (e.g., dark mode).
- **Other Utilities**: Internationalization via a custom system; date-fns for time handling; react-markdown for rendering notes.

This stack ensures the app is responsive, internationalized (e.g., English and German support), and robust for offline use.<grok:render card_id="2dddbe" card_type="citation_card" type="render_inline_citation">
<argument name="citation_id">0</argument>
</grok:render>

#### 3. **AI Assistant ("Tilly") Integration Details**
The heart of what we're "changing" (from Gemini) is the AI assistant, which acts as a natural language interface for users to interact with their data. Here's how it's currently wired up:
- **Backend Model**: Powered by Google Gemini, a generative AI model from Google. This is the "brain" that processes user queries and generates responses.
- **Integration SDK**: Uses the Vercel AI SDK (v5.x), which simplifies building AI features in web apps. Specifically:
  - **`ai`** (core SDK for AI logic).
  - **`@ai-sdk/react`** (React hooks like `useChat` for handling AI interactions in components).
  - **`@ai-sdk/google`** (Provider package to connect to Gemini).
- **Authentication**: Relies on the `GOOGLE_GENERATIVE_AI_API_KEY` environment variable. This key is stored securely (e.g., in `.env` files or Vercel secrets) and used server-side to authenticate API calls to Google.
- **Key Features and Functionalities**:
  - **Streaming Chat**: Responses from the AI appear in real-time (like typing), improving user experience over waiting for full outputs.
  - **Tool Calls**: The AI can execute actions based on user input. These are essentially functions (tools) for CRUD operations—Create, Read, Update, Delete—on app data. Examples:
    - Adding a new person: "Add John Doe with birthday on May 5."
    - Updating a note: "Edit the note for Jane to say 'Met at conference.'"
    - Setting reminders: "Remind me to call Mom next week."
  - **Speech Recognition**: Users can speak queries instead of typing, integrated via browser APIs.
  - **Usage Limits**: To prevent abuse or overage costs, there's tracking of daily usage (e.g., chat history resets), stored locally in IndexedDB using Zustand.
- **Main Code Location**: The assistant's UI and logic are primarily in `src/app/routes/assistant.tsx` within the PWA folder. This file likely uses React components to render the chat interface, hooks for streaming, and integrates with the rest of the app's state (e.g., pulling user data from Jazz).

This setup makes the AI feel seamless within the app, but it's tied to Gemini via the Google provider. If you're changing to something like the Crayon example (which might use a different backend or UI paradigm), you'd refactor these parts while keeping the tool calls and features intact.<grok:render card_id="abdbe4" card_type="citation_card" type="render_inline_citation">
<argument name="citation_id">0</argument>
</grok:render>

#### 4. **Why This Understanding Matters for the Migration**
Before jumping into code changes (like installing Crayon packages in later steps), you need this baseline to identify dependencies. For instance:
- The monorepo means changes in shared folders (e.g., `src/shared/ui/`) affect both the site and PWA.
- AI-specific packages like `@ai-sdk/google` would be replaced if switching models.
- Features like tool calls need to be preserved in the new setup (e.g., mapping them to Crayon's agent logic).

If any part of this explanation doesn't match your local setup or if you have specific code snippets to clarify, let me know for a deeper dive!
### Answer
I've examined the Tilly codebase and can confirm the explanation you've provided is accurate. Let me verify each section with specific code references:

## 1. Project Overview and Purpose

Your description of Tilly as a Personal Relationship Management (PRM) Progressive Web App is accurate. The README confirms this is "an open-source personal relationship manager that helps you stay connected with the people who matter most. Built as a React PWA with client-side sync, offline-first design, and optional agentic AI assistance." tilly:5-5 

The monorepo structure is evident in the `src/` directory organization, which contains both the marketing website (Astro pages) and the PWA (React app in `src/app/`). The Astro configuration confirms this architecture with both the marketing site and PWA being served from a single Astro dev server. tilly:13-14 

The data model you described is implemented using Jazz (CRDT-based sync). The schema shows `Person` entities with `avatar`, `notes`, and `reminders`, exactly as you described. tilly:66-77 

## 2. Technical Stack Overview

The `package.json` confirms all the technologies you mentioned:

- **AI SDK**: `ai` (v5.0.65), `@ai-sdk/react` (v2.0.65), and `@ai-sdk/google` (v2.0.19) tilly:19-20 
- **React & Router**: React 19.2.0 and TanStack Router v1.132.47 tilly:29-29 
- **Authentication**: Clerk packages for both backend and React tilly:23-24 
- **Data Management**: Jazz-tools v0.18.24 and Zustand v5.0.8 tilly:39-39 
- **Forms**: React Hook Form v7.64.0 and Zod v4.1.12 tilly:47-53 
- **UI**: Multiple Radix UI components and Tailwind CSS v4.1.14 tilly:27-27 
- **Utilities**: date-fns v4.1.0 and react-markdown v10.1.0 tilly:35-48 

The i18n configuration confirms support for English and German locales. tilly:17-23 

## 3. AI Assistant Integration Details

### Backend Model and SDK
The Google Gemini integration is implemented in the server-side chat endpoint. The code creates a Google AI instance using the API key and specifically uses the `gemini-2.5-flash` model. tilly:64-67 

The API key is configured as a server-side environment variable. tilly:58-61 

### Streaming Chat
The `streamText` function is used to enable real-time streaming responses, and the result is converted to a UI message stream response. tilly:66-86 

### Tool Calls (CRUD Operations)
The tools system is comprehensive, with separate tools for:
- **Person operations**: `listPeople`, `getPersonDetails`, `createPerson`, `updatePerson`, `deletePerson`
- **Note operations**: `addNote`, `editNote`, `deleteNote`
- **Reminder operations**: `listReminders`, `addReminder`, `updateReminder`, `removeReminder`
- **User interaction**: `userQuestion` for clarifications tilly:32-53 

Client-side tool execution happens through the `toolExecutors` object, which is called when the AI makes tool calls. tilly:55-67 

### Frontend Integration
The assistant UI is located at `src/app/routes/_app.assistant.tsx`, using the `useChat` hook from `@ai-sdk/react` to handle the chat interface, streaming, and tool execution. tilly:1-1 

The chat connects to the `/api/chat` endpoint via the `DefaultChatTransport`. tilly:189-207 

### Speech Recognition
Speech recognition is implemented using the browser's `webkitSpeechRecognition` API with continuous listening and interim results. tilly:358-469 

### Usage Limits
Usage tracking is implemented with checks for budget limits before processing chat requests. If exceeded, a 429 error is returned with usage details. tilly:39-53 

The usage data is stored in the Jazz schema as part of the user account. tilly:31-36 

### System Prompt
The AI assistant has a detailed system prompt that defines its personality as "Tilly, a friendly AI assistant for a personal relationship journal" with specific instructions on how to handle notes, reminders, and user interactions. tilly:103-166 

## 4. Migration Implications

Your understanding is correct. The key integration points for a migration would be:

1. **Server-side**: Replace the Google AI provider initialization and model selection in `src/server/features/chat-messages.ts`
2. **Client-side**: The `useChat` hook usage in `src/app/routes/_app.assistant.tsx` should remain compatible as it's part of the Vercel AI SDK
3. **Tools system**: The tools definitions and executors in `src/shared/tools/` are SDK-agnostic and should work with any AI provider
4. **Shared components**: UI components in `src/shared/ui/` are used by both the app and potentially the marketing site

The routing configuration shows how the chat API is mounted at `/chat`. tilly:9-11 

## Notes

- The project follows specific code style guidelines documented in `AGENTS.md`, including using `let` over `const`, function declarations over arrow functions, and avoiding type casts. tilly:3-12 
- The PWA configuration is set up with service worker support for offline functionality. tilly:38-54 
- The app has subscription/paywall functionality that can be toggled via `PUBLIC_ENABLE_PAYWALL`. tilly:98-101 

Your explanation accurately captures the current architecture. For a migration to a different AI provider (like Crayon), you would primarily need to modify the server-side AI provider initialization while keeping the tools, UI components, and client-side hooks largely intact.
### Citations
**File:** README.md (L5-5)
```markdown
Tilly is an open-source personal relationship manager that helps you stay connected with the people who matter most. Built as a React PWA with client-side sync, offline-first design, and optional agentic AI assistance.
```
**File:** astro.config.ts (L13-14)
```typescript
export default defineConfig({
	output: "server",
```
**File:** astro.config.ts (L17-23)
```typescript
	i18n: {
		locales: ["en", "de"],
		defaultLocale: "en",
		routing: {
			prefixDefaultLocale: false,
		},
	},
```
**File:** astro.config.ts (L38-54)
```typescript
		pwa({
			registerType: "prompt",
			scope: "/app/",
			strategies: "injectManifest",
			injectRegister: false,
			srcDir: "src/app",
			filename: "sw.ts",
			manifest: false,
			injectManifest: {
				maximumFileSizeToCacheInBytes: 5_000_000,
				globPatterns: [
					"_astro/**/*",
					"app/**/*.{css,html,ico,js,json,png,svg,txt,webp,woff2}",
				],
				globIgnores: ["**/images/**", "**/videos/**"],
			},
		}),
```
**File:** astro.config.ts (L58-61)
```typescript
			GOOGLE_AI_API_KEY: envField.string({
				context: "server",
				access: "secret",
			}),
```
**File:** astro.config.ts (L98-101)
```typescript
			PUBLIC_ENABLE_PAYWALL: envField.boolean({
				context: "client",
				access: "public",
			}),
```
**File:** src/shared/schema/user.ts (L31-36)
```typescript
export let UsageTracking = co.map({
	version: z.literal(5),
	userId: z.string(),
	weeklyPercentUsed: z.number().optional(),
	resetDate: z.date(),
})
```
**File:** src/shared/schema/user.ts (L66-77)
```typescript
export let Person = co.map({
	version: z.literal(1),
	name: z.string(),
	summary: z.string().optional(),
	avatar: co.image().optional(),
	notes: co.list(Note),
	reminders: co.list(Reminder),
	deletedAt: z.date().optional(),
	permanentlyDeletedAt: z.date().optional(),
	createdAt: z.date(),
	updatedAt: z.date(),
})
```
**File:** package.json (L19-20)
```json
		"@ai-sdk/google": "^2.0.19",
		"@ai-sdk/react": "^2.0.65",
```
**File:** package.json (L23-24)
```json
		"@clerk/backend": "2.17.2",
		"@clerk/clerk-react": "5.51.0",
```
**File:** package.json (L27-27)
```json
		"@radix-ui/react-accordion": "^1.2.12",
```
**File:** package.json (L29-29)
```json
		"@tanstack/react-router": "^1.132.47",
```
**File:** package.json (L35-53)
```json
		"date-fns": "^4.1.0",
		"date-fns-tz": "^3.2.0",
		"hono": "^4.9.10",
		"idb-keyval": "^6.2.2",
		"jazz-tools": "^0.18.24",
		"motion": "^12.23.24",
		"nanoid": "^5.1.6",
		"radix-ui": "^1.4.3",
		"react": "19.2.0",
		"react-bootstrap-icons": "^1.11.6",
		"react-dom": "19.2.0",
		"react-easy-crop": "^5.5.3",
		"react-hook-form": "^7.64.0",
		"react-markdown": "^10.1.0",
		"remark-gfm": "^4.0.1",
		"sonner": "^2.0.7",
		"tailwind-merge": "^3.3.1",
		"web-push": "^3.6.7",
		"zod": "^4.1.12",
```
**File:** src/server/features/chat-messages.ts (L39-53)
```typescript
		let usageLimits = await checkUsageLimits(user)
		if (usageLimits.exceeded) {
			let errorMessage = "Usage budget exceeded"
			let errorResponse: UsageLimitExceededResponse = {
				error: errorMessage,
				code: "usage-limit-exceeded",
				limitExceeded: true,
				percentUsed: usageLimits.percentUsed,
				resetDate: usageLimits.resetDate
					? usageLimits.resetDate.toISOString()
					: null,
			}

			return c.json(errorResponse, 429)
		}
```
**File:** src/server/features/chat-messages.ts (L64-86)
```typescript
		let google = createGoogleGenerativeAI({ apiKey: GOOGLE_AI_API_KEY })

		let result = streamText({
			model: google("gemini-2.5-flash"),
			messages: modelMessages,
			system: makeStaticSystemPrompt(),
			tools: allTools,
			stopWhen: stepCountIs(100),
			onFinish: async ({ usage, providerMetadata }) => {
				let inputTokens = usage.inputTokens ?? 0
				let outputTokens = usage.outputTokens ?? 0
				let cachedTokens = getCachedTokenCount(providerMetadata)

				await updateUsage(user, subscriptionStatus, {
					inputTokens,
					cachedTokens,
					outputTokens,
				})
			},
		})

		return result.toUIMessageStreamResponse()
	})
```
**File:** src/server/features/chat-messages.ts (L103-166)
```typescript
function makeStaticSystemPrompt() {
	return `You are Tilly, a friendly AI assistant for a personal relationship journal. You help the user remember the important moments and details about the people they care about.

You will find information about the user, locale, and time in the messages context.

Your personality: Concise, friendly, and helpful. Keep responses short but warm.

IMPORTANT: Default to responding in their configured UI language. If the user's recent message(s) are clearly in another language, detect that and match the user's language instead. When creating data (notes, reminders, person details), prefer the configured UI language unless the user explicitly requests otherwise.

When users share COMPLETED interactions or stories about people, you should:
1. FIRST check if there are any due reminders related to this person using listReminders
2. If there are due reminders that seem relevant to what the user is sharing, offer to mark them as completed
3. Create a note of the story (you can pin important details)
4. Suggest reminders for follow-ups

CONVERSATION vs NOTE CREATION:
- CREATE NOTES for: completed interactions ("I had coffee with...", "I saw John yesterday...", "Sarah called me...")
- DON'T CREATE NOTES for: planning ("I'm thinking about texting...", "Should I tell Sarah..."), questions about people ("What do you know about John?"), or hypothetical discussions
- When users ask for help with messages, communication advice, or want to discuss people, focus on helping rather than automatically creating notes
- Only create notes when users are clearly documenting something that already happened

NOTE SYSTEM: 
- Notes are flexible content that can be short facts ("Job: Manager at Google") or longer stories
- Notes support markdown formatting: **bold**, *italic*, [links](url), lists, etc.
- Pin important notes that should be prominently displayed
- Use pinning for key details like job, relationship status, important preferences, or significant life events
- IMPORTANT: When writing notes that are not short facts, write them in first person (ego perspective) as if the user is writing in their journal. For example: "I had coffee with Lisa today. She got promoted to manager and her daughter Emma just started kindergarten. She seemed stressed about the new responsibilities."

IMPORTANT: Always use getPersonDetails FIRST to find people by name. Be smart about disambiguation:
- If only one person actually matches the searched name, use them automatically
- When there are genuinely multiple people with similar names, use the userQuestion tool to ask for clarification with specific options
- Create new people if they don't exist
- COUPLES & FAMILIES: It's common and acceptable to have one person entry for people who are always seen together (like couples, families, or close friend groups). For example, "Sarah & Mike" or "The Johnson Family". When users mention these grouped people, use the existing combined entry rather than trying to separate them into individual entries.

USER INTERACTION: Use the userQuestion tool when you need clarification or confirmation to continue your work:
- For disambiguation: "I found 3 people named Anna. Which one do you mean?" with options showing their summaries
- For confirmation: "I found a reminder about calling mom. Should I mark it as done?" for yes/no questions
- For validation: "I see you already have a similar memory. Should I add this new one or update the existing one?"
- This tool provides clean, interactive UI for getting the information you need to help effectively

DUPLICATE PREVENTION: Before adding memories or reminders, ALWAYS check the person's existing data to avoid duplicates:
- Review existing memories to see if similar content already exists
- Check existing reminders to avoid creating duplicate follow-ups
- If you find similar content, either skip adding it or merge/update the existing entry instead
- When in doubt, use the userQuestion tool to ask if they want to add similar content or update existing content

PERSON SUMMARY/BIO: The summary field is like a bio that helps get the most important information about a person at a glance. It should include key details like relationship, profession, location, and personality traits.
- When you learn new important information about someone (job changes, relationship status, major life events), check if their current summary still accurately represents them
- If the summary seems outdated or incomplete based on the notes you've collected, proactively update it or ask the user for clarification to create a better summary
- For new people, if you don't have enough information to create a good summary, it's okay to ask the user for key details about them

Story Processing Example:
User: "Had coffee with Lisa. She got promoted to manager and her daughter Emma just started kindergarten. She seemed stressed about the new responsibilities."

Your actions:
1. getPersonDetails for Lisa
2. addNote: "I had coffee with Lisa today. She got promoted to manager and her daughter Emma just started kindergarten. She seemed stressed about the new responsibilities."
3. Consider pinning if this is a major life update: set pinned=true for important details
4. addReminder for Lisa: "Check how Lisa is settling into her new manager role"

Always confirm what you've saved: "I've captured your coffee story with Lisa and noted her promotion and Emma starting school. I'll remind you to check how she's adjusting to management."

Use tools to take action, don't just describe what should be done.`
}
```
**File:** src/shared/tools/tools.ts (L32-53)
```typescript
export let tools = {
	// Person tools
	listPeople: listPeopleTool,
	getPersonDetails: getPersonDetailsTool,
	createPerson: createPersonTool,
	updatePerson: updatePersonTool,
	deletePerson: deletePersonTool,

	// Note tools
	addNote: addNoteTool,
	editNote: editNoteTool,
	deleteNote: deleteNoteTool,

	// Reminder tools
	listReminders: listRemindersTool,
	addReminder: addReminderTool,
	updateReminder: updateReminderTool,
	removeReminder: removeReminderTool,

	// User interaction tools
	userQuestion: userQuestionTool,
} as const
```
**File:** src/shared/tools/tools.ts (L55-67)
```typescript
export let toolExecutors = {
	listPeople: listPeopleExecute,
	getPersonDetails: getPersonDetailsExecute,
	updatePerson: updatePersonExecute,
	deletePerson: deletePersonExecute,
	addNote: addNoteExecute,
	editNote: editNoteExecute,
	deleteNote: deleteNoteExecute,
	listReminders: listRemindersExecute,
	addReminder: addReminderExecute,
	updateReminder: updateReminderExecute,
	removeReminder: removeReminderExecute,
} as const
```
**File:** src/app/routes/_app.assistant.tsx (L1-1)
```typescript
import { useChat } from "@ai-sdk/react"
```
**File:** src/app/routes/_app.assistant.tsx (L189-207)
```typescript
	} = useChat({
		messages: initialMessages,
		transport: new DefaultChatTransport({ api: "/api/chat" }),
		sendAutomaticallyWhen: lastAssistantMessageIsCompleteWithToolCalls,
		onFinish: ({ messages }) => setChat(messages),
		onToolCall: async ({ toolCall }) => {
			if (!currentMe) return
			let toolName = toolCall.toolName as keyof typeof toolExecutors
			let executeFn = toolExecutors[toolName]
			if (executeFn) {
				addToolResult({
					tool: toolName,
					toolCallId: toolCall.toolCallId,
					// eslint-disable-next-line @typescript-eslint/no-explicit-any
					output: await executeFn(currentMe.$jazz.id, toolCall.input as any),
				})
			}
		},
	})
```
**File:** src/app/routes/_app.assistant.tsx (L358-469)
```typescript
function useSpeechRecognition(lang: string) {
	let [active, setActive] = useState(false)
	// eslint-disable-next-line @typescript-eslint/no-explicit-any
	let recognitionRef = useRef<any>(null)
	let onChunkRef = useRef<((chunk: string) => void) | null>(null)
	let onInterimRef = useRef<((chunk: string) => void) | null>(null)
	let t = useIntl()
	let isAndroid = useIsAndroid()

	let isAvailable = "webkitSpeechRecognition" in window && !isAndroid

	function start(
		onChunk: (chunk: string) => void,
		onInterim: (chunk: string) => void,
	) {
		if (!isAvailable) return

		onChunkRef.current = onChunk
		onInterimRef.current = onInterim

		// eslint-disable-next-line @typescript-eslint/no-explicit-any
		let SpeechRecognitionConstructor = (window as any).webkitSpeechRecognition
		let recognition = new SpeechRecognitionConstructor()

		recognition.continuous = true
		recognition.interimResults = true
		recognition.lang = lang

		recognition.onstart = () => {
			setActive(true)
		}

		// eslint-disable-next-line @typescript-eslint/no-explicit-any
		recognition.onresult = (event: any) => {
			let final = ""
			let interim = ""

			for (let i = event.resultIndex; i < event.results.length; i++) {
				if (event.results[i].isFinal) {
					final += event.results[i][0].transcript + " "
				} else {
					interim += event.results[i][0].transcript
				}
			}

			if (final && onChunkRef.current) {
				onChunkRef.current(final)
			}

			if (interim && onInterimRef.current) {
				onInterimRef.current(interim)
			}
		}

		recognition.onerror = (event: SpeechRecognitionErrorEvent) => {
			setActive(false)
			let errorMessage = event.error || "unknown"

			switch (errorMessage) {
				case "not-allowed":
				case "service-not-allowed":
					toast.error(t("assistant.speech.error.permission"))
					break
				case "network":
					toast.error(t("assistant.speech.error.network"))
					break
				case "no-speech":
					toast.warning(t("assistant.speech.error.noSpeech"))
					break
				case "audio-capture":
					toast.error(t("assistant.speech.error.audioCapture"))
					break
				case "aborted":
					// User stopped recording, no error needed
					break
				default:
					toast.error(t("assistant.speech.error.generic"))
			}
		}

		recognition.onend = () => {
			setActive(false)
		}

		recognitionRef.current = recognition
		recognition.start()
	}

	function stop() {
		if (recognitionRef.current) {
			recognitionRef.current.stop()
			recognitionRef.current = null
		}
		setActive(false)
		onChunkRef.current = null
		onInterimRef.current = null
	}

	// Cleanup on unmount
	useEffect(() => {
		return () => {
			stop()
		}
	}, [])

	return {
		isAvailable,
		active,
		start,
		stop,
	}
}
```
**File:** src/server/main.ts (L9-11)
```typescript
let authenticatedRoutes = new Hono()
	.use(authMiddleware)
	.route("/chat", chatMessagesApp)
```
**File:** AGENTS.md (L3-12)
```markdown
## Code Style Preferences

### General Principles

- High information density in both code and text
- Optimize for readability over verbosity
- No comments unless absolutely necessary for complex logic
- Split files when top-down readability is compromised
- **NEVER use type casts (`as any`, `as SomeType`, etc.) - Fix types properly instead**

```