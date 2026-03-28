# Changelog

All notable changes to HexCMS Studio will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.2.0] - 2026-03-28

### Fixed
- Inline code backticks rendering as literal characters in preview
- CodeMirror crash when switching to code editor mode (SSR module duplication)
- Duplicate `@codemirror/state` versions from `codemirror` meta-package

### Security
- Upload route: path traversal protection, 10MB file size limit, MIME type validation
- Markdown renderer: removed `allowDangerousHtml` to prevent script injection
- Preview: iframes restricted to YouTube and Vimeo sources only
- Config: use `os.homedir()` instead of unreliable `HOME` env var

### Changed
- License reverted from AGPL-3.0 to MIT

### Removed
- Unused Zustand store (`src/stores/editor.ts`)
- Unused `markdown-it` and `codemirror` dependencies
- Unused exports from `lib/markdown.ts`

### Added
- CONTRIBUTING.md, CODE_OF_CONDUCT.md, SECURITY.md
- GitHub Sponsors funding configuration
- Issue template config (disables blank issues, adds security contact)

## [0.1.0] - 2026-01-14

### Added
- Visual WYSIWYG editor with TipTap
- Raw markdown code editor with CodeMirror 6
- YouTube embed support - paste URLs directly, auto-converted to responsive iframes
- Live markdown preview with syntax highlighting
- Frontmatter form editor for post metadata
- Multi-repository support with local Git repos
- Git integration (stage, commit, push, pull)
- File browser for adding repositories
- Multi-theme support (Light, Dark, Midnight, Sepia)
- AI-powered meta description generation

### Technical
- Built with Next.js 16 (App Router, Turbopack)
- Tailwind CSS v4 styling
- TypeScript throughout
- Local-only design for filesystem access
