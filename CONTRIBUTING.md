# Contributing to Canva Linux App

Thank you for your interest in contributing! This document provides guidelines for contributing to the project.

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md). By participating, you agree to uphold this code.

## Ways to Contribute

- 🐛 **Bug Reports**: File issues with reproduction steps
- 💡 **Feature Requests**: Propose new features or improvements
- 📝 **Documentation**: Improve docs, add examples, fix typos
- 🧪 **Testing**: Write tests, report compatibility issues
- 💻 **Code**: Submit pull requests for fixes and features
- 🎨 **Design**: UI/UX improvements, icons, themes
- 🌍 **Localization**: Translate interface text
- 📦 **Packaging**: Improve distribution packages

## Getting Started

### Development Setup

```bash
# 1. Fork and clone
git clone https://github.com/your-username/canva-linux-app.git
cd canva-linux-app

# 2. Install prerequisites (see README.md)

# 3. Configure environment
cp .env.example .env
# Add your Canva OAuth credentials

# 4. Run development server
cargo tauri dev
```

### Project Structure

```
canva-linux-app/
├── src/              # Rust backend
├── frontend/         # TypeScript frontend (injection layer)
├── packaging/        # Distribution packages
├── tests/            # Integration & E2E tests
├── docs/             # Architecture & specifications
└── scripts/          # Build & release scripts
```

## Development Workflow

### Branch Strategy

| Branch | Purpose |
|--------|---------|
| `main` | Production releases (tagged) |
| `develop` | Integration branch for next release |
| `feature/*` | New features |
| `fix/*` | Bug fixes |
| `docs/*` | Documentation updates |
| `release/*` | Release preparation |

### Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Types**:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `style`: Formatting, missing semicolons, etc.
- `refactor`: Code change that neither fixes nor adds feature
- `perf`: Performance improvement
- `test`: Adding missing tests
- `chore`: Maintenance tasks

**Examples**:
```
feat(auth): add PKCE OAuth flow
fix(export): handle large file downloads
docs(arch): update system architecture diagram
refactor(cache): simplify cleanup logic
test(fs): add portal dialog tests
chore(deps): update tauri to 2.1
```

### Pull Request Process

1. **Create PR** against `develop` branch
2. **Fill PR template** with:
   - Description of changes
   - Related issue number
   - Testing performed
   - Screenshots (if UI changes)
3. **Ensure checks pass**:
   - `cargo test`
   - `cargo clippy -- -D warnings`
   - `cargo fmt --check`
   - `pnpm -C frontend test`
   - `pnpm -C frontend lint`
   - `pnpm -C tests/e2e test` (if applicable)
4. **Request review** from maintainers
5. **Address feedback** - push new commits to same branch
6. **Squash and merge** after approval

### Code Review Guidelines

**Reviewers check**:
- [ ] Code compiles and tests pass
- [ ] Follows project style and patterns
- [ ] Adequate test coverage
- [ ] Documentation updated
- [ ] No security issues
- [ ] Performance considerations
- [ ] Accessibility maintained

**Authors should**:
- Keep PRs focused and small (<500 lines ideal)
- Write clear commit messages
- Respond to feedback promptly
- Update PR description as things change

## Coding Standards

### Rust (Backend)

```rust
// Use rustfmt (default config)
cargo fmt

// Use clippy
cargo clippy -- -D warnings

// Naming
snake_case for functions, variables, modules
PascalCase for types, traits, enums
UPPER_SNAKE_CASE for constants

// Error handling
// Use Result<T, AppError> for fallible operations
// Use thiserror for error types
// Use anyhow for application errors

// Async
// Use tokio for async runtime
// Avoid blocking in async context
// Use tokio::task::spawn_blocking for CPU-intensive work

// Documentation
/// Public APIs must have rustdoc comments
/// Include examples where helpful
```

### TypeScript (Frontend)

```typescript
// Use ESLint + Prettier
pnpm lint
pnpm format

// Naming
camelCase for variables, functions
PascalCase for components, types, interfaces
UPPER_SNAKE_CASE for constants

// Types
// Prefer interfaces over type aliases for object shapes
// Use explicit return types for public functions
// Avoid any - use unknown or proper types

// React
// Use functional components with hooks
// Use TypeScript for all components
// Follow React best practices
```

### Git

```bash
# Sign commits (recommended)
git config --global commit.gpgsign true

# Keep history clean
# Rebase feature branches on develop
git fetch origin
git rebase origin/develop
```

## Testing Requirements

### Unit Tests
- Target: >80% coverage for new code
- Run: `cargo test` / `pnpm test`

### Integration Tests
- Test Tauri commands end-to-end
- Mock external dependencies (Canva API, portals)
- Run: `cargo test --test integration_test`

### E2E Tests
- Critical user flows (login, create design, export)
- Run: `pnpm -C tests/e2e test`
- Visual regression for UI changes

### Manual Testing Checklist
Before submitting PR with UI changes:
- [ ] Test on Ubuntu 24.04 (GNOME, Wayland)
- [ ] Test on Fedora 39 (KDE, Wayland)
- [ ] Test dark/light theme
- [ ] Test HiDPI (150%, 200%)
- [ ] Test keyboard navigation
- [ ] Test screen reader (Orca/NVDA)

## Documentation

### Required Updates
- [ ] Code comments for public APIs
- [ ] README.md if user-facing changes
- [ ] CHANGELOG.md entry (see format below)
- [ ] Architecture docs if structural changes
- [ ] API docs if endpoints change

### CHANGELOG Format

```markdown
## [1.1.0] - 2026-12-01

### Added
- New feature X (#123)

### Changed
- Improved performance of Y (#124)

### Fixed
- Bug in export pipeline (#125)

### Security
- Updated dependency Z to fix CVE-XXXX
```

## Release Process

### Versioning
[Semantic Versioning](https://semver.org/): `MAJOR.MINOR.PATCH`

### Release Checklist
- [ ] All CI checks pass
- [ ] CHANGELOG updated
- [ ] Version bumped in `Cargo.toml`, `package.json`, `tauri.conf.json`
- [ ] Tag created: `git tag v1.2.3`
- [ ] GitHub Release created with artifacts
- [ ] Flatpak/Snap updates submitted
- [ ] Announcement posted

## Getting Help

- **Questions**: [GitHub Discussions](https://github.com/canva-linux/canva-linux-app/discussions)
- **Bugs**: [GitHub Issues](https://github.com/canva-linux/canva-linux-app/issues)
- **Security**: security@canva-linux.app
- **Chat**: [Discord](https://discord.gg/canva-linux) / [Matrix](https://matrix.to/#/#canva-linux:matrix.org)

## Recognition

Contributors are recognized in:
- [CONTRIBUTORS.md](CONTRIBUTORS.md)
- Release notes
- Project website

---

*Thank you for contributing to Canva Linux App!*