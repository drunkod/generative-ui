# DeepWiki Q&A with Code Context for Repository: ccssmnn/tilly
## Q1
Please analyze my code repository and list ALL dependencies used in the project for functionalyty for the AI Assistant (Tilly) 

Here's a well-structured prompt in English for finding and replacing all AI Assistant (Tilly) related functionality:

## Prompt:

```
I need to find and replace ALL references to the AI Assistant "Tilly" in my codebase with a new assistant. Please help me:

1. **SEARCH AND IDENTIFY all files containing:**
   - Direct references to "Tilly" (case-insensitive)
   - Variable names containing "tilly"
   - Class/function names with "tilly"
   - Configuration keys/values mentioning "tilly"
   - Comments and documentation mentioning "Tilly"
   - String literals with "Tilly"
   - File names containing "tilly"
   - Directory names with "tilly"

2. **SCAN these file types specifically:**
   - Source code files (.js, .ts, .py, .java, .go, etc.)
   - Configuration files (.json, .yaml, .yml, .env, .config)
   - Documentation (.md, .txt, .rst)
   - Test files (*test*, *spec*)
   - Docker files and compose files
   - CI/CD pipeline files
   - Database migration/seed files
   - Localization/translation files

3. **PROVIDE OUTPUT as:**
   - Full file path
   - Line numbers where "Tilly" appears
   - Context (3 lines before and after)
   - Type of reference (variable/function/string/comment)
   - Suggested replacement pattern

4. **CATEGORIZE findings by:**
   - Critical code references (must replace)
   - Configuration references
   - Documentation/comments
   - Test files
   - Assets/resources

5. **ADDITIONALLY, identify:**
   - Hardcoded values related to Tilly
   - API endpoints specific to Tilly
   - Database fields/tables with Tilly references
   - External service integrations
   - Environment variables
   - Log messages mentioning Tilly

Please provide a systematic replacement strategy and highlight any potential breaking changes or dependencies that need special attention during migration.

Format: [FilePath] : [LineNumber] : [ReferenceType] : [Current Code] -> [Suggested Replacement]
```
### Answer
# Comprehensive Analysis: Tilly AI Assistant Dependencies & References

## 1. AI Assistant Dependencies

### Core AI/ML Dependencies

**AI SDK Integration:**
- `ai` (v5.0.65) - Vercel AI SDK for streaming and tool execution
- `@ai-sdk/google` (v2.0.19) - Google Gemini provider for AI SDK
- `@ai-sdk/react` (v2.0.65) - React hooks for AI interactions tilly:18-30 

**Model Configuration:**
- Uses Google Gemini `gemini-2.5-flash` model for main chat assistant
- Uses `gemini-2.0-flash-exp` for translation tasks tilly:64-67 

### Authentication & Storage Dependencies

**Authentication:**
- `@clerk/backend` (2.17.2) - Server-side authentication
- `@clerk/clerk-react` (5.51.0) - Client-side authentication

**Distributed Database:**
- `jazz-tools` (v0.18.24) - Client-side encrypted, distributed database for storing user data, people, notes, and reminders tilly:23-39 

### Supporting Libraries

**State Management & Storage:**
- `zustand` (v5.0.8) - Client-side state management for chat history
- `idb-keyval` (v6.2.2) - IndexedDB wrapper for persistent storage

**Push Notifications:**
- `web-push` (v3.6.7) - Server-side push notification delivery

**UI & Form Management:**
- `react-hook-form` (v7.64.0) - Form handling for chat input
- `@hookform/resolvers` (v5.2.2) - Form validation
- `react-markdown` (v10.1.0) - Markdown rendering for AI responses
- `remark-gfm` (v4.0.1) - GitHub Flavored Markdown support

**Date/Time:**
- `date-fns` (v4.1.0) - Date manipulation
- `date-fns-tz` (v3.2.0) - Timezone support for user context

**Validation:**
- `zod` (v4.1.12) - Schema validation for messages and tools tilly:35-54 

### Environment Configuration

