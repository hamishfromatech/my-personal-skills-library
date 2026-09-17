---
name: modern-electron-app-design
description: Build stunning, professional-grade Electron desktop apps with modern UI frameworks (React/Vue + Tailwind CSS v4 + ShadCN UI), frameless windows with custom title bars, dark/light mode, type-safe IPC, and native-feel design patterns. Use when designing cross-platform desktop applications, building Electron app UIs, or setting up modern Electron development toolchains.
---

# Modern Professional Beautiful Looking Electron Desktop Apps

## Overview

Building stunning, professional-grade desktop applications with Electron requires a thoughtful blend of modern web technologies, native integration patterns, and polished UI/UX design. The 2025-2026 landscape favors React or Vue paired with Tailwind CSS v4 for styling, ShadCN UI for accessible component libraries, Vite for fast builds, and TypeScript for type safety. Key design principles include frameless windows with rounded corners, native-looking custom title bars, dark/light mode support, smooth animations, and responsive layouts that feel at home on macOS, Windows, and Linux.

**Date:** 2026-06-29 (Australia/Brisbane timezone)

## When to Use

- Designing and building cross-platform desktop applications with Electron
- Setting up a modern Electron development toolchain (Vite + Tailwind + ShadCN UI)
- Implementing custom window chrome (frameless windows, custom title bars, window controls)
- Adding dark/light mode with native OS theme integration
- Building type-safe IPC communication between main and renderer processes
- Creating polished, professional UIs that feel native on macOS, Windows, and Linux

### NOT for
- Simple single-page web apps that don't need desktop integration
- Projects where bundle size is the primary constraint (consider Tauri instead)
- Teams without frontend web development experience (HTML/CSS/JS required)

## Tech Stack Recommendations (2025-2026)

### Core Framework
- **Electron** — Cross-platform desktop app framework built on Chromium and Node.js
- **React + TypeScript** or **Vue 3 + TypeScript** — Component-based UI with type safety
- **Vite** or **Electron-Vite** — Fast build tooling with hot module replacement (HMR)

### Styling & Components
- **Tailwind CSS v4** — Utility-first CSS framework (new Oxide engine, CSS-priority config, near-zero runtime)
- **ShadCN UI** — Beautiful, accessible component library built on Radix UI and Tailwind. You own the code (copied into your project, not a package dependency)
- **MUI / Ant Design / PrimeVue** — Alternative design systems for React or Vue

### Build & Distribution
- **Electron Forge** — Unified build tooling from project creation to packaging (batteries-included)
- **Electron Builder** — Packaging with auto-update support for macOS, Windows, Linux

## Quick Start: Electron + Vite + Tailwind + ShadCN UI Setup

### Step 1: Scaffold an Electron-Vite project
```bash
npm create electron-vite@latest my-app
cd my-app
npm install
npm run dev
```

### Step 2: Install Tailwind CSS v4 for the renderer
```bash
npm install -D tailwindcss @tailwindcss/vite
```

Add the Tailwind plugin to `electron.vite.config.ts`:
```typescript
import { defineConfig, externalizeDepsPlugin } from 'electron-vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'
import { resolve } from 'path'

export default defineConfig({
  main: { plugins: [externalizeDepsPlugin()] },
  preload: { plugins: [externalizeDepsPlugin()] },
  renderer: {
    resolve: {
      alias: {
        '@renderer': resolve('src/renderer/src'),
        '@': resolve('src/renderer/src'),
      },
    },
    plugins: [react(), tailwindcss()],
  },
})
```

Add `@import "tailwindcss";` at the top of `src/renderer/src/assets/main.css`.

### Step 3: Add a JavaScript/TypeScript alias
Create `jsconfig.json` (or update `tsconfig.json`):
```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/renderer/src/*"],
      "@renderer/*": ["src/renderer/src/*"]
    }
  }
}
```

### Step 4: Initialize ShadCN UI
```bash
npx shadcn@latest init
```
If it complains about no supported framework, copy `electron.vite.config.ts` to `vite.config.ts` and rerun.

### Step 5: Add and use a ShadCN component
```bash
npx shadcn@latest add button
```

```tsx
import { Button } from '@/components/ui/button'

function App() {
  return (
    <div className="flex h-screen items-center justify-center bg-slate-950">
      <Button variant="default">Hello from ShadCN inside Electron!</Button>
    </div>
  )
}
```

