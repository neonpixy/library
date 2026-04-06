> **[Omnidea](https://github.com/neonpixy/omnidea)** / **[Library](https://github.com/neonpixy/library)** · [README](README.md) · [WIRING.md](https://github.com/neonpixy/omnidea/blob/main/WIRING.md)

# Library

Shared libraries for Omnidea. Five npm packages: SDK, components, glass rendering, editor, and effects.

## Packages

| Package | Path | What It Is |
|---------|------|-----------|
| **@omnidea/net** | `sdk/` | TypeScript SDK. 860 typed operations auto-generated from Omninet's 29 Rust crates. Wraps the pipeline API. |
| **@omnidea/ui** | `ui/` | Solid.js component library. 54 components (34 components + 13 atoms + 4 layout + 2 crystal + 1 theme) with neu and crystal modes. UnoCSS theme. |
| **@omnidea/crystal** | `crystal/` | WebGPU glass effects (TypeScript). Kawase blur, SDF shapes (analytic + JFA), dome lighting, cursor tracking, glass-on-glass compositing. Native Rust/wgpu version at `crystal/native/`. |
| **@omnidea/editor** | `editor/` | CRDT-backed editor. Daemon owns SequenceRga, TypeScript owns the view. Solid.js component. |
| **@omnidea/fx** | `fx/` | Visual effects primitives. Animated color, bevel, glow effects as Solid.js components. |

## Build

```bash
npm install          # install all workspace deps
npm run build        # build sdk, crystal, ui in dependency order
npm run generate     # regenerate SDK from Omninet Rust sources
npm run test         # run vitest
npm run clean        # remove dist/ and node_modules
```

Individual builds: `npm run build:sdk`, `npm run build:crystal`, `npm run build:ui`.

## Per-Package Docs

- `crystal/CLAUDE.md` — architecture, shaders, gotchas
- `crystal/README.md` — features, API quick start
- `crystal/SDK.md` — full API reference
- `sdk/README.md` — bridge architecture, namespaces, code generation

## Workspace Structure

```
Library/
  crystal/        @omnidea/crystal  — WebGPU glass rendering (TS + native Rust)
  sdk/            @omnidea/net      — protocol SDK (auto-generated)
  ui/             @omnidea/ui       — Solid.js components
  editor/         @omnidea/editor   — CRDT editor
  fx/             @omnidea/fx       — visual effects
```

## Key Patterns

- **Source-distributed packages**: ui, editor, and fx export raw TypeScript (`"main": "./src/lib/index.ts"`). No build step for consumers.
- **Built packages**: sdk and crystal compile to `dist/`. Must be built before importing.
- **Code generation**: SDK types and ops are generated from Omninet Rust source by `scripts/generate.py`. Run `npm run generate` after Omninet changes.
- **Icons**: Remix Icon (`ri-` prefix). No paid icon dependencies.
- **CSS framework**: UnoCSS (not Tailwind). Config at `uno.config.ts` in Throne.

## How Library Connects to Other Repos

```
Omninet (protocol)    Library (shared libs)     Omny (browser)
  29 Rust crates   ->  @omnidea/net SDK   ->  Apps (programs)
  Regalia tokens   ->  @omnidea/ui theme  ->  visual design
                       @omnidea/crystal   ->  glass rendering
                       @omnidea/editor    ->  CRDT editing
```

All four repos are siblings inside `Omnidea/`: `Covenant/`, `Omninet/`, `Omny/`, `Library/`.