**Required API Keys & Secrets:**
- `GOOGLE_AI_API_KEY` - Google Gemini API key (server-side secret)
- `CLERK_SECRET_KEY` - Clerk authentication secret
- `PUBLIC_CLERK_PUBLISHABLE_KEY` - Clerk public key
- `PUBLIC_JAZZ_SYNC_SERVER` - Jazz sync server URL
- `JAZZ_WORKER_SECRET` - Jazz worker authentication
- `VAPID_PRIVATE_KEY` / `PUBLIC_VAPID_KEY` - Web push notification keys
- `CRON_SECRET` - Scheduled jobs authentication

**Usage Control Variables:**
- `WEEKLY_BUDGET` - Token budget per week
- `INPUT_TOKEN_COST_PER_MILLION` - Cost calculation
- `CACHED_INPUT_TOKEN_COST_PER_MILLION` - Cached token cost
- `OUTPUT_TOKEN_COST_PER_MILLION` - Output token cost
- `MAX_REQUEST_TOKENS` - Request size limit tilly:56-127 

---

## 2. Complete "Tilly" Reference Analysis

### A. CRITICAL CODE REFERENCES (Must Replace)

#### 1. AI System Prompt
**[src/server/features/chat-messages.ts]** : Lines 103-165 : **System Prompt** : Core AI personality definition tilly:103-165 

#### 2. Chat Message Types
**[src/shared/tools/tools.ts]** : Line 87 : **Type Definition** : `TillyUIMessage` type tilly:86-87 

#### 3. State Management
**[src/app/lib/store.ts]** : Lines 6-17 : **Type & Variable** : Chat state using `TillyUIMessage[]` tilly:6-17 

**[src/app/lib/store.ts]** : Line 193 : **Storage Key** : IndexedDB storage key `"tilly-app-storage"` tilly:193-193 

#### 4. Service Worker Caching
**[src/app/sw.ts]** : Lines 26-28 : **Cache Names** : Service worker cache identifiers tilly:26-28 

**[src/app/sw.ts]** : Lines 213-220 : **Default Notification** : Push notification defaults tilly:213-220 

#### 5. Component References
**[src/app/routes/_app.assistant.tsx]** : Line 273 : **Avatar Image** : Logo reference in chat interface tilly:270-277 

**[src/app/routes/index.tsx]** : Line 68 : **App Title** : Welcome screen title tilly:68-69 

#### 6. Push Notification Configuration
**[src/server/features/push-shared.ts]** : Line 27 : **VAPID Email** : Push notification contact email tilly:27-30 

---

### B. CONFIGURATION REFERENCES

#### 1. PWA Manifest
**[public/app/manifest.json]** : Lines 1-4 : **App Metadata** : PWA name and description tilly:1-4 

#### 2. Package Configuration
**[package.json]** : Line 2 : **Package Name** : NPM package identifier tilly:2-2 

---

### C. DOCUMENTATION & CONTENT

#### 1. README & Project Documentation
**[README.md]** : 19 matches throughout file : **Multiple contexts** : Project description, features, examples tilly:1-7 

#### 2. Internationalization Messages

**Assistant Messages (English):**
**[src/shared/intl/messages.assistant.ts]** : 16 matches : **UI Strings** : Chat interface text, placeholders, tool confirmations tilly:5-38 

**Assistant Messages (German):** tilly:206-239 

**Tool Confirmation Messages:** tilly:70-74 

**Settings Messages:**
**[src/shared/intl/messages.settings.ts]** : 71 matches : **Settings UI** : Subscription, features, plans

**People Messages:**
**[src/shared/intl/messages.people.ts]** : 8 matches : **People UI** : Empty states, descriptions

**Tour Messages:**
**[src/shared/intl/messages.tour.ts]** : 10 matches : **Tour UI** : Onboarding content

**Reminders Messages:**
**[src/shared/intl/messages.reminders.ts]** : 4 matches : **Reminders UI** : Empty states

