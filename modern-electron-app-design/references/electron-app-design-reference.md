# Modern Electron App Design — Reference Material

## Electron Official Documentation

### Window Customization
- **Source:** https://www.electronjs.org/docs/latest/tutorial/window-customization
- The `BrowserWindow` module is the foundation of your Electron application, exposing APIs to customize the look and behavior of app windows.
- `BrowserWindow` is a subclass of `BaseWindow`. `BaseWindow` supports composing many web views.
- Key options: `frame`, `transparent`, `roundedCorners`, `hasShadow`, `titleBarStyle`, `titleBarOverlay`

### Custom Title Bar
- **Source:** https://www.electronjs.org/docs/latest/tutorial/custom-title-bar
- Implementing a custom title bar helps applications feel more modern and consistent across platforms.
- Use `titleBarStyle: 'hidden'` with `titleBarOverlay` to customize height, color, and symbol color of window controls.
- Use CSS `-webkit-app-region: drag` to make custom title bars draggable.
- Use `-webkit-app-region: no-drag` for interactive elements (buttons) within the title bar.

### Dark Mode
- **Source:** https://github.com/electron/electron/blob/main/docs/tutorial/dark-mode.md
- Use `nativeTheme.shouldUseDarkColors` to detect current theme.
- Use `nativeTheme.themeSource = 'dark' | 'light' | 'system'` to set theme.
- Listen for `nativeTheme.on('updated')` to react to system theme changes.
- Use `ipcMain.handle()` and `ipcRenderer.invoke()` for main-renderer communication.

### Corner Smoothing CSS
- **Source:** https://github.com/electron/electron/blob/main/docs/api/corner-smoothing-css.md
- `-electron-corner-smoothing: system-ui` matches the OS design language for corner smoothness.
- macOS applies different default percentages than Windows/Linux.
- Values: `0%` (no smoothing), `system-ui` (platform default), `100%` (maximum smoothing).

### Frameless Window
- **Source:** https://github.com/electron/electron/blob/main/docs/fiddles/windows/manage-windows/frameless-window/index.html
- `frame: false` removes all chrome including toolbars, title bars, and borders.
- `transparent: true` enables transparent window backgrounds (performance impact).

### System Preferences
- **Source:** https://github.com/electron/electron/blob/main/docs/api/system-preferences.md
- `systemPreferences.getEffectiveAppearance()` returns the current system appearance ('dark' or 'light').

---

## 2025 Setup Guide: Electron-Vite + Tailwind-Shadcn UI

- **Author:** Mohit Nagaraj
- **Source:** https://blog.mohitnagaraj.in/blog/202505/Electron_Shadcn_Guide
- **Published:** May 20, 2025

### Key Points
- **Electron-Vite v3** has first-class support for React, Vue, and Svelte templates with blazingly fast HMR in both main and renderer processes.
- **Tailwind CSS v4** introduced a new engine that pares runtime to almost nothing, adding native text-shadow utilities and mask support.
- **ShadCN UI** ships accessible, unstyled-by-default components that slot neatly into any Tailwind project.

### Setup Steps
1. Scaffold: `npm create electron-vite@latest my-app`
2. Install Tailwind: `npm install -D tailwindcss @tailwindcss/vite`
3. Add Tailwind plugin to `electron.vite.config.ts` renderer section
4. Add `@import "tailwindcss";` at top of `src/renderer/src/assets/main.css`
5. Create `jsconfig.json` with `@/*` alias pointing to `src/renderer/src/*`
6. Initialize ShadCN: `npx shadcn@latest init`
7. Add components: `npx shadcn@latest add button`

### Common Gotchas
- **No import alias found:** Ensure `@/*` path exists in jsconfig/tsconfig.json
- **Tailwind classes don't show up:** Forgot `@import "tailwindcss";` or plugin not loaded
- **ESLint can't resolve @/:** Add import/resolver settings to .eslintrc
- **Prod build ships 5 MB of icons:** Tree-shake icon imports (use named imports from lucide-react)

---

## How to Add Shadcn/UI to an Electron-Vite App in 5 Easy Steps

- **Author:** Gonzalo F. Buszmicz
- **Source:** https://gbuszmicz.medium.com/how-to-add-shadcn-ui-to-an-electron-vite-app-in-5-easy-steps-cadfdf267823
- **Published:** May 3, 2025

### Key Points
- Electron-Vite combines Vite's speed with Electron's power.
- Shadcn/UI copies components into your project (not a package dependency) — you own the code.
- Shadcn expects a `vite.config.ts` file — copy `electron.vite.config.ts` contents to it.

