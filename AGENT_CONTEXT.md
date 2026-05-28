# AGENT_CONTEXT.md — hexcms-studio

**Last updated:** 2026-05-23

## 1. TL;DR + Identity

HexCMS Studio is a local-only visual markdown editor and admin UI companion to HexCMS. It provides WYSIWYG editing (TipTap), raw code editing (CodeMirror 6), frontmatter management, Git integration, and multi-repository support -- all running as a Next.js server on the user's machine with direct filesystem access.

| Field | Value |
|-------|-------|
| slug | hexcms-studio |
| division | hexaxia-labs |
| type | oss-library |
| status | active |
| version | 0.2.0 |
| owner | alamb-hex |
| repo | https://github.com/alamb-hex/hexcms-studio |
| license | MIT |

## 2. Most Important Facts

- **Local-only by design.** Requires direct filesystem access for file reads/writes and Git commands. Cannot be deployed to Vercel or any serverless platform.
- **Companion to HexCMS.** Studio is the editor UI layer; HexCMS is the content API/backend. Studio connects to local Git repos that HexCMS reads from. They are sibling projects, not nested.
- **Moved to Hexaxia-Labs GitHub org.** The canonical public repo is `github.com/Hexaxia-Labs/hexcms-studio`. The `alamb-hex/hexcms-studio` remote is the development origin.
- **Default port is 3000.** Configurable via `PORT` env var. Run with `pnpm dev` (development) or `pnpm build && pnpm start` (production).
- **Config stored in `~/.hexcms-studio/config.json`.** Repository registrations persist here across restarts. Uses `os.homedir()` -- not the `HOME` env var.
- **Node.js 18+ required.** TypeScript 6, Next.js 16 (App Router), Turbopack enabled in dev.
- **Two editor modes:** Visual (TipTap WYSIWYG) and Code (CodeMirror 6 raw markdown). Both load via dynamic imports (`ssr: false`) to avoid SSR crashes.
- **Two app modes:** Blog mode (repo-scoped, frontmatter-aware) and File mode (raw filesystem browser, no frontmatter parsing).

## 3. Repo Map

```
hexcms-studio/
├── src/
│   ├── app/
│   │   ├── page.tsx              # Single-page app root, all UI state lives here
│   │   ├── layout.tsx            # Root layout, theme context provider
│   │   ├── globals.css           # Tailwind v4 + CSS variable theme definitions
│   │   └── api/
│   │       ├── files/route.ts    # GET/PUT/POST/DELETE markdown files in active repo
│   │       ├── filesystem/route.ts # General filesystem browse + raw file save
│   │       ├── git/route.ts      # Git status, stage, commit, push, pull
│   │       ├── images/[...path]/route.ts # Serve images from content dirs
│   │       ├── preview/route.ts  # Render markdown to HTML (server-side)
│   │       ├── repos/route.ts    # CRUD for registered repositories
│   │       └── upload/route.ts   # Image upload (10MB limit, MIME validation)
│   ├── components/
│   │   ├── Editor/               # CodeMirror 6 raw markdown editor
│   │   ├── VisualEditor/         # TipTap WYSIWYG editor
│   │   ├── FileBrowser/          # File mode filesystem navigator
│   │   ├── FileTree/             # Blog mode repo file tree
│   │   ├── FrontmatterForm/      # YAML frontmatter editing form
│   │   ├── GitPanel/             # Git operations sidebar
│   │   ├── Preview/              # Live markdown preview (DOMPurify sanitized)
│   │   └── ThemeSwitcher/        # Light/Dark/Midnight/Sepia/System selector
│   ├── contexts/
│   │   └── ThemeContext.tsx      # Theme state and CSS variable application
│   └── lib/
│       ├── config.ts             # RepoConfig + AppConfig types, ~/.hexcms-studio CRUD
│       ├── files.ts              # Filesystem read/write/tree helpers
│       ├── git.ts                # simple-git wrappers (status, diff, commit, push, pull)
│       └── markdown.ts           # unified/remark/rehype pipeline, gray-matter parsing
├── docs/
│   ├── COMPONENTS.md             # Component props, data flow, styling patterns
│   ├── INSTALLATION.md           # Setup guide for end users
│   ├── THEMING.md                # Theme customization
│   └── USER_GUIDE.md             # End-user usage documentation
├── public/                       # Static assets including Hexaxia logo
├── biome.json                    # Linter + formatter config (tabs, double quotes)
├── next.config.ts                # Minimal Next.js config
├── package.json                  # Dependencies, scripts, postcss override
└── CHANGELOG.md                  # Version history
```

**Where to look:**