**UI Messages:**
**[src/shared/intl/messages.ui.ts]** : 8 matches : **General UI** : Navigation, buttons, labels

#### 3. Marketing Pages (German)
**[src/pages/de/index.astro]** : 61 matches : **Marketing Content** : Hero section, features, testimonials tilly:16-24 tilly:46-52 

#### 4. Marketing Pages (English)
**[src/pages/en/index.astro]** : 57 matches : **Marketing Content** : Similar structure to German version

#### 5. Blog Posts
**[src/content/blog/self-hosting/en.md]** : 17 matches : **Tutorial Content** : Self-hosting guide
**[src/content/blog/self-hosting/de.md]** : 17 matches : **Tutorial Content** : German self-hosting guide
**[src/content/blog/pragmatic-relationship-journaling/en.md]** : 2 matches : **Blog Content** : Product mention
**[src/content/blog/pragmatic-relationship-journaling/de.md]** : 2 matches : **Blog Content** : Product mention

#### 6. Blog Index Pages
**[src/pages/de/blog/index.astro]** : 12 matches : **Blog Layout** : Title, metadata
**[src/pages/en/blog/index.astro]** : 12 matches : **Blog Layout** : Title, metadata
**[src/pages/de/blog/[...slug].astro]** : 9 matches : **Blog Post Template**
**[src/pages/en/blog/[...slug].astro]** : 9 matches : **Blog Post Template**

#### 7. Legal Pages
**[src/pages/de/privacy.md]** : 26 matches : **Privacy Policy** : Service name references
**[src/pages/en/privacy.md]** : 26 matches : **Privacy Policy** : Service name references
**[src/pages/de/imprint.md]** : 2 matches : **Imprint** : Website name
**[src/pages/en/imprint.md]** : 2 matches : **Imprint** : Website name

**[src/www/layouts/LegalPageLayout.astro]** : 3 matches : **Legal Layout** : Page titles

#### 8. Additional Pages
**[src/pages/app/[...all].astro]** : 3 matches : **App Shell** : Page titles
**[src/pages/index.astro]** : 3 matches : **Root Redirect** : Title

---

### D. ASSET & RESOURCE REFERENCES

#### 1. ASCII Art
**[tilly-ascii-art.txt]** : **Visual Asset** : ASCII logo representation

#### 2. Mock Data Templates
**[scripts/data/mock-data-template.json]** : 1 match : **Test Data** : Sample assistant interaction
**[scripts/data/mock-data-template.de.json]** : 1 match : **Test Data** : German sample data

#### 3. Component Files
**[src/app/features/assistant-message-components.tsx]** : 5 matches : **Type References** : `TillyUIMessage` usage tilly:4-6 

#### 4. Script Files
**[scripts/og.tsx]** : 2 matches : **OG Image Generator** : Meta image title
**[scripts/README.md]** : 1 match : **Script Documentation**

#### 5. Error Handling
**[src/app/components/main-error-boundary.tsx]** : 1 match : **Error Message**
**[src/app/components/splash-screen.tsx]** : 1 match : **Loading Screen**

#### 6. Data Schemas
**[src/app/features/data-file-schema.ts]** : 1 match : **Export Schema**
**[src/app/features/data-upload-button.tsx]** : 1 match : **Upload Validation**
**[src/app/features/data-download-button.tsx]** : 2 matches : **Download Filename** tilly:106-117 

#### 7. Tour Components
**[src/app/routes/tour.tsx]** : 1 match : **Tour Content**

---

## 3. Systematic Replacement Strategy

### Phase 1: Define Replacement Names
```
OLD_NAME: Tilly
OLD_NAME_LOWER: tilly
OLD_PACKAGE: @ccssmnn/tilly

NEW_NAME: [YourAssistantName]
NEW_NAME_LOWER: [yourassistantname]
NEW_PACKAGE: @[yourorg]/[yourassistantname]
```

### Phase 2: Critical Code Changes (Breaking Changes)