### Pro Tip: Avoid Duplicating Vite Configs
Instead of having both `electron.vite.config.ts` and `vite.config.ts`, use only `vite.config.ts`:
1. Update `package.json` scripts to add `--config ./vite.config.ts` to all electron-vite commands
2. Update `electron-builder.yml` to exclude `vite.config.*` instead of `electron.vite.config.*`
3. Update `tsconfig.node.json` to include `vite.config.ts`
4. Delete `electron.vite.config.ts`

## Window Configuration Best Practices

### Frameless Windows with Rounded Corners
```javascript
const { app, BrowserWindow } = require('electron')

const createWindow = () => {
  const win = new BrowserWindow({
    frame: false,           // Removes native chrome (title bar, borders)
    transparent: false,     // Keep opaque for performance
    roundedCorners: true,   // Modern rounded corners (default in Electron v30+)
    hasShadow: true,        // Subtle drop shadow for depth
    width: 1200,
    height: 800,
    minWidth: 400,
    minHeight: 300,
    title: 'My App',
    webPreferences: {
      preload: path.join(__dirname, 'preload.js'),
      contextIsolation: true,
      nodeIntegration: false
    }
  })

  win.loadFile('index.html')
}
```

### Custom Title Bar with Window Controls Overlay
```javascript
const { BrowserWindow } = require('electron')

const win = new BrowserWindow({
  titleBarStyle: 'hidden',
  titleBarOverlay: {
    color: '#2f3241',
    symbolColor: '#74b1be',
    height: 60
  }
})
```

### Custom Title Bar CSS
```css
:root {
  --titlebar-height: 42px;
  --bg-primary: #ffffff;
  --bg-secondary: #f8fafc;
  --text-primary: #0f172a;
  --accent-color: #3b82f6;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg-primary: #1e293b;
    --bg-secondary: #0f172a;
    --text-primary: #f1f5f9;
  }
}

.titlebar {
  -webkit-app-region: drag; /* Makes title bar draggable */
  height: var(--titlebar-height);
  background: var(--bg-secondary);
  display: flex;
  align-items: center;
  padding: 0 16px;
  gap: 8px;
}

.window-controls {
  -webkit-app-region: no-drag; /* Buttons remain clickable */
  display: flex;
  gap: 8px;
  margin-left: auto;
}

.close-btn:hover { background: #ef4444; }
.minimize-btn:hover { background: #f59e0b; }
.maximize-btn:hover { background: #22c55e; }
```

### Corner Smoothing (Native Feel)
```css
.rounded-element {
  border-radius: 24px;
  -electron-corner-smoothing: system-ui; /* Matches OS design language */
}
```

## Dark Mode Implementation

### Native Theme Management (Main Process)
```javascript
const { app, BrowserWindow, nativeTheme, ipcMain } = require('electron')

ipcMain.handle('dark-mode:toggle', () => {
  if (nativeTheme.shouldUseDarkColors) {
    nativeTheme.themeSource = 'light'
  } else {
    nativeTheme.themeSource = 'dark'
  }
  return nativeTheme.shouldUseDarkColors
})

ipcMain.handle('dark-mode:system', () => {
  nativeTheme.themeSource = 'system'
})

// Listen for system theme changes
nativeTheme.on('updated', () => {
  if (nativeTheme.themeSource === 'system') {
    BrowserWindow.getAllWindows().forEach(win => {
      win.webContents.send('dark-mode:system-applied')
    })
  }
})
```

### CSS Dark Mode
```css
@media (prefers-color-scheme: dark) {
  body {
    background-color: #0f172a;
    color: #e2e8f0;
  }
  .card {
    background-color: #1e293b;
    border-color: #334155;
  }
}

body.dark-mode {
  --bg-primary: #0f172a;
  --text-primary: #f8fafc;
}
```

## Security Best Practices

### Preload Script Pattern (Required)
```javascript
// preload.js
const { contextBridge, ipcRenderer } = require('electron')

contextBridge.exposeInMainWorld('api', {
  darkMode: {
    toggle: () => ipcRenderer.invoke('dark-mode:toggle'),
    system: () => ipcRenderer.invoke('dark-mode:system')
  }
})
```

