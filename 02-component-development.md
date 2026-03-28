Prompt 2: Component Development (Vue/Nuxt Edition)
Add a [COMPONENT NAME] to the project using shadcn-vue.

Workflow
1. Check if Component Exists in shadcn-vue
Use shadcn MCP to search the registry:

Search for the component: search_items_in_registries with query "[component name]"

If found → view details: view_items_in_registries to see structure and dependencies

Get usage examples: get_item_examples_from_registries with query "[component]-demo"

Decision:

Component exists → go to Step 2 (Install)

Component doesn't exist → go to Step 4 (Build Custom)

Common shadcn-vue components:

Layout: Card, Separator, Tabs, Accordion, Collapsible

Forms: Button, Input, Select, Checkbox, Radio Group, Switch, Textarea, Label, Form

Feedback: Alert, Toast, Progress, Skeleton, Badge

Overlay: Dialog, Drawer, Popover, Tooltip, Dropdown Menu, Context Menu, Alert Dialog

Navigation: Navigation Menu, Breadcrumb, Pagination, Command

Data: Table, Data Table, Calendar, Chart

2. Install shadcn-vue Component
Get the install command using shadcn MCP:

get_add_command_for_items for the component

Run the command:

Bash
npx shadcn-vue@latest add [component-name]
This adds the component to components/ui/.
It automatically uses CSS variables from assets/css/tailwind.css.

Review the installed component to understand:

Available variants (size, style, etc.)

Props interface

How it uses CSS variables

Emitted events (if applicable)

3. Customize Component (if needed)
If the base component needs additional variants or behavior, create a wrapped version in components/Custom[ComponentName].vue:

Snippet de código
<script setup lang="ts">
import { Button } from '@/components/ui/button'
import { cn } from '@/lib/utils'

interface Props {
  intent?: 'default' | 'success' | 'warning' | 'info'
  class?: string
}

const props = withDefaults(defineProps<Props>(), {
  intent: 'default',
})
</script>

<template>
  <Button
    :class="cn(
      // Use CSS variables via Tailwind classes
      props.intent === 'success' && 'bg-success text-success-foreground hover:bg-success/90',
      props.intent === 'warning' && 'bg-warning text-warning-foreground hover:bg-warning/90',
      props.intent === 'info' && 'bg-info text-info-foreground hover:bg-info/90',
      props.class
    )"
    v-bind="$attrs"
  >
    <slot />
  </Button>
</template>
Customization patterns:

Add new color variants using your CSS variables (bg-success, text-warning, etc.)

Add new size variants

Compose multiple shadcn-vue components together

Add loading states, icons, or other features via slots and props

4. Build Custom Component (if not in shadcn-vue)
If shadcn-vue doesn't have this component, build it using:

Vue/Radix primitives as building blocks

CSS variables via Tailwind classes

shadcn-vue's patterns for consistency

Create components/CustomWidget.vue:

Snippet de código
<script setup lang="ts">
import { cn } from '@/lib/utils'

interface Props {
  variant?: 'default' | 'primary' | 'muted'
  class?: string
}

const props = withDefaults(defineProps<Props>(), {
  variant: 'default',
})
</script>

<template>
  <div :class="cn(
    'rounded-lg border p-4',
    props.variant === 'default' && 'bg-card text-card-foreground border-border',
    props.variant === 'primary' && 'bg-primary text-primary-foreground border-primary',
    props.variant === 'muted' && 'bg-muted text-muted-foreground border-border',
    props.class
  )">
    <slot />
  </div>
</template>
5. Create Component Showcase
Add to pages/styleguide/components/[component-name].vue:

Define the layout metadata (definePageMeta({ layout: 'styleguide' }))

All variants side by side (sizes, colors, styles)

All states (default, hover, focus, disabled, loading)

Dark mode preview (toggle between light/dark)

Interactive demo with prop controls (using Vue ref or reactive)

Code examples for common use cases

Use examples from shadcn MCP (get_item_examples_from_registries) as reference.

6. Document Usage
Include in the showcase page:

Import statement

Basic usage example with <template> block

All available props with types and defaults

Variant examples with code

Accessibility notes (keyboard navigation, ARIA attributes)

7. Update Styleguide Navigation
Add the new component to utils/navigation.ts:

In the "Components" section, add a new entry:

TypeScript
{
  title: "Components",
  items: [
    // ... existing components
    { name: "[Component Name]", href: "/styleguide/components/[component-name]" },
  ]
}
This makes the component appear in the styleguide sidebar navigation.

Directory Structure
components/
├── ui/                    # Base shadcn-vue components (auto-generated)
│   ├── button/
│   │   └── index.ts
│   ├── card/
│   └── ...
└── CustomComponent.vue    # Your customized/new components

pages/
└── styleguide/
    └── components/
        └── [component-name].vue   # Component showcase

utils/
└── navigation.ts          # Navigation config updated here
Output
Component installed/created in components/

Showcase page in pages/styleguide/components/[name].vue

Navigation updated in utils/navigation.ts

Component visible in styleguide sidebar

Usage documented with code examples

Notes
Use shadcn MCP to search, view, and get examples before building

CSS variables are the source of truth (defined in assets/css/tailwind.css)

Tailwind classes reference CSS variables (bg-primary, text-muted-foreground)

No Figma needed for component development - shadcn defines the design

Extend, don't rebuild - customize shadcn-vue components rather than building from scratch