1. **System Prompt** - Update AI personality in `src/server/features/chat-messages.ts` (line 104)
2. **Type Definitions** - Rename `TillyUIMessage` type globally
3. **Storage Keys** - Change `tilly-app-storage` (requires data migration or users lose chat history)
4. **Cache Names** - Update service worker cache names (requires cache invalidation)
5. **Package Name** - Update `package.json` name field

### Phase 3: Configuration Updates (Non-Breaking)

1. **PWA Manifest** - Update app name and short_name
2. **VAPID Email** - Update contact email in push configuration
3. **Notification Defaults** - Update default notification text

### Phase 4: Content & Documentation (Non-Breaking)

1. **Internationalization Files** - Update all message strings in `src/shared/intl/messages.*.ts`
2. **Marketing Pages** - Update landing pages (de/en)
3. **Blog Posts** - Update all blog content references
4. **Legal Pages** - Update privacy policy, imprint
5. **README** - Update project documentation
6. **ASCII Art** - Replace or remove branding asset

### Phase 5: Asset Updates (Non-Breaking)

1. **Mock Data** - Update test data templates
2. **OG Images** - Generate new social preview images
3. **Component References** - Update avatar/logo references
4. **Error Messages** - Update user-facing error text

---

## 4. Migration Risks & Special Considerations

### High Risk (Data Loss Potential)
- **Storage Key Change**: Changing `tilly-app-storage` will cause users to lose chat history unless you implement migration logic
- **Cache Name Change**: Service worker cache invalidation may cause offline functionality issues during transition

### Medium Risk (User Experience)
- **System Prompt**: Changing AI personality may affect user expectations and tool behavior
- **Notification Text**: Users with cached notifications will see old branding
- **PWA Name**: Users will see new name after PWA update, may cause confusion

### Low Risk (Cosmetic)
- Content updates (documentation, marketing, blog posts)
- Asset replacements (images, ASCII art)
- Internationalization string updates

---

## Notes

### Complete File List with Tilly References (44 files total):

**High Priority (Code):**
1. `src/server/features/chat-messages.ts` - AI system prompt and streaming
2. `src/shared/tools/tools.ts` - Type definitions
3. `src/app/lib/store.ts` - State management and storage
4. `src/app/sw.ts` - Service worker configuration
5. `src/app/routes/_app.assistant.tsx` - Main chat interface
6. `src/server/features/push-shared.ts` - Push notifications
7. `src/app/features/assistant-message-components.tsx` - Message rendering

**Medium Priority (Configuration & Assets):**
8. `package.json` - Package identifier
9. `public/app/manifest.json` - PWA metadata
10. `tilly-ascii-art.txt` - Branding asset

**Lower Priority (Content - 34 files):**
- 8 internationalization files (`src/shared/intl/messages.*.ts`)
- 4 marketing pages (`src/pages/*/index.astro`)
- 4 blog index pages
- 4 blog post files
- 4 legal pages
- 10 additional component/utility files

