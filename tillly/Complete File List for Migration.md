# Complete File List for Migration

## 🔴 Priority 1: Critical Code Files (Breaking Changes)

### Core AI System
```bash
1. src/server/features/chat-messages.ts
   - Lines: 103-165 (System prompt)
   - Change: Replace "Tilly" in AI personality definition

2. src/shared/tools/tools.ts
   - Line: 87
   - Change: Rename `TillyUIMessage` type
```

### State Management & Storage
```bash
3. src/app/lib/store.ts
   - Lines: 6-17 (Type imports and usage)
   - Line: 193 (Storage key)
   - Change: Update type references and storage key name

4. src/app/sw.ts
   - Lines: 26-28 (Cache names)
   - Lines: 213-220 (Notification defaults)
   - Change: Update cache identifiers and notification text
```

### Components
```bash
5. src/app/routes/_app.assistant.tsx
   - Line: 273
   - Change: Update avatar alt text and logo reference

6. src/app/routes/index.tsx
   - Line: 68
   - Change: Update app title in welcome screen

7. src/app/features/assistant-message-components.tsx
   - Lines: 4-6
   - Change: Update `TillyUIMessage` type imports
```

### Server Configuration
```bash
8. src/server/features/push-shared.ts
   - Line: 27
   - Change: Update VAPID email contact
```

---

## 🟡 Priority 2: Configuration Files

```bash
9. package.json
   - Line: 2
   - Change: Update package name

10. public/app/manifest.json
    - Lines: 1-4
    - Change: Update PWA name, short_name, and description
```

---

## 🟢 Priority 3: Internationalization Files (All UI Text)

### Assistant Messages
```bash
11. src/shared/intl/messages.assistant.ts
    - 32 total replacements
    - Lines: 5-38, 70-74, 206-239
```

### Settings Messages
```bash
12. src/shared/intl/messages.settings.ts
    - 71 replacements throughout file
```

### People Messages
```bash
13. src/shared/intl/messages.people.ts
    - 8 replacements
```

### Tour Messages
```bash
14. src/shared/intl/messages.tour.ts
    - 10 replacements
```

### Reminders Messages
```bash
15. src/shared/intl/messages.reminders.ts
    - 4 replacements
```

### UI Messages
```bash
16. src/shared/intl/messages.ui.ts
    - 8 replacements
```

---

## 📄 Priority 4: Documentation & Content

### Main Documentation
```bash
17. README.md
    - 19 replacements throughout file
    - Lines: 1-7 (Header and description)
```

### Marketing Pages (German)
```bash
18. src/pages/de/index.astro
    - 61 replacements
    - Lines: 16-24, 46-52 (Title, navigation, hero section)

19. src/pages/de/blog/index.astro
    - 12 replacements (Blog index)

20. src/pages/de/blog/[...slug].astro
    - 9 replacements (Blog post template)
```

### Marketing Pages (English)
```bash
21. src/pages/en/index.astro
    - 57 replacements

22. src/pages/en/blog/index.astro
    - 12 replacements

23. src/pages/en/blog/[...slug].astro
    - 9 replacements
```

### Blog Posts
```bash
24. src/content/blog/self-hosting/en.md
    - 17 replacements

25. src/content/blog/self-hosting/de.md
    - 17 replacements

26. src/content/blog/pragmatic-relationship-journaling/en.md
    - 2 replacements

27. src/content/blog/pragmatic-relationship-journaling/de.md
    - 2 replacements
```

### Legal Pages
```bash
28. src/pages/de/privacy.md
    - 26 replacements

29. src/pages/en/privacy.md
    - 26 replacements

30. src/pages/de/imprint.md
    - 2 replacements

31. src/pages/en/imprint.md
    - 2 replacements
```

### Legal Layout
```bash
32. src/www/layouts/LegalPageLayout.astro
    - 3 replacements
```

---

## 🎨 Priority 5: Additional Components & Assets

### Page Components
```bash
33. src/pages/app/[...all].astro
    - 3 replacements (App shell titles)

34. src/pages/index.astro
    - 3 replacements (Root redirect)
```

### Tour & Error Components
```bash
35. src/app/routes/tour.tsx
    - 1 replacement

36. src/app/components/main-error-boundary.tsx
    - 1 replacement

37. src/app/components/splash-screen.tsx
    - 1 replacement
```

### Data Management Components
```bash
38. src/app/features/data-file-schema.ts
    - 1 replacement

39. src/app/features/data-upload-button.tsx
    - 1 replacement

40. src/app/features/data-download-button.tsx
    - 2 replacements
    - Lines: 106-117 (Export filename)
```

---

## 📦 Priority 6: Scripts & Test Data

```bash
41. scripts/og.tsx
    - 2 replacements (OG image generator)

42. scripts/README.md
    - 1 replacement

43. scripts/data/mock-data-template.json
    - 1 replacement

44. scripts/data/mock-data-template.de.json
    - 1 replacement
```

---

## 🎭 Priority 7: Assets (Non-Code Files)

```bash
45. tilly-ascii-art.txt
    - Complete replacement or deletion
```

### Icon Files to Replace
```bash
46. public/app/icons/icon-192x192.png
47. public/app/icons/icon-96x96.png
48. public/app/icons/transparent-96x96.png
49. public/app/icons/icon-512x512.png (if exists)
50. public/app/icons/icon-384x384.png (if exists)
51. public/app/icons/icon-256x256.png (if exists)
52. public/app/icons/icon-128x128.png (if exists)
```

---

## 📊 Summary by File Type

| File Type | Count | Action Required |
|-----------|-------|----------------|
| TypeScript/JavaScript | 15 | Code changes |
| Internationalization | 6 | Text replacements |
| Markdown | 11 | Content updates |
| Astro Components | 9 | Template updates |
| JSON | 4 | Configuration updates |
| PNG/Assets | 8+ | File replacements |
| **Total** | **53+** | |

---

## 🔧 Batch Processing Commands

### Find all occurrences
```bash
# Case-insensitive search for all Tilly references
grep -ri "tilly" --include="*.ts" --include="*.tsx" --include="*.astro" --include="*.md" --include="*.json" .
```

### Automated replacement (use with caution)
```bash
# Replace in TypeScript/JavaScript files
find . -type f \( -name "*.ts" -o -name "*.tsx" \) -exec sed -i 's/Tilly/YourAssistantName/g' {} +
find . -type f \( -name "*.ts" -o -name "*.tsx" \) -exec sed -i 's/tilly/yourassistantname/g' {} +

# Replace in Markdown files
find . -type f -name "*.md" -exec sed -i 's/Tilly/YourAssistantName/g' {} +

# Replace in Astro files
find . -type f -name "*.astro" -exec sed -i 's/Tilly/YourAssistantName/g' {} +

# Replace in JSON files
find . -type f -name "*.json" -exec sed -i 's/tilly/yourassistantname/g' {} +
```

### Verify changes
```bash
# Check for any remaining references
grep -r "tilly" . --exclude-dir=node_modules --exclude-dir=.git --exclude="*.log"
```