### Pro Tip: Avoid Duplicating Vite Configs
1. Update `package.json` scripts to add `--config ./vite.config.ts` to all electron-vite commands
2. Update `electron-builder.yml` to exclude `vite.config.*` instead of `electron.vite.config.*`
3. Update `tsconfig.node.json` to include `vite.config.ts`
4. Delete `electron.vite.config.ts`

---

## guasam/electron-react-app — Modern Desktop Application Starter

- **Source:** https://github.com/guasam/electron-react-app
- **Stars:** 774 | **Forks:** 137 | **License:** MIT

### Stack
- Electron, React, TypeScript, Shadcn UI, TailwindCSS, Electron Vite, Electron Builder

### In-Built Features
| Feature | Description |
|---|---|
| Conveyor | Type-safe IPC with Zod validation |
| Custom Titlebar & Menus | Style the window titlebar and menus |
| Clean Project Structure | Separation of main and renderer processes |
| Resources Protocol | Access local file resources via res:// |
| Import Path Aliases | Clean imports with @/ alias |
| Theme Switcher | Built-in dark/light mode |
| Error Boundary | React error boundary with detailed reporting |
| Welcome Kit | Interactive showcase with Framer Motion |
| Hot Reload | Vite's HMR |
| VS Code Debugging | Pre-configured launch configs |

### Conveyor IPC Pattern
- Type-safe IPC with Zod schemas for runtime validation
- Three access methods: React hooks, global conveyor, window object
- 4-step pattern: Define Schema → Add API Method → Implement Handler → Register Handler

---

## BRVWL/electron-react-typescript-template — Production-Ready Template

- **Source:** https://github.com/BRVWL/electron-react-typescript-template
- **Stars:** 3 | **License:** MIT

### Features
- Electron + React + TypeScript + Vite
- shadcn/ui with 15+ components (button, card, badge, progress, switch, tooltip, etc.)
- Dark/light mode with smooth transitions
- System monitoring dashboard example (CPU, memory, network)
- Secure IPC with contextBridge
- Electron Forge for cross-platform builds
- Content Security Policy (CSP) configured

### Template Structure
```
src/
├── index.ts              # Entry point
├── main/                 # Electron main process
│   ├── main.ts
│   ├── preload.ts
│   └── ipc/
│       ├── index.ts
│       └── handlers.ts
├── renderer/             # React UI
│   ├── App.tsx
│   ├── renderer.tsx
│   ├── index.css
│   ├── components/ui/   # shadcn/ui components
│   └── lib/utils.ts
└── shared/               # Shared types
    └── electron.d.ts
```

### 4-Step Feature Addition Pattern
1. Define Data Types in `src/shared/electron.d.ts`
2. Create Backend Handler in `src/main/ipc/handlers.ts`
3. Expose to Frontend in `src/main/preload.ts`
4. Use in React Components

---

## Awesome Electron — Curated Resources

- **Source:** https://github.com/sindresorhus/awesome-electron
- Comprehensive list of Electron resources, tools, apps, and boilerplates.

---

## GlassKit — CSS Glassmorphism Component Library

- **Source:** https://github.com/JUNGHERZ/GlassKit
- Complete CSS component library with glassmorphism aesthetics
- Inspired by iOS 26 Liquid Glass and visionOS
- Features: deeper blur effects, luminous borders, dynamic light reflections
- One CSS file, no build tools, no dependencies

---

## Popular Electron Apps (Design Inspiration)

| App | Design Notes |
|---|---|
| **VS Code** | Custom title bar, sidebar, tabs, activity bar, command palette |
| **Slack** | Custom window chrome, sidebar navigation, threaded conversations |
| **Discord** | Gaming-focused UI, custom title bar, overlay, voice channels |
| **Figma** | Frameless window, canvas-based UI, toolbar, layers panel |
| **Notion** | Clean document editor, minimal chrome, sidebar navigation |
| **Obsidian** | Custom title bar, graph view, sidebar, plugin system |
| **Linear** | Minimalist issue tracker, beautiful typography, keyboard-first |

---

## UI Design Trends (2025-2026)

### Glassmorphism
- Frosted glass effect using `backdrop-filter: blur()`
- Semi-transparent backgrounds with subtle borders
- Creates depth and layering
- Popularized by iOS 26 Liquid Glass and visionOS

### Neumorphism
- Soft, extruded plastic look with inner shadows
- Creates tactile, physical-feeling UI elements
- Best used sparingly for specific components

### Design System Approach
- CSS custom properties for theming
- Consistent color palette with light/dark variants
- Typography scale with modern fonts (Inter, SF Pro, Segoe UI, Roboto)
- Spacing system (4px or 8px grid)
- Component library with consistent API
