# Prompt 1: Design System Setup (Vue/Nuxt Edition)

Analyze this design screenshot, extract design tokens, and set up a complete design system with shadcn-vue.

## Input

`[SCREENSHOT FROM DRIBBBLE, BEHANCE, OR ANY DESIGN INSPIRATION]`

## Workflow

### 1. Analyze the Design

Look at the image and identify/infer:

**Colors**

- Primary/brand color → generate full scale (50-900)
- Neutral/grey colors → generate full scale (50-900)
- Semantic colors (success, error, warning, info)
- Background and surface colors
- Border colors

**Typography**

- Font family (sans-serif, serif, monospace)
- Heading sizes and weights
- Body text sizes

**Spacing & Radius**

- Spacing rhythm (tight, normal, relaxed)
- Border radius style (sharp, rounded, pill)

**Shadows**

- Shadow style (none, subtle, prominent)

### 2. Initialize shadcn-vue

```bash
npx shadcn-vue@latest init
```

When prompted:

- Framework: Nuxt
- Style: Default
- Base color: Neutral (we'll override with our tokens)
- CSS variables: Yes

### 3. Generate and Apply tailwind.css

Replace `assets/css/tailwind.css` with extracted design tokens:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    /* === BASE === */
    --background: [extracted page background];
    --foreground: [extracted text color];

    /* === CARD === */
    --card: [extracted card background];
    --card-foreground: [extracted card text];

    /* === POPOVER / DROPDOWN / TOOLTIP === */
    --popover: [same as card or white];
    --popover-foreground: [same as card-foreground];

    /* === PRIMARY (main brand color) === */
    --primary: [extracted primary color];
    --primary-foreground: [white or dark based on contrast];

    /* === SECONDARY === */
    --secondary: [light grey or muted version];
    --secondary-foreground: [dark text];

    /* === MUTED === */
    --muted: [light grey background];
    --muted-foreground: [medium grey text];

    /* === ACCENT === */
    --accent: [same as secondary or slight tint];
    --accent-foreground: [dark text];

    /* === DESTRUCTIVE === */
    --destructive: [red/error color];
    --destructive-foreground: [white];

    /* === BORDERS & INPUTS === */
    --border: [extracted border color];
    --input: [slightly darker border for inputs];
    --ring: [primary color for focus rings];

    /* === BORDER RADIUS === */
    --radius: [extracted radius, e.g., 0.5rem];

    /* === CHART COLORS === */
    --chart-1: [primary];
    --chart-2: [complementary color];
    --chart-3: [variation];
    --chart-4: [variation];
    --chart-5: [variation];

    /* === SIDEBAR === */
    --sidebar-background: [sidebar background];
    --sidebar-foreground: [sidebar text];
    --sidebar-primary: [primary];
    --sidebar-primary-foreground: [white];
    --sidebar-accent: [accent];
    --sidebar-accent-foreground: [dark text];
    --sidebar-border: [border color];
    --sidebar-ring: [primary];

    /* === CUSTOM SEMANTIC COLORS === */
    --success: [green];
    --success-foreground: [white];
    --warning: [yellow/orange];
    --warning-foreground: [dark for contrast];
    --info: [blue];
    --info-foreground: [white];
  }

  .dark {
    /* Inverted values for dark mode */
    --background: [dark background];
    --foreground: [light text];
    /* ... all other variables for dark mode */
  }
}

@layer base {
  * {
    @apply border-border;
  }
  body {
    @apply bg-background text-foreground;
    font-family: '[extracted font]', sans-serif;
  }
}
```

### 4. Install Recommended Font

If a Google Font matches the design, add it to your `nuxt.config.ts` using `@nuxtjs/google-fonts`:

```bash
npm install -D @nuxtjs/google-fonts
```

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  modules: [
    '@nuxtjs/tailwindcss',
    '@nuxtjs/google-fonts'
  ],
  googleFonts: {
    families: {
      Inter: [400, 500, 600, 700], // Replace with extracted font
    }
  }
})
```

### 5. Install Demo Components

Use shadcn-vue to install components for the styleguide:

```bash
npx shadcn-vue@latest add button card badge alert radio-group
```

### 6. Create Styleguide Navigation Config

