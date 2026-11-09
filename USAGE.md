# Using @amilrubasinghe/loyalty-ui in a React / Next.js project

A complete guide with working examples for a loyalty management admin app.

**Packages:**
- `@amilrubasinghe/loyalty-tokens` — design token CSS variables
- `@amilrubasinghe/loyalty-ui` — React components

---

## 1. Installation

### Option A — From npm (recommended)

```bash
npm login   # once per machine — use your npm credentials
pnpm add @amilrubasinghe/loyalty-tokens
pnpm add @amilrubasinghe/loyalty-ui
```

No `.npmrc` or org setup needed. Packages are published under your personal npm scope.

---

### Option B — From GitHub Releases (no npm account needed)

Replace `0.1.0` with the actual release version.

```bash
pnpm add https://github.com/AmilRubasinghe/loyalty_design_system/releases/download/v0.1.0/amilrubasinghe-loyalty-tokens-0.1.0.tgz
pnpm add https://github.com/AmilRubasinghe/loyalty_design_system/releases/download/v0.1.0/amilrubasinghe-loyalty-ui-0.1.0.tgz
```

Or add to `package.json` and run `pnpm install`:

```json
{
  "dependencies": {
    "@amilrubasinghe/loyalty-tokens": "https://github.com/AmilRubasinghe/loyalty_design_system/releases/download/v0.1.0/amilrubasinghe-loyalty-tokens-0.1.0.tgz",
    "@amilrubasinghe/loyalty-ui":     "https://github.com/AmilRubasinghe/loyalty_design_system/releases/download/v0.1.0/amilrubasinghe-loyalty-ui-0.1.0.tgz"
  }
}
```

---

### Option C — Local path (same machine, dev-friendly)

Build the library once, then point to it by path.

```bash
# In the design system repo
pnpm --filter @amilrubasinghe/loyalty-tokens build
pnpm --filter @amilrubasinghe/loyalty-ui build
```

In your consumer project's `package.json`:

```json
{
  "dependencies": {
    "@amilrubasinghe/loyalty-tokens": "file:../loyalty_design_system/packages/tokens",
    "@amilrubasinghe/loyalty-ui":     "file:../loyalty_design_system/packages/ui"
  }
}
```

```bash
pnpm install
```

> Rebuild the library whenever you change components — changes reflect after the next build.

---

## 2. Project setup

### 2.1 Import CSS

Add these four imports **once**, at the very top of your app entry point. Order matters.

**React / Vite — `src/main.tsx`:**
```ts
import '@amilrubasinghe/loyalty-tokens/tokens.css';   // design token CSS variables
import '@amilrubasinghe/loyalty-tokens/styles.css';   // shadows, grid helpers
import '@amilrubasinghe/loyalty-ui/index.css';        // reset + Inter font
import '@amilrubasinghe/loyalty-ui/components.css';   // pre-compiled component styles
```

**Next.js App Router — `app/layout.tsx`:**
```tsx
import '@amilrubasinghe/loyalty-tokens/tokens.css';
import '@amilrubasinghe/loyalty-tokens/styles.css';
import '@amilrubasinghe/loyalty-ui/index.css';
import '@amilrubasinghe/loyalty-ui/components.css';

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" data-theme="light" data-brand="finspace">
      <body>{children}</body>
    </html>
  );
}
```

**Next.js Pages Router — `pages/_app.tsx`:**
```tsx
import '@amilrubasinghe/loyalty-tokens/tokens.css';
import '@amilrubasinghe/loyalty-tokens/styles.css';
import '@amilrubasinghe/loyalty-ui/index.css';
import '@amilrubasinghe/loyalty-ui/components.css';
import type { AppProps } from 'next/app';

export default function App({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />;
}
```

> **No Tailwind setup needed.** Component styles are pre-compiled. Do not install or configure Tailwind CSS in your project.

---

### 2.2 Set theme on `<html>`

All theming is controlled by `data-theme` and `data-brand` on the `<html>` element.

```html
<!-- index.html (Vite) or app/layout.tsx (Next.js) -->
<html data-theme="light" data-brand="finspace" lang="en">
```

Or set from JavaScript:
```ts
document.documentElement.setAttribute('data-theme', 'light'); // 'light' | 'dark'
document.documentElement.setAttribute('data-brand', 'finspace');
```

Available built-in brands (no extra CSS needed):

| `data-brand` | Primary colour | Font |
|---|---|---|
| `finspace` (default) | Indigo `#625bff` | Inter |
| `burgundy` | Burgundy `#660033` | Inter |
| `sampath` | Orange `#f97316` | Hanken Grotesk |

---

### 2.3 Peer dependencies

React 19 is required:

```json
{
  "dependencies": {
    "react":     "^19.0.0",
    "react-dom": "^19.0.0"
  }
}
```

---

## 3. Component examples

All components are imported from `@amilrubasinghe/loyalty-ui`.

```ts
import {
  FinButton,
  FinTextField,
  FinBadge,
  FinAvatar,
  FinStatCard,
  FinToggle,
  FinIcon,
  FinDialog,
  FinTable,
  FinSideSheet,
} from '@amilrubasinghe/loyalty-ui';
```

---

### 3.1 FinButton