### Renderer Usage
```javascript
async function toggleDarkMode() {
  const isDark = await window.api.darkMode.toggle()
  console.log('Current theme:', isDark ? 'dark' : 'light')
}
```

## UI Design Principles for Professional Apps

### 1. Consistent Spacing & Typography
- Use a design token system (spacing scale, font sizes)
- Maintain consistent padding/margins (8px grid system)
- Choose modern fonts: Inter, SF Pro, Segoe UI, or Roboto

### 2. Visual Hierarchy
- Primary actions use accent colors
- Secondary actions use muted tones
- Clear distinction between interactive and static elements

### 3. Subtle Animations
```css
.card {
  transition: transform 0.15s ease, box-shadow 0.2s ease;
}

.card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
}

.page-enter {
  opacity: 0;
  transform: translateX(20px);
}

.page-enter-active {
  opacity: 1;
  transform: translateX(0);
  transition: all 0.3s ease;
}
```

### 4. Responsive Layout Patterns
- Flexbox or Grid for adaptive layouts
- Handle window resizing gracefully
- Support different screen sizes and DPI scaling

## Recommended Project Structure
```
my-electron-app/
├── src/
│   ├── main/
│   │   ├── index.js          # Main process entry point
│   │   ├── preload.js        # Security bridge script
│   │   └── ipc/              # IPC handlers
│   │       ├── index.ts
│   │       └── handlers.ts
│   └── renderer/
│       ├── index.html        # App shell
│       ├── assets/
│       │   └── main.css      # Global styles with Tailwind import
│       ├── components/
│       │   └── ui/           # ShadCN UI components
│       ├── lib/
│       │   └── utils.ts      # Utility functions (cn, etc.)
│       └── App.tsx           # Main React component
├── assets/                   # Icons, images, etc.
├── package.json
└── electron.vite.config.ts   # Vite config for Electron
```

## Starter Templates & Resources

### Quick Start Templates
- **guasam/electron-react-app** — Modern starter with Electron, React, TypeScript, TailwindCSS, ShadCN UI, custom titlebar, type-safe IPC (Conveyor), theme switcher, Framer Motion animations
- **BRVWL/electron-react-typescript-template** — Production-ready with Electron Forge, shadcn/ui (15+ components), dark/light mode, system monitoring example, secure IPC
- **LuanRoger/electron-shadcn** — Electron Forge with shadcn/ui, Vite + TypeScript
- **rohitsoni007/electron-shadcn** — Modern Electron app template with React, TypeScript, Vite, and shadcn/ui

### Design Inspiration Sources
- **ShadCN UI** (ui.shadcn.com) — Accessible, customizable components
- **Tailwind CSS** (tailwindcss.com) — Utility-first styling
- **Electron Fiddles** — Official examples for windows, menus, dialogs
- **macOS / Windows 11 design guidelines** — Native feel references
- **GlassKit** — CSS component library with glassmorphism aesthetics (inspired by iOS 26 Liquid Glass and visionOS)
- **Awesome Electron** (github.com/sindresorhus/awesome-electron) — Curated list of Electron resources

## Key Design Patterns for Professional Apps

### Type-Safe IPC (Conveyor Pattern)
The Conveyor pattern from guasam/electron-react-app provides type-safe IPC:
```typescript
// Define schema with Zod
import { z } from 'zod'

export const appIpcSchema = {
  'get-app-info': {
    args: z.tuple([]),
    return: z.object({
      name: z.string(),
      version: z.string(),
      platform: z.string(),
    }),
  },
} as const

// Use in React
import { useConveyor } from '@/app/hooks/use-conveyor'

function MyComponent() {
  const { version } = useConveyor('app')
  
  const handleGetVersion = async () => {
    console.log('App version:', await version())
  }
}
```

### 4-Step Feature Addition Pattern
1. **Define Data Types** — Add interfaces in shared types
2. **Create Backend Handler** — Add IPC handler in main process
3. **Expose to Frontend** — Add to preload script via contextBridge
4. **Use in React Components** — Build UI with error handling

### Custom Dialogs & Modals
```javascript
const { dialog } = require('electron')

app.whenReady().then(() => {
  const win = new BrowserWindow({
    width: 800, height: 600,
    frame: false, roundedCorners: true, hasShadow: true,
    webPreferences: { preload: path.join(__dirname, 'preload.js') }
  })
})
```

