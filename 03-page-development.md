# Prompt 3: Page Development (Vue/Nuxt Edition)

Build a **[PAGE NAME]** page based on the provided design/screenshot.

**Input:** [SCREENSHOT OR FIGMA URL]

## Workflow

### 1. Analyze the Design Visually

Look at the image and identify:

**Layout Structure:**

- How many main sections/columns?
- Is there a sidebar? Header? Footer?
- What's the grid structure? (1-column, 2-column, 3-column)
- Container widths and spacing patterns

**UI Sections:**

- Break down the page into logical sections (top to bottom, left to right)
- Name each section by its purpose (e.g., "Sidebar Navigation", "Task List", "Chat Panel")

**Content Hierarchy:**

- What are the primary headings?
- What's the main content vs. supporting content?
- What are the call-to-action elements?

### 2. Map Visual Elements to shadcn-vue Components

For each UI element identified, map it to a shadcn-vue component:

| Visual Element | shadcn-vue Component | Notes |
| --- | --- | --- |
| Navigation sidebar | Sidebar | Use sidebar components |
| Tabs/segmented control | Tabs | For section switching |
| Cards with content | Card | CardHeader, CardContent, CardFooter |
| List of items | Card or Table | Depending on complexity |
| Buttons | Button | Variant: default, outline, ghost |
| Form inputs | Input, Textarea | With Label |
| Dropdowns | Select or DropdownMenu | |
| Badges/tags | Badge | For status indicators |
| Icons | lucide-vue-next icons | |
| Modal/dialog | Dialog | For overlays |
| Toast/notification | Toast | For feedback |
| Avatar/profile image | Avatar | |
| Progress indicator | Progress | |
| Checkbox/toggle | Checkbox or Switch | |

Use shadcn MCP to verify components exist:

- `search_items_in_registries` for each component type
- `get_add_command_for_items` to get install commands

**IMPORTANT:** Prioritize importing existing components from the design system over creating new ones.

### 3. Define Page Sections

Create a section breakdown:

```text
Page: [PAGE NAME]
├── Header
│   ├── Logo/Brand
│   ├── Navigation tabs
│   └── User actions
├── Sidebar (if present)
│   ├── Navigation items
│   └── ...
├── Main Content
│   ├── Section 1: [name]
│   ├── Section 2: [name]
│   └── ...
└── Footer (if present)
```

### 4. Install Required Components

Based on the mapping, install all needed shadcn-vue components:

```bash
npx shadcn-vue@latest add [component1] [component2] [component3] ...
```

### 5. Build Page Structure

Create `pages/[page-name].vue` (or `pages/[page-name]/index.vue`):

```vue
<script setup lang="ts">
import { Card, CardHeader, CardContent } from '@/components/ui/card'
import { Button } from '@/components/ui/button'
import { Tabs, TabsList, TabsTrigger, TabsContent } from '@/components/ui/tabs'
// ... other imports
</script>

<template>
  <div class="flex min-h-screen">
    <aside class="w-64 border-r bg-sidebar">
      </aside>

    <main class="flex-1">
      <header class="border-b p-4">
        </header>

      <div class="p-6">
        </div>
    </main>
  </div>
</template>
```

### 6. Apply Styling

Use Tailwind classes that reference your CSS variables:

- **Backgrounds:** `bg-background`, `bg-card`, `bg-muted`, `bg-sidebar`
- **Text:** `text-foreground`, `text-muted-foreground`
- **Borders:** `border-border`
- **Spacing:** Use Tailwind's spacing scale (`p-4`, `gap-6`, `space-y-4`)

### 7. Responsive Behavior

Define how the layout adapts:

- **Mobile (< 768px):** Sidebar collapses, single column
- **Tablet (768px–1024px):** Sidebar as overlay or mini
- **Desktop (> 1024px):** Full layout as designed

```vue
<template>
  <div class="flex flex-col md:flex-row">
    <aside class="hidden md:block md:w-64">
      </aside>
    <main class="flex-1">
      </main>
  </div>
</template>
```

### 8. Add Interactivity

Implement:

- Navigation/routing between pages using `<NuxtLink>` or `useRouter()`
- State for tabs, toggles, and selections using Vue's `ref` and `reactive`
- Form handling via `v-model`
- Loading and error states using Vue conditionals (`v-if`, `v-else`)

### 9. Add Page Metadata

Use Nuxt's `useHead` composable inside your `<script setup>` for SEO and page titles:

```vue
<script setup lang="ts">
useHead({
  title: 'Page Title',
  meta: [
    { name: 'description', content: 'Page description for SEO' }
  ]
})
</script>
```

## Output

- List of identified sections and components
- All required shadcn-vue components installed
- Page created at `pages/[page-name].vue`
- Responsive layout matching the design
- Interactive elements working with Vue state

## Example Analysis

For a screenshot showing a project management interface:

**Identified Sections:**

- **Left Sidebar** — Navigation and project info
- **Center Panel** — Chat/conversation view with task cards
- **Right Panel** — Task list with actions

**Component Mapping:**

| Element | Component |
| --- | --- |
| Sidebar | Sidebar |
| Project dropdown | Select |
| Chat messages | Card |
| Task cards | Card with CardHeader, CardContent, CardFooter |
| "Approve Plan" button | Button (variant: default) |
| "Edit Plan" button | Button (variant: outline) |
| Task list items | Card or custom list item |
| "View Plan" links | Button (variant: ghost) |
| Status badges | Badge |
| Tabs (Preview/Plan/Code) | Tabs |
| Avatar | Avatar |
| Input field | Input or Textarea |

**Install command:**

```bash
npx shadcn-vue@latest add sidebar card button badge tabs avatar input select
```

## Notes

- Works with any image — rough Figma designs, screenshots, or mockups
- Visual analysis first — identify patterns before mapping to components
- Use shadcn MCP to verify components exist and get install commands
- CSS variables for all colors (defined in `assets/css/tailwind.css`)
- Mobile-first — consider responsive behavior from the start
- Import from design system — prioritize importing components from the design system over creating new ones
