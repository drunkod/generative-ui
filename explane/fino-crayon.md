# Fino-Crayon Project - Complete Explanation

## 🎯 What is Fino-Crayon?

A **financial AI assistant** that:
- Analyzes your transactions
- Shows interactive charts and visualizations
- Asks for consent before actions
- Persists conversations in a database
- Uses AI tools to query data

---

## 🏗️ Architecture Overview

```
User Interface (React)
    ↓
Thread Management (Conversations)
    ↓
API Routes (/api/ask, /api/threads, /api/messages)
    ↓
OpenAI + Tools (SQL queries, budget setting)
    ↓
Database (Prisma + SQLite)
    ↓
Response Templates (Charts, Cards, Consent Forms)
```

---

## 📊 Complete Flow Diagram

```
User: "Show my food expenses"
    ↓
1. Frontend (page.tsx)
    → Sends to /api/ask with threadId + message
    ↓
2. API Route (api/ask/route.ts)
    → Loads conversation history from DB
    → Builds OpenAI request with:
        • System prompt (instructions)
        • Tools (execute_sql, set_budget)
        • Response format (structured templates)
    ↓
3. OpenAI Processing
    → Decides: "I need to query the database"
    → Calls tool: execute_sql("SELECT * FROM Transaction WHERE category='Food'")
    → Gets results
    → Formats response with breakdown_expenses template
    ↓
4. Streaming Response
    → Stream: "Here's your food spending:" (text)
    → Stream: { breakdown_expenses template with data } (UI component)
    → Stream: "Consider reducing dining out" (text)
    ↓
5. Frontend Rendering
    → Renders text
    → Renders interactive pie chart
    → Saves to database
    ↓
User sees: Text + Interactive Chart
```

---

## 🔑 Key Components Breakdown

### **1. Database Schema (Prisma)**

```prisma
Transaction {
  id, date, amount, category, transaction_type
}

Thread {
  id, title, messages[], status
}

Messages {
  id, threadId, message (serialized), timestamps
}
```

**Purpose:** Store financial transactions and conversation history

---

### **2. Response Templates** (Custom UI Components)

Located in: `src/app/responseTemplates/`

| Template | What It Does | When Used |
|----------|-------------|-----------|
| **breakdown_expenses** | Pie chart of spending by category | "Show my expenses" |
| **trends** | Line chart over time | "Show spending trend" |
| **breakdown_2d** | Multi-dimensional bar chart | "Expenses by month and category" |
| **user_consent** | Accept/Decline buttons | "Set a budget" |

**Example Template Definition:**

```typescript
// templates.ts
{
  name: "breakdown_expenses",
  description: "Renders a summary of the user's financial situation.",
  parameters: BreakdownExpensesSummarySchema, // Zod schema
}
```

**The AI learns:** "When user asks about expenses, use this template with this data shape"

---

### **3. Tools (AI Functions)**

Located in: `src/app/api/ask/executors.ts`

#### **Tool 1: execute_sql**
```typescript
execute_sql({
  query: "SELECT category, SUM(amount) FROM Transaction WHERE transaction_type='debit' GROUP BY category",
  description: "Analyzing your spending by category"
})
```

**What happens:**
1. AI generates SQL query
2. Function executes on database
3. Returns results to AI
4. AI formats into template

#### **Tool 2: set_budget**
```typescript
set_budget({
  category: "Food",
  budget: 500,
  consent: true  // From user_consent template
})
```

**What happens:**
1. AI asks for consent using `user_consent` template
2. User clicks Accept/Decline
3. Tool executes with consent status

---

## 🔄 Step-by-Step Example

### **User:** "What are my biggest expenses?"

---

#### **Step 1: Frontend Sends Request**

```typescript
// page.tsx
threadManager.onProcessMessage({
  message: "What are my biggest expenses?",
  threadManager,
  abortController
})
```

Sends to `/api/ask`:
```json
{
  "threadId": 1,
  "role": "user",
  "message": "What are my biggest expenses?"
}
```