Create `utils/navigation.ts` to manage styleguide navigation:

```ts
export interface NavItem {
  name: string
  href: string
}

export interface NavSection {
  title: string
  items: NavItem[]
}

export const navigation: NavSection[] = [
  {
    title: "Foundation",
    items: [
      { name: "Design Tokens", href: "/styleguide" },
    ]
  },
  {
    title: "Components",
    items: [
      // Components will be added here by Prompt 2
    ]
  }
]
```

### 7. Create Styleguide Layout with Sidebar

Create `layouts/styleguide.vue` with a sidebar that reads from the navigation config:

```vue
<script setup lang="ts">
import { useRoute } from 'vue-router'
import { navigation } from '~/utils/navigation'
import { cn } from '@/lib/utils'

const route = useRoute()
</script>

<template>
  <div class="flex min-h-screen">
    <aside class="w-64 border-r bg-card p-6 flex flex-col gap-6 fixed top-0 left-0 h-screen overflow-y-auto">
      <div>
        <NuxtLink to="/styleguide" class="text-xl font-bold">
          Design System
        </NuxtLink>
      </div>

      <nav class="flex flex-col gap-6">
        <div v-for="section in navigation" :key="section.title">
          <h3 class="text-sm font-semibold text-muted-foreground mb-2">
            {{ section.title }}
          </h3>
          <ul class="flex flex-col gap-1">
            <li v-for="item in section.items" :key="item.href">
              <NuxtLink
                :to="item.href"
                :class="cn(
                  'block px-3 py-2 rounded-md text-sm transition-colors',
                  route.path === item.href
                    ? 'bg-primary text-primary-foreground'
                    : 'hover:bg-muted'
                )"
              >
                {{ item.name }}
              </NuxtLink>
            </li>
          </ul>
        </div>
      </nav>
    </aside>

    <main class="flex-1 ml-64 overflow-auto">
      <slot />
    </main>
  </div>
</template>
```

### 8. Create Styleguide Page

Create `pages/styleguide/index.vue` displaying ALL design tokens in one page. Make sure to wrap it in the new layout:

```vue
<script setup lang="ts">
definePageMeta({
  layout: 'styleguide'
})
// Import demo components here
</script>

<template>
  <div class="p-8">
  </div>
</template>
```

Ensure it includes:

- Color palette — all colors as swatches with CSS variable names
- Primary scale — 50 through 900
- Grey scale — 50 through 900
- Semantic colors — success, warning, error, info
- Typography — heading and body text samples
- Border radius — examples of each size
- Shadows — shadow examples
- Components — button, card, badge, alert, radio group using the tokens
- Dark mode toggle — preview both themes

## Directory Structure

```text
├── assets/
│   └── css/
│       └── tailwind.css     # Complete design tokens
├── layouts/
│   └── styleguide.vue       # Shared layout with sidebar navigation
├── pages/
│   └── styleguide/
│       ├── index.vue        # All design tokens in one page
│       └── components/
│           └── [name].vue   # Individual components (added by Prompt 2)
└── utils/
    └── navigation.ts        # Navigation config (editable by Prompt 2)
```

## Output

- shadcn-vue initialized
- `assets/css/tailwind.css` with complete design tokens
- Font installed via Nuxt config
- Demo components installed (button, card, badge, alert, radio-group)
- Styleguide with navigable sidebar:
  - `layouts/styleguide.vue` — layout with sidebar
  - `utils/navigation.ts` — navigation config
  - `pages/styleguide/index.vue` — all design tokens
- Design system ready for Prompt 2 (components) and Prompt 3 (pages)

## Design Summary (also provide)

After setup, summarize:

- Primary color: `[hex and color name]`
- Font: `[font name]`
- Style: `[e.g., "Modern minimal", "Bold colorful", "Soft friendly"]`
- Border radius: `[e.g., "Rounded 8px", "Sharp", "Pill"]`
- Overall feel: `[brief description]`

## Notes

- If colors aren't clearly visible, make reasonable inferences
- Generate harmonious color scales using color theory
- Ensure sufficient contrast for accessibility (4.5:1 for text)
- Chart colors should be visually distinct
- When in doubt, use shadcn-vue defaults as fallback
