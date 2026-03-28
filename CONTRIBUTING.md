# Contributing to HexCMS Studio

Thanks for your interest in contributing! Here's how to get started.

## Development Setup

```bash
git clone https://github.com/alamb-hex/hexcms-studio.git
cd hexcms-studio
pnpm install
pnpm dev
```

Open http://localhost:3000 to start.

## Making Changes

1. Fork the repo and create a branch from `main`
2. Make your changes
3. Run `pnpm lint` to check for issues
4. Run `pnpm build` to verify the build succeeds
5. Submit a pull request

## Code Style

- TypeScript throughout - no `any` types without justification
- Use Biome for linting (configured in `biome.json`)
- Follow existing patterns in the codebase
- Tailwind CSS v4 for styling

## Pull Requests

- Keep PRs focused on a single change
- Fill out the PR template
- Include screenshots for UI changes
- Update documentation if your change affects user-facing behavior

## Reporting Issues

- Use the [Bug Report](https://github.com/alamb-hex/hexcms-studio/issues/new?template=bug_report.md) template for bugs
- Use the [Feature Request](https://github.com/alamb-hex/hexcms-studio/issues/new?template=feature_request.md) template for ideas

## License

By contributing, you agree that your contributions will be licensed under the [MIT License](LICENSE).