---

#### **Step 2: API Loads Context**

```typescript
// route.ts
const previousMessages = await prisma.messages.findMany({
  where: { threadId: 1 },
  orderBy: { created_at: "asc" }
});
```

Gets conversation history to maintain context.

---

#### **Step 3: Build OpenAI Request**

```typescript
openai.beta.chat.completions.runTools({
  model: "gpt-4o",
  messages: [
    SYSTEM_MESSAGE,  // Instructions
    ...historicalMessages,  // Previous conversation
    userMessage  // Current question
  ],
  tools: [
    execute_sql,  // Can query database
    set_budget    // Can set budgets
  ],
  response_format: {
    // Must return structured JSON with templates
    type: "json_schema",
    json_schema: TemplatesJsonSchema
  }
})
```

**System Message Tells AI:**
```
"You are Fino, a financial advisor.
You can use tools to query transactions.
ALWAYS respond with this structure:
{
  "response": [
    "text explanation",
    { template with data },
    "additional insights"
  ]
}
```

---

#### **Step 4: AI Processes (Behind the Scenes)**

**AI Thinking:**
1. "User wants biggest expenses"
2. "I need transaction data"
3. **Calls Tool:** `execute_sql`

```javascript
execute_sql({
  query: "SELECT category, SUM(ABS(amount)) as total FROM Transaction WHERE transaction_type='debit' GROUP BY category ORDER BY total DESC LIMIT 5",
  description: "Analyzing your top spending categories"
})
```

**Tool Returns:**
```json
[
  { "category": "Food", "total": 1250 },
  { "category": "Transportation", "total": 800 },
  { "category": "Entertainment", "total": 600 }
]
```

4. **AI Formats Response:**

```json
{
  "response": [
    "Here are your biggest expense categories:",
    {
      "type": "template",
      "name": "breakdown_expenses",
      "templateProps": {
        "expenses": [
          { "category": "Food", "amount": 1250 },
          { "category": "Transportation", "amount": 800 }
        ],
        "total_spent": 2650
      }
    },
    "Food is your largest expense. Consider cooking at home more often to save money."
  ]
}
```

---

#### **Step 5: Stream Response**

```typescript
// fromOpenAICompletion converts to Server-Sent Events
return new NextResponse(responseStream, {
  headers: {
    "Content-Type": "text/event-stream",
  },
});
```

**Streams in real-time:**
```
Event 1: "Here are"
Event 2: "Here are your biggest"
Event 3: "Here are your biggest expense categories:"
Event 4: { breakdown_expenses template }
Event 5: "Food is your largest..."
```

---

#### **Step 6: Frontend Renders**

```typescript
// processStreamedMessage handles the stream
await processStreamedMessage({
  response,
  createMessage: threadManager.appendMessages,
  updateMessage: threadManager.updateMessage,
});
```

**Renders:**
1. Text: "Here are your biggest expense categories:"
2. **Component:** Renders `<BreakdownExpenses />` with props
3. Text: "Food is your largest expense..."

---

#### **Step 7: Save to Database**

```typescript
// onFinish callback
await prisma.messages.createMany({
  data: [
    { threadId: 1, message: JSON.stringify(userMessage) },
    { threadId: 1, message: JSON.stringify(aiResponse) }
  ]
});
```

Conversation persisted for next time!

---

## 🎨 Visual Example: User Consent Flow

### **User:** "Set a food budget of $400"

**AI Response Stream:**
```json
{
  "response": [
    "I can help you set a budget for the Food category.",
    {
      "type": "template",
      "name": "user_consent",
      "templateProps": {
        "explanation": "This will set your monthly Food budget to $400. You'll get alerts when you approach this limit."
      }
    }
  ]
}
```

**Frontend Renders:**
```
┌────────────────────────────────────┐
│ User Consent                       │
│ This will set your monthly Food    │
│ budget to $400. You'll get alerts  │
│ when you approach this limit.      │
│                                    │
│  [Accept]  [Decline]               │
└────────────────────────────────────┘
```

