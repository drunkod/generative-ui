# Tilly → New Assistant Migration Plan

## 📋 Preliminary Preparation

### 1. Define Replacement Parameters
```yaml
Current values:
  name: Tilly
  name_lower: tilly
  package: @ccssmnn/tilly
  domain: tilly.social / tilly.app
  email: support@tilly.app

New values:
  name: [YourAssistantName]         # e.g.: "Alice"
  name_lower: [yourassistantname]   # e.g.: "alice"
  package: @[yourorg]/[name]        # e.g.: "@mycompany/alice"
  domain: [yourdomain]               # e.g.: "alice.ai"
  email: [youremail]                 # e.g.: "support@alice.ai"
```

### 2. Create Backups
- Full repository backup
- Database export (if production exists)
- Save current environment settings

---

## 🚨 Phase 1: Critical Changes (Breaking Changes)

### 1.1 Update AI Personality
**File:** `src/server/features/chat-messages.ts` (lines 103-165)
```typescript
// Replace "You are Tilly" with "You are [YourAssistantName]"
// Update all Tilly mentions in the system prompt
```

### 1.2 Rename Data Types
**Global search and replace:**
```bash
# Rename TillyUIMessage type
TillyUIMessage → [YourAssistantName]UIMessage

# Affected files:
- src/shared/tools/tools.ts (L87)
- src/app/lib/store.ts (L7, L17)
- src/app/features/assistant-message-components.tsx (L4-6)
```

### 1.3 Update Storage Keys
**⚠️ IMPORTANT: Requires data migration**

**File:** `src/app/lib/store.ts` (L193)
```typescript
// Old key
name: "tilly-app-storage"
// New key
name: "[yourassistantname]-app-storage"
```

**Migration script to preserve user history:**
```javascript
// Add to application initialization code
const migrateStorage = async () => {
  const oldData = await get('tilly-app-storage');
  if (oldData && !await get('[yourassistantname]-app-storage')) {
    await set('[yourassistantname]-app-storage', oldData);
    console.log('Storage migrated successfully');
  }
};
```

### 1.4 Service Worker and Caching
**File:** `src/app/sw.ts` (L26-28, L213-220)
```typescript
// Update cache names
let USER_CACHE = "[yourassistantname]-user-v1"
let APP_SHELL_CACHE = "[yourassistantname]-pages-v1"

// Update notifications
title: "[YourAssistantName]",
tag: "[yourassistantname]-notification",
```

---

## 📦 Phase 2: Configuration and Metadata

### 2.1 NPM Package
**File:** `package.json` (L2)
```json
"name": "@[yourorg]/[yourassistantname]",
```

### 2.2 PWA Manifest
**File:** `public/app/manifest.json` (L1-4)
```json
{
  "name": "[YourAssistantName] Remembers",
  "short_name": "[YourAssistantName]",
  "description": "[YourAssistantName] helps you remember what matters..."
}
```

### 2.3 Push Notifications
**File:** `src/server/features/push-shared.ts` (L27-30)
```typescript
webpush.setVapidDetails(
  "mailto:[youremail]",
  PUBLIC_VAPID_KEY,
  VAPID_PRIVATE_KEY,
);
```

---

## 📝 Phase 3: Content and Localization

### 3.1 Internationalization (i18n)
**Bulk update in folder:** `src/shared/intl/`

| File | Number of replacements | Priority |
|------|------------------------|----------|
| messages.assistant.ts | 32 | High |
| messages.settings.ts | 71 | Medium |
| messages.people.ts | 8 | Medium |
| messages.tour.ts | 10 | Medium |
| messages.reminders.ts | 4 | Low |
| messages.ui.ts | 8 | Low |

**Automation for replacement:**
```bash
# Use sed or similar utility
find src/shared/intl -name "*.ts" -exec sed -i 's/Tilly/[YourAssistantName]/g' {} +
```

### 3.2 Marketing Pages
**Update landing pages:**
- `src/pages/de/index.astro` (61 replacements)
- `src/pages/en/index.astro` (57 replacements)

### 3.3 Documentation
**Update:**
- `README.md` - project description
- All blog posts in `src/content/blog/`
- Legal pages (`privacy.md`, `imprint.md`)

---

## 🎨 Phase 4: Assets and UI Components

### 4.1 Logos and Icons
**Replace files:**
```
public/app/icons/
├── icon-192x192.png
├── icon-96x96.png
├── transparent-96x96.png
└── ... other sizes
```

### 4.2 UI Components
**File:** `src/app/routes/_app.assistant.tsx` (L273)
```tsx
// Update alt text
alt="[YourAssistantName] logo"
```

**File:** `src/app/routes/index.tsx` (L68)
```tsx
<TypographyH1>[YourAssistantName]</TypographyH1>
```

### 4.3 ASCII Art
**File:** `tilly-ascii-art.txt`
- Create new ASCII art or delete file

---

## 🧪 Phase 5: Testing

### 5.1 Update Test Data
```
scripts/data/mock-data-template.json
scripts/data/mock-data-template.de.json
```

### 5.2 Update OG Images
**File:** `scripts/og.tsx`
- Regenerate social media previews

---

## ⚡ Migration Execution Order

### Day 1: Preparation
1. ✅ Create new branch `migration/new-assistant`
2. ✅ Prepare all new assets (logos, icons)
3. ✅ Write data migration script

### Day 2: Critical Changes
1. ⚠️ Update AI system prompt
2. ⚠️ Rename data types
3. ⚠️ Update storage keys with migration
4. ⚠️ Update Service Worker

### Day 3: Configuration
1. 📦 Update package.json
2. 📦 Update PWA manifest
3. 📦 Update push notification configuration

### Day 4: Content
1. 📝 Update all i18n files
2. 📝 Update marketing pages
3. 📝 Update documentation

### Day 5: Finalization
1. 🎨 Replace all assets
2. 🧪 Update test data
3. 🧪 Run full testing
4. 📋 Code review and merge

---

## 🎯 Validation Checklist

### Functional Tests
- [ ] AI assistant responds with new name
- [ ] Chat history persists after migration
- [ ] Push notifications work
- [ ] PWA installs with new name
- [ ] Offline mode works correctly

### Visual Checks
- [ ] New logos display correctly
- [ ] Name updated in all UI elements
- [ ] Meta tags updated for SEO
- [ ] OG images updated

### Technical Checks
- [ ] No "Tilly" mentions in code (except migration)
- [ ] TypeScript compiles without errors
- [ ] Tests pass successfully
- [ ] Build process completes successfully

---

## 🚀 Deployment

### Deployment Strategy
1. **Staging environment** - full testing
2. **Canary deployment** - 10% of users
3. **Full rollout** - after 24-48 hours of monitoring

### Rollback (if necessary)
```bash
# Quick rollback to previous version
git revert --no-commit HEAD~5..HEAD
git commit -m "Revert migration"
```

---

## 📊 Post-Migration Monitoring

### Metrics to Track
- Number of errors in logs
- Percentage of successful AI interactions
- Number of active users
- User feedback

### Hotfixes
Prepare hotfix scripts for:
- User data recovery
- Cache clearing
- Force PWA update