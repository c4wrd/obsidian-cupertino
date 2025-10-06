# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Cupertino is an Obsidian theme that provides a native, minimal design optimized for desktop and mobile devices. It won Best Theme in Obsidian's 2024 Gems of the Year awards.

The theme focuses on:
- **Native & minimal** appearance across macOS, Windows, and Android
- **Mobile optimization** with redesigned modals, menus, and comfortable spacing
- **Liquid Glass** visual effects (requires Style Settings plugin)
- **Windows Mode** with Fluent Design UI styling
- Platform-specific optimizations that are enabled by default

## Architecture

### Build System

The theme is built using **SCSS** and compiled to `theme.css`. There is no package.json or build script in the repository.

**To compile SCSS to CSS:**
```bash
npx sass src/theme.scss theme.css
```

The main entry point is `src/theme.scss`, which imports all other modules using `@use` directives.

### Directory Structure

```
src/
├── app/              # Core application UI (editor, tabs, modals, sidedock)
├── components/       # Reusable UI components (buttons, checkboxes, menus)
├── features/         # Optional features and helper classes
├── plugins/          # Styling for specific Obsidian plugins
└── plugins-core/     # Styling for Obsidian's built-in plugins
```

### Module Organization

The `src/theme.scss` file imports modules in this order:
1. **App modules** - Core UI elements (root, animation, tab, editor, modal, etc.)
2. **Components** - Reusable UI widgets (button, checkbox, menu, slider, etc.)
3. **Core plugins** - Built-in Obsidian features (backlinks, canvas, graph, outline)
4. **Community plugins** - Third-party plugin styling (style-settings, notebook-navigator, etc.)
5. **Features** - Optional enhancements (windows-mode, cards, image-filters, etc.)

### Style Settings Integration

The theme uses the **Style Settings** plugin for configuration. Settings are defined in `src/app/root.scss` using the `@settings` comment block syntax.

Settings are primarily for **disabling features** rather than adding customization. Platform-specific settings are automatically hidden/shown based on the current platform:
- macOS settings hidden on Windows/mobile
- Windows settings hidden on non-Windows platforms
- Android settings hidden on non-Android devices

This logic is implemented in `src/plugins/style-settings.scss`.

### Platform-Specific Styling

The theme adapts to different platforms using CSS body classes:
- `body.mod-windows` - Windows-specific styling
- `body.is-mobile` - Mobile device styling
- `body.is-android` - Android-specific styling
- `body.is-phone` - Phone-specific styling (vs tablet)

**Windows Mode** (`src/features/windows-mode.scss`) is a complete styling system built on top of Cupertino components, featuring Segoe UI fonts and Fluent Design patterns.

### Helper Classes

Cupertino supports helper classes from the Minimal theme:
- **Cards** - Grid layouts for Dataview tables and lists (`cards`, `list-cards`, `cards-cols-N`)
- **Block width** - Width controls (`wide`, `max`, `table-100`, `img-max`, etc.)
- **Image filters** - Visual effects applied via markdown (`#invert`, `#circle`, `#blend`, etc.)
- **Tables** - Sizing and striping (`table-small`, `row-alt`, `col-alt`)
- **Embeds** - Styling controls (`embed-strict`, `embed-hide-title`)

### Custom Checkboxes

The theme implements [Alternative Checkboxes](https://github.com/damiankorcz/Alternative-Checkboxes-Reference-Set) with 25+ checkbox variants using syntax like `- [/]`, `- [?]`, `- [!]`, etc. These are styled in `src/components/checkbox.scss`.

## Development Workflow

### Making Changes

1. Edit SCSS files in the `src/` directory
2. Compile using `npx sass src/theme.scss theme.css`
3. Test in Obsidian by reloading the theme

### Adding New Features

- **New component** → Add to `src/components/` and import in `src/theme.scss`
- **New feature** → Add to `src/features/` and import in `src/theme.scss`
- **Plugin support** → Add to `src/plugins/` or `src/plugins-core/` and import in `src/theme.scss`
- **New setting** → Add to the `@settings` block in `src/app/root.scss`

### Release Process

Releases are automated via GitHub Actions (`.github/release.yml`):
1. Update version in `manifest.json`
2. Push a git tag (e.g., `git tag 2.0.6 && git push origin 2.0.6`)
3. GitHub Action creates a draft release with `manifest.json` and `theme.css`

The compiled `theme.css` must be committed to the repository for releases.

## Philosophy

From the README:
1. **Less plugins** - Style Settings only for disabling features, not adding them
2. **Less customizations** - No extensive customization options to avoid distraction
3. **Less visual noise** - Low priority UI elements auto-hide to maintain focus on content

This means: avoid adding new customization options unless absolutely necessary. Features should work well out-of-the-box.