| Task | File(s) |
|------|---------|
| Add/change UI layout | `src/app/page.tsx` |
| Add an API endpoint | `src/app/api/<name>/route.ts` |
| Modify editor behavior | `src/components/Editor/` or `src/components/VisualEditor/` |
| Change Git operations | `src/lib/git.ts` + `src/app/api/git/route.ts` |
| Change config schema | `src/lib/config.ts` |
| Change markdown rendering | `src/lib/markdown.ts` + `src/app/api/preview/route.ts` |
| Modify themes | `src/app/globals.css` + `src/contexts/ThemeContext.tsx` |

## 4. Stack

| Layer | Technology |
|-------|-----------|
| Framework | Next.js 16 (App Router, Turbopack) |
| Language | TypeScript 6 |
| Styling | Tailwind CSS v4 + `@tailwindcss/typography` |
| WYSIWYG editor | TipTap 3 (`@tiptap/react`, `@tiptap/starter-kit`, extensions) |
| Code editor | CodeMirror 6 (`@codemirror/view`, `@codemirror/lang-markdown`, `@codemirror/theme-one-dark`) |
| Markdown pipeline | unified + remark-parse + remark-gfm + remark-rehype + rehype-highlight + rehype-stringify |
| Frontmatter parsing | gray-matter (server), inline YAML parser (client) |
| HTML sanitization | DOMPurify |
| Git | simple-git 3 |
| Linter/formatter | Biome 2 |
| Package manager | pnpm (preferred); npm lockfile also present |
| Dev port | 3000 (default) |
| Test runner | None configured |

No test suite exists as of v0.2.0.

## 5. Active Work (as of 2026-05-23)

**Current version:** 0.2.0 (released 2026-03-28)

Recent activity has been dependency maintenance -- 6+ dep update commits since the 0.2.0 release, including security fixes for `postcss` (GHSA-qx2v-qp2m-jg93, patched twice via flat override).

**Known gaps / likely next work:**
- No test suite. Adding Playwright or Vitest is a probable next step.
- Sidebar version badge in `page.tsx` still reads `v0.1.0` (hardcoded string at line 671) even though `package.json` is at `0.2.0`.
- `CONTRIBUTING.md` references a `Sidebar` and `ThemeToggle` component at paths that no longer match the actual component structure (`FileTree` and `ThemeSwitcher` are the real names).
- No formal GitHub Discussions or issue triage visible yet.

## 6. Gotchas

- **Never deploy to serverless.** The `/api/files`, `/api/git`, and `/api/filesystem` routes all call `fs` and `simple-git` with absolute filesystem paths. They will fail on Vercel, Netlify, or any read-only host.
- **CodeMirror and TipTap must be dynamically imported.** Both editors use `dynamic(() => import(...), { ssr: false })` in `page.tsx`. Moving them to static imports breaks SSR. The CodeMirror crash from duplicate `@codemirror/state` was fixed in 0.2.0 by removing the `codemirror` meta-package.
- **Config uses `os.homedir()`, not `process.env.HOME`.** Home dir resolution in `lib/config.ts` is explicit. Do not add logic that reads `HOME` directly.
- **Frontmatter parsing is duplicated.** Server-side uses `gray-matter`. Client-side uses a hand-rolled regex parser in `page.tsx`. They handle edge cases differently. The client parser does not support nested YAML or multi-line values.
- **Image uploads go to an `images/` subdirectory** relative to the active markdown file's directory, not to a global `public/` folder. The API returns both an `/api/images/...` path (for editor preview) and a `./images/filename` path (for the saved markdown). Do not conflate the two.
- **Tailwind v4 class patterns:** Use semantic CSS variable classes (`bg-background`, `text-foreground`, `border-border`) throughout. Do not use hardcoded color utilities like `bg-white` or `dark:bg-gray-900`.
- **pnpm overrides:** `postcss` is pinned to `8.5.15` via `overrides` in `package.json` (security fix). `ajv` is pinned to `6.14.0` via `pnpm.overrides`. Do not remove these without confirming the CVEs are resolved upstream.
- **No auth.** Studio is local-only with no authentication layer. Do not expose the dev or production server to a network.

## 7. Related Projects

| Project | Path | Relationship |
|---------|------|-------------|
| hexcms | `~/Projects/hexcms` | Content API backend that reads the markdown repos Studio manages. Sibling project, not a parent. Studio is the write/edit surface; HexCMS is the read/serve surface. |
| hexops | `~/Projects/hexops` | Hexaxia-Labs monorepo for shared OSS tooling. Studio and HexCMS both moved to the Hexaxia-Labs GitHub org under hexops umbrella. |
| project-registry | `~/Projects/project-registry` | Registry entry for hexcms-studio exists. Use `lookup.sh hexcms-studio` for current status fields. |