### Context Menus & Native Integrations
```javascript
const { Menu, MenuItem } = require('electron')

win.on('ready-to-show', () => {
  const contextMenu = new Menu([
    { label: 'Copy', role: 'copy' },
    { type: 'separator' },
    { label: 'Settings', click: () => win.loadFile('settings.html') }
  ])

  win.webContents.on('context-menu', (e, params) => {
    if (params.editable) contextMenu.popup({ window: win })
  })
})
```

## Modern UI Design Trends (2025-2026)

### Glassmorphism
- Frosted glass effect using `backdrop-filter: blur()`
- Semi-transparent backgrounds with subtle borders
- Creates depth and layering in the UI
- Popularized by iOS 26 Liquid Glass and visionOS

### Neumorphism
- Soft, extruded plastic look with inner shadows
- Creates tactile, physical-feeling UI elements
- Best used sparingly for specific components (buttons, cards, toggles)

### Design System Approach
- CSS custom properties for theming
- Consistent color palette with light/dark variants
- Typography scale with modern fonts
- Spacing system (4px or 8px grid)
- Component library with consistent API

## Common Pitfalls to Avoid

1. **Don't disable sandboxing** — Keep `contextIsolation: true` and avoid `nodeIntegration: true` in renderer
2. **Avoid transparent backgrounds unless needed** — They impact performance significantly
3. **Test on all platforms** — macOS, Windows, and Linux have different rendering quirks
4. **Use CSS variables for theming** — Makes dark/light mode switching seamless
5. **Don't over-engineer the title bar** — Keep it minimal; focus 80% of effort on content area
6. **Tree-shake icon imports** — Use `import { ArrowRight } from 'lucide-react'` instead of `import * as Icons`
7. **Keep aliases in sync** — Ensure Vite, jsconfig/tsconfig, and ESLint all have matching path aliases

## Build & Distribution Checklist
- [ ] Custom app icons (macOS .appiconset, Windows .ico, Linux SVGs)
- [ ] Auto-update configured (Squirrel.Windows, Electron Updater, Sparkle for macOS)
- [ ] Code signing for macOS (notarization) and Windows (signtool)
- [ ] Installer creation (InnoSetup for Windows, DMG for macOS, AppImage/Flatpak for Linux)
- [ ] Content Security Policy (CSP) configured in index.html
- [ ] Production build tested on all target platforms

## Popular Apps Built with Electron (Design Inspiration)
- **VS Code** — Professional editor with custom title bar, sidebar, tabs
- **Slack** — Clean messaging UI with custom window chrome
- **Discord** — Gaming-focused UI with custom title bar and overlay
- **Figma** — Design tool with frameless window and canvas-based UI
- **Notion** — Clean document editor with minimal chrome
- **Obsidian** — Knowledge base with custom title bar and sidebar
- **Linear** — Minimalist issue tracker with beautiful typography

## Related Skills
- See `developer-experience-and-flow/onboarding-acceleration-protocol` for AI-assisted developer onboarding patterns
- See `cognitive-science-and-ux/cognitive-surrender-defense` for preventing over-dependence on AI during development
- See `privacy-and-trust/privacy-first-trust-architecture` for privacy-first design patterns applicable to Electron apps

## Sources
- Electron Official Documentation — Window Customization, Custom Title Bar, Dark Mode, Corner Smoothing CSS (electronjs.org/docs)
- Mohit Nagaraj — "2025 Setup Guide: Electron-Vite + Tailwind-Shadcn UI" (blog.mohitnagaraj.in, May 2025)
- Gonzalo F. Buszmicz — "How to Add Shadcn/UI to an Electron-Vite App in 5 Easy Steps" (Medium, May 2025)
- guasam/electron-react-app — Modern Electron starter with Conveyor IPC, custom titlebar, theme switcher (github.com/guasam/electron-react-app)
- BRVWL/electron-react-typescript-template — Production-ready Electron template with shadcn/ui (github.com/BRVWL/electron-react-typescript-template)
- sindresorhus/awesome-electron — Curated list of Electron resources (github.com/sindresorhus/awesome-electron)
- GlassKit — CSS glassmorphism component library (github.com/JUNGHERZ/GlassKit)