```tsx
import { FinButton } from '@amilrubasinghe/loyalty-ui';

<FinButton variant="primary">Add member</FinButton>
<FinButton variant="secondary">Export report</FinButton>
<FinButton variant="tertiary">Cancel</FinButton>

// With leading icon
<FinButton variant="primary" icon="leading" iconName="add">Add programme</FinButton>

// Icon only
<FinButton variant="tertiary" icon="only-icon" iconName="filter_list">Filter</FinButton>

// Destructive
<FinButton tone="destructive" variant="primary">Delete member</FinButton>

// Disabled
<FinButton variant="primary" disabled>Save changes</FinButton>
```

---

### 3.2 FinTextField

```tsx
import { FinTextField } from '@amilrubasinghe/loyalty-ui';

<FinTextField label="Programme name" placeholder="e.g. Platinum Rewards" />

// With helper text
<FinTextField
  label="Points multiplier"
  helperText="Applied to base points earned per transaction"
/>

// Error state
<FinTextField label="Email address" value="not-an-email" error helperText="Enter a valid email address" />

// Disabled
<FinTextField label="Member ID" value="MBR-00142" disabled />

// Mobile number variant
<FinTextField variant="mobile-number" label="Contact number" defaultCountry="LK" />
```

---

### 3.3 FinBadge

```tsx
import { FinBadge } from '@amilrubasinghe/loyalty-ui';

<FinBadge tone="success" label="Active" />
<FinBadge tone="warning" label="Expiring soon" />
<FinBadge tone="error"   label="Suspended" />
<FinBadge tone="info"    label="Pending approval" />
<FinBadge tone="neutral" label="Draft" />
```

---

### 3.4 FinStatCard

```tsx
import { FinStatCard } from '@amilrubasinghe/loyalty-ui';

<FinStatCard label="Total members"     value="24,816" />
<FinStatCard label="Active programmes" value="6" />
<FinStatCard label="Points issued"     value="4,208,500 pts" />
```

---

### 3.5 FinAvatar

```tsx
import { FinAvatar } from '@amilrubasinghe/loyalty-ui';

<FinAvatar type="text"  initials="OJ" label="Olivia James"  size="medium" />
<FinAvatar type="image" src="/photo.jpg" initials="OJ" label="Olivia James" size="medium" />
<FinAvatar type="icon"  label="Unknown member" size="small" />
```

---

### 3.6 FinToggle

```tsx
import { FinToggle } from '@amilrubasinghe/loyalty-ui';

<FinToggle label="Enable notifications" />
<FinToggle label="Allow points transfer" defaultChecked />
<FinToggle label="Archived" disabled />
```

---

### 3.7 FinDialog

```tsx
import { FinButton, FinDialog } from '@amilrubasinghe/loyalty-ui';
import { useState } from 'react';

function DeleteDialog() {
  const [open, setOpen] = useState(false);
  return (
    <>
      <FinButton tone="destructive" variant="secondary" onClick={() => setOpen(true)}>
        Delete member
      </FinButton>
      <FinDialog
        open={open}
        onClose={() => setOpen(false)}
        title="Delete member?"
        description="This will permanently remove Olivia James and all associated points history."
        primaryAction={
          <FinButton tone="destructive" variant="primary" onClick={() => setOpen(false)}>Delete</FinButton>
        }
        secondaryAction={
          <FinButton variant="tertiary" onClick={() => setOpen(false)}>Cancel</FinButton>
        }
      />
    </>
  );
}
```

---

## 4. Semantic CSS variables (for page layout)

Use these in your own CSS instead of hardcoded colors:

```css
var(--fin-bg-primary)          /* main page background */
var(--fin-bg-secondary)        /* raised surfaces, cards */
var(--fin-text-primary)        /* headings, body text */
var(--fin-text-secondary)      /* helper text, captions */
var(--fin-border-default)      /* table borders, dividers */
var(--fin-border-subtle)       /* row separators */

/* Typography shorthands */
font: var(--fin-font-headline-large-h4);
font: var(--fin-font-label-medium);
font: var(--fin-font-body-medium);
font: var(--fin-font-caption-medium);
```

---

## 5. Theme toggle (dark mode)

```tsx
// React / Next.js ('use client' required in Next.js)
'use client';

import { FinButton } from '@amilrubasinghe/loyalty-ui';
import { useEffect, useState } from 'react';

export function ThemeToggle() {
  const [theme, setTheme] = useState<'light' | 'dark'>('light');

  useEffect(() => {
    document.documentElement.setAttribute('data-theme', theme);
  }, [theme]);

  return (
    <FinButton
      variant="tertiary"
      icon="only-icon"
      iconName={theme === 'light' ? 'dark_mode' : 'light_mode'}
      onClick={() => setTheme(t => (t === 'light' ? 'dark' : 'light'))}
    >
      {theme === 'light' ? 'Switch to dark mode' : 'Switch to light mode'}
    </FinButton>
  );
}
```

---

## 6. Troubleshooting

| Problem | Fix |
|---|---|
| `404 Not Found` on npm publish | Personal npm scope needs `npm login` first |
| Components render but have no styles | Add the four CSS imports in the correct order |
| Colors wrong / no theme | Add `data-theme="light"` to `<html>` |
| Icons not showing | Ensure `@amilrubasinghe/loyalty-ui/index.css` is imported |
| TypeScript errors on props | Rebuild the library — `dist/index.d.ts` is generated at build time |
| `dist/` not found after install | Run `pnpm --filter @amilrubasinghe/loyalty-ui build` in the design system repo |