The most critical dependencies for AI functionality are the AI SDK packages (`ai`, `@ai-sdk/google`, `@ai-sdk/react`), authentication (`@clerk/*`), distributed database (`jazz-tools`), and supporting libraries for state management, validation, and push notifications. The Google Gemini API is the core AI provider with usage controlled through environment variables for token budgets and cost calculations.
### Citations
**File:** package.json (L2-2)
```json
	"name": "@ccssmnn/tilly",
```
**File:** package.json (L18-54)
```json
	"dependencies": {
		"@ai-sdk/google": "^2.0.19",
		"@ai-sdk/react": "^2.0.65",
		"@astrojs/vercel": "^8.2.9",
		"@ccssmnn/intl": "^0.1.1",
		"@clerk/backend": "2.17.2",
		"@clerk/clerk-react": "5.51.0",
		"@hono/zod-validator": "^0.7.3",
		"@hookform/resolvers": "^5.2.2",
		"@radix-ui/react-accordion": "^1.2.12",
		"@tailwindcss/typography": "^0.5.19",
		"@tanstack/react-router": "^1.132.47",
		"ai": "^5.0.65",
		"astro": "^5.14.3",
		"class-variance-authority": "^0.7.1",
		"clsx": "^2.1.1",
		"cmdk": "^1.1.1",
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
		"zustand": "^5.0.8"
```
**File:** src/server/features/chat-messages.ts (L64-67)
```typescript
		let google = createGoogleGenerativeAI({ apiKey: GOOGLE_AI_API_KEY })

		let result = streamText({
			model: google("gemini-2.5-flash"),
```
**File:** src/server/features/chat-messages.ts (L103-165)
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
```
**File:** astro.config.ts (L56-127)
```typescript
	env: {
		schema: {
			GOOGLE_AI_API_KEY: envField.string({
				context: "server",
				access: "secret",
			}),
			CLERK_SECRET_KEY: envField.string({
				context: "server",
				access: "secret",
			}),
			PUBLIC_CLERK_PUBLISHABLE_KEY: envField.string({
				context: "client",
				access: "public",
			}),
			PUBLIC_CLERK_ACCOUNTS_URL: envField.string({
				context: "client",
				access: "public",
			}),
			PUBLIC_JAZZ_SYNC_SERVER: envField.string({
				context: "client",
				access: "public",
			}),
			PUBLIC_VAPID_KEY: envField.string({
				context: "client",
				access: "public",
			}),
			VAPID_PRIVATE_KEY: envField.string({
				context: "server",
				access: "secret",
			}),
			CRON_SECRET: envField.string({
				context: "server",
				access: "secret",
			}),
			PUBLIC_JAZZ_WORKER_ACCOUNT: envField.string({
				context: "client",
				access: "public",
			}),
			JAZZ_WORKER_SECRET: envField.string({
				context: "server",
				access: "secret",
			}),
			PUBLIC_ENABLE_PAYWALL: envField.boolean({
				context: "client",
				access: "public",
			}),
			WEEKLY_BUDGET: envField.number({
				context: "server",
				access: "secret",
			}),
			INPUT_TOKEN_COST_PER_MILLION: envField.number({
				context: "server",
				access: "secret",
			}),
			CACHED_INPUT_TOKEN_COST_PER_MILLION: envField.number({
				context: "server",
				access: "secret",
			}),
			OUTPUT_TOKEN_COST_PER_MILLION: envField.number({
				context: "server",
				access: "secret",
			}),
			MAX_REQUEST_TOKENS: envField.number({
				context: "server",
				access: "secret",
			}),
			PUBLIC_PLAUSIBLE_DOMAIN: envField.string({
				context: "client",
				access: "public",
				optional: true,
			}),
		},
```
**File:** src/shared/tools/tools.ts (L86-87)
```typescript
// eslint-disable-next-line @typescript-eslint/no-empty-object-type
export type TillyUIMessage = UIMessage<MessageMetadata, {}, MyTools>
```
**File:** src/app/lib/store.ts (L6-17)
```typescript
import type { PersistStorage } from "zustand/middleware"
import type { TillyUIMessage } from "#shared/tools/tools"
import { tryCatch } from "#shared/lib/trycatch"

interface AppState {
	peopleSearchQuery: string
	setPeopleSearchQuery: (query: string) => void

	remindersSearchQuery: string
	setRemindersSearchQuery: (query: string) => void

	chat: TillyUIMessage[]
```
**File:** src/app/lib/store.ts (L193-193)
```typescript
			name: "tilly-app-storage",
```
**File:** src/app/sw.ts (L26-28)
```typescript
let sw = self
let USER_CACHE = "tilly-user-v1"
let APP_SHELL_CACHE = "tilly-pages-v1"
```
**File:** src/app/sw.ts (L213-220)
```typescript
function getDefaultNotificationPayload(): NotificationPayload {
	return {
		title: "Tilly",
		body: "You have a new notification",
		icon: "/app/icons/icon-192x192.png",
		badge: "/app/icons/transparent-96x96.png",
		tag: "tilly-notification",
	}
```
**File:** src/app/routes/_app.assistant.tsx (L270-277)
```typescript
						<div className="text-muted-foreground flex items-center justify-center gap-3 py-2 text-sm">
							<Avatar className="size-8 animate-pulse">
								<AvatarImage
									src="/app/icons/icon-192x192.png"
									alt="Tilly logo"
								/>
								<AvatarFallback>T</AvatarFallback>
							</Avatar>
```
**File:** src/app/routes/index.tsx (L68-69)
```typescript
							<motion.div layoutId="title">
								<TypographyH1>Tilly</TypographyH1>
```
**File:** src/server/features/push-shared.ts (L27-30)
```typescript
webpush.setVapidDetails(
	"mailto:support@tilly.app",
	PUBLIC_VAPID_KEY,
	VAPID_PRIVATE_KEY,
```
**File:** public/app/manifest.json (L1-4)
```json
{
	"name": "Tilly Remembers",
	"short_name": "Tilly",
	"description": "Tilly helps you remember what matters and stay in touch with your loved ones. ",
```
**File:** README.md (L1-7)
```markdown
<img src="./public/app/icons/icon-96x96.png" alt="Tilly" width="96" height="96" align="center">

# Tilly

Tilly is an open-source personal relationship manager that helps you stay connected with the people who matter most. Built as a React PWA with client-side sync, offline-first design, and optional agentic AI assistance.

**Try it live:** [tilly.social](https://tilly.social)
```
**File:** src/shared/intl/messages.assistant.ts (L5-38)
```typescript
const baseAssistantMessages = messages({
	// Assistant interface messages
	"assistant.title": "Chat with Tilly",
	"assistant.pageTitle": "Chat - Tilly",
	"assistant.subscribe.title": "Get AI Assistance with Tilly Plus",
	"assistant.subscribe.description":
		"Tilly Agent can turn your thoughts into notes and reminders. It's like talking to your journal!",
	"assistant.subscribe.settings": "Try for Free",
	"assistant.subscribe.currentPlan": "Current plan: {$plan}",
	"assistant.subscribe.currentPlan.unknown": "Unknown",
	"assistant.subscribe.trialStatus": "Free trial active",
	"assistant.subscribe.loading": "Checking your subscription...",
	"assistant.chatUnavailable.title": "Chat unavailable",
	"assistant.chatUnavailable.description":
		"Tilly chat requires an internet connection. All other features work offline.",
	"assistant.emptyState":
		"Share a story, and I'll help you remember what matters",
	"assistant.generating": "Generating response...",
	"assistant.error.title": "Something went wrong",
	"assistant.responseFailure.title": "Response Issue",
	"assistant.responseFailure.emptyDescription":
		"I'm sorry, but I didn't respond properly. This sometimes happens with AI models. Please try your question again.",
	"assistant.usageLimit.title": "Usage limit reached",
	"assistant.usageLimit.description":
		"You've reached your usage limit. Check your settings to see when limits reset.",
	"assistant.usageLimit.viewSettings": "View Settings",
	"assistant.clearChat": "Clear Chat",
	"assistant.clearChatHint.title": "Save usage with fresh conversations",
	"assistant.clearChatHint.description":
		"Starting a new conversation helps save your usage budget. Clear your chat to begin fresh.",
	"assistant.clearChatHint.dismiss": "Got it",
	"assistant.placeholder.disabled": "Chat requires internet connection",
	"assistant.placeholder.initial": "Chat with Tilly",
	"assistant.placeholder.reply": "Reply to Tilly...",
```
**File:** src/shared/intl/messages.assistant.ts (L70-74)
```typescript
	"tool.person.createConfirm.title": "Create new Person {$name}?",
	"tool.person.createConfirm.description":
		"Tilly want's to create a new Person.",
	"tool.person.createCancel.reason": "User chose not to create this person",
	"tool.person.created.message": "Created person: {$name}",
```
**File:** src/shared/intl/messages.assistant.ts (L206-239)
```typescript
const deAssistantMessages = translate(baseAssistantMessages, {
	// Assistant interface messages
	"assistant.title": "Mit Tilly chatten",
	"assistant.pageTitle": "Chat - Tilly",
	"assistant.subscribe.title": "Erhalte KI Unterstützung mit Tilly Plus",
	"assistant.subscribe.description":
		"Der Tilly Agent verwandelt lose Gedanken in Notizen und Erinnerungen. Als würdest du mit deinem Journal Sprechen.",
	"assistant.subscribe.settings": "Kostenlos ausprobieren",
	"assistant.subscribe.currentPlan": "Aktueller Tarif: {$plan}",
	"assistant.subscribe.currentPlan.unknown": "Unbekannt",
	"assistant.subscribe.trialStatus": "Kostenlose Testphase aktiv",
	"assistant.subscribe.loading": "Abonnement wird geprüft...",
	"assistant.chatUnavailable.title": "Chat nicht verfügbar",
	"assistant.chatUnavailable.description":
		"Der Tilly-Chat benötigt eine Internetverbindung. Alle anderen Funktionen funktionieren offline.",
	"assistant.emptyState":
		"Erzähle eine Geschichte – ich helfe dir, das Wichtige zu behalten",
	"assistant.generating": "Antwort wird erstellt...",
	"assistant.error.title": "Etwas ist schief gelaufen",
	"assistant.responseFailure.title": "Antwortproblem",
	"assistant.responseFailure.emptyDescription":
		"Entschuldigung, aber ich habe nicht richtig geantwortet. Das passiert manchmal bei KI-Modellen. Bitte versuche deine Frage nochmal.",
	"assistant.usageLimit.title": "Nutzungsgrenze erreicht",
	"assistant.usageLimit.description":
		"Du hast deine Nutzungsgrenze erreicht. Schaue in den Einstellungen wann die Grenzen zurückgesetzt werden.",
	"assistant.usageLimit.viewSettings": "Einstellungen ansehen",
	"assistant.clearChat": "Chat leeren",
	"assistant.clearChatHint.title": "Nutzung mit neuen Gesprächen sparen",
	"assistant.clearChatHint.description":
		"Ein neues Gespräch zu beginnen hilft, dein Nutzungsbudget zu sparen. Lösche deinen Chat für einen frischen Start.",
	"assistant.clearChatHint.dismiss": "Alles klar",
	"assistant.placeholder.disabled": "Chat erfordert Internetverbindung",
	"assistant.placeholder.initial": "Mit Tilly chatten",
	"assistant.placeholder.reply": "Tilly antworten...",
```
**File:** src/pages/de/index.astro (L16-24)
```text
<MinimalMarketingLayout
	title="Tilly - Dein persönlicher Freundschaftsassistent"
	description="Bleib in Verbindung mit den Menschen, die dir wichtig sind."
>
	<Navbar
		client:load
		logo={{
			text: "Tilly",
			href: "/de/",
```
**File:** src/pages/de/index.astro (L46-52)
```text
			badge={{
				text: "Tilly ist Open Source",
				href: "https://github.com/ccssmnn/tilly",
				description: "Schau dir den Code an",
			}}
			title="Vergiss nie wieder, dich zu melden"
			description="Deine Freunde erzählen dir wichtige Dinge. Tilly hilft dir, dich zu erinnern und nachzufragen, wenn es darauf ankommt."
```
**File:** src/app/features/assistant-message-components.tsx (L4-6)
```typescript
import { UserQuestionConfirmation } from "#shared/tools/user-question-ui"
import type { AddToolResultFunction, TillyUIMessage } from "#shared/tools/tools"

```
**File:** src/app/features/data-download-button.tsx (L106-117)
```typescript
			let exportData: FileData = {
				type: "tilly",
				version: 1,
				people: peopleWithDataURLs,
			}

			let blob = new Blob([JSON.stringify(exportData, null, 2)], {
				type: "application/json",
			})
			let url = URL.createObjectURL(blob)
			let a = document.createElement("a")
			a.href = url
```