**User Clicks "Accept":**

```typescript
// user-consent.tsx
<Button onClick={() =>
  threadActions.processMessage({
    role: "user",
    message: "I accept"
  })
}>Accept</Button>
```

**Sends:** "I accept" → AI receives → Calls `set_budget` tool with `consent: true`

---

## 🗂️ File Responsibilities

| File | Purpose |
|------|---------|
| **page.tsx** | Main UI, thread management, message handling |
| **api/ask/route.ts** | Process messages, call OpenAI, handle tools |
| **api/ask/executors.ts** | Tool implementations (SQL, budget) |
| **api/threads/route.ts** | CRUD operations for conversations |
| **api/messages/route.ts** | Load/update message history |
| **responseTemplates/*.tsx** | UI components AI can render |
| **types/responseTemplates/*.tsx** | Zod schemas for type safety |
| **lib/db/init.ts** | Seed database with mock transactions |

---

## 🧩 Key Concepts

### **1. Structured Outputs**

AI doesn't return free-form text. It returns:
```json
{
  "response": [
    "text" | { template object }
  ]
}
```

Enforced by `response_format: json_schema`

---

### **2. Tool Calling**

AI can execute functions during generation:
```
User: "Show expenses"
  → AI: "I need data"
  → AI calls: execute_sql(...)
  → Tool returns data
  → AI formats with template
```

---

### **3. Thread Persistence**

Every conversation saved:
```typescript
Thread { id: 1, title: "Expense Analysis" }
  └─ Messages [
       { role: "user", message: "Show expenses" },
       { role: "assistant", message: {...template} }
     ]
```

---

### **4. Type Safety**

```typescript
// Zod schema defines shape
const BreakdownExpensesSummarySchema = z.object({
  expenses: z.array(z.object({
    category: z.string(),
    amount: z.number()
  })),
  total_spent: z.number()
});

// React component gets typed props
export const BreakdownExpenses: React.FC<BreakdownExpensesSummaryProps> = ({
  expenses,
  total_spent
}) => { ... }
```

AI must return data matching schema → Component receives correct props

---

## 🚀 How to Test

1. **Run the app:**
   ```bash
   pnpm install
   pnpm dev
   ```

2. **Try these prompts:**
   - "What are my expenses?" → breakdown_expenses template
   - "Show spending over time" → trends template
   - "Expenses by month and category" → breakdown_2d template
   - "Set a food budget of $500" → user_consent template

3. **Watch the flow:**
   - Network tab: See streaming events
   - Database: Check `prisma/dev.db` for saved messages
   - Console: Tool calls logged

---

## 💡 Compare to Simple Template

| Feature | Template-JS | Fino-Crayon |
|---------|-------------|-------------|
| Database | ❌ | ✅ Prisma + SQLite |
| Persistence | ❌ | ✅ Threads + Messages |
| Custom Templates | ❌ | ✅ 4 financial templates |
| Tools | ❌ | ✅ SQL + Budget setting |
| Structured Output | ❌ | ✅ JSON schema enforced |
| Complexity | Beginner | Advanced |

---

## 🎓 Key Learnings

1. **Templates = UI Components the AI can use**
2. **Tools = Functions the AI can call**
3. **Structured Output = Force AI to return specific JSON shape**
4. **Streaming = Real-time rendering as AI generates**
5. **Persistence = Save conversations for context**

---

## 🤔 Questions to Explore

1. **How to add a new template?**
   - Create component in `responseTemplates/`
   - Define Zod schema in `types/responseTemplates/`
   - Register in `templates.ts`

2. **How to add a new tool?**
   - Create function in `executors.ts`
   - Define Zod schema for parameters
   - Register in `runTools` call

3. **How does streaming work internally?**
   - `fromOpenAICompletion` converts OpenAI stream → SSE format
   - `processStreamedMessage` parses SSE → renders templates

Want me to dive deeper into any specific part?