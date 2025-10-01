# changelogen-rs

💅 **Beautiful Changelogs using Conventional Commits** - Rust port of [@unjs/changelogen](https://github.com/unjs/changelogen)

[![CI](https://github.com/MuntasirSZN/changelogen-rs/workflows/CI/badge.svg)](https://github.com/MuntasirSZN/changelogen-rs/actions)

## Status

✨ **MVP Complete** - Core features implemented with parity to the JavaScript version.

**Parity Achieved**: This Rust implementation aims for output parity with [@unjs/changelogen](https://github.com/unjs/changelogen). Commit classification, version inference, and markdown output should match the JavaScript version exactly. See [PARITY_SPEC.md](PARITY_SPEC.md) for detailed requirements.

**Distribution**: Available via both Cargo (for Rust projects) and npm (for JavaScript/Node.js projects via NAPI-RS bindings). See [NPM_INTEGRATION.md](NPM_INTEGRATION.md) for npm package details.

See [tasks.md](tasks.md) for detailed roadmap and implementation status.

## Installation

### Rust (via Cargo)

```bash
cargo install changelogen
```

### JavaScript/Node.js (via npm)

```bash
npm install changelogen
# or
yarn add changelogen
# or
pnpm add changelogen
```

See [README-NPM.md](README-NPM.md) for npm-specific documentation.

## Features

- ✅ **Conventional Commit Parsing** - Supports standard commit message formats
- ✅ **Configurable Types** - Customize commit types, emojis, and semver impact
- ✅ **Multiple Providers** - GitHub, GitLab, Bitbucket repository detection
- ✅ **Parallel Processing** - Fast parsing of large commit histories
- ✅ **Author Attribution** - Automatic contributor detection and acknowledgment
- ✅ **Semantic Versioning** - Automatic version bumping based on changes
- ✅ **Idempotent Operation** - Safe to rerun without duplicating entries
- ✅ **Clean Code Quality** - No unwrap() outside tests, clippy clean, comprehensive test coverage
- ✅ **npm Package** - JavaScript/Node.js bindings via NAPI-RS

## Quick Start

### CLI Usage (Rust)

```bash
# Install from source (cargo publish pending)
git clone https://github.com/MuntasirSZN/changelogen-rs
cd changelogen-rs
cargo install --path .

# Basic usage
changelogen show                    # Show next version
changelogen generate                # Generate changelog block  
changelogen generate --write        # Update CHANGELOG.md
changelogen release                 # Full release pipeline (tag + changelog)
changelogen --help                  # See all options
```

### JavaScript/Node.js API

```javascript
import { generate, release, showVersion } from 'changelogen';

// Generate changelog
const result = await generate({
  from: 'v1.0.0',
  to: 'HEAD',
  write: true
});

console.log(`Generated changelog with ${result.commits} commits`);

// Perform release
const releaseResult = await release({
  dryRun: true,
  yes: true
});

console.log(`Release ${releaseResult.newVersion}`);

// Show next version
const version = await showVersion();
console.log(`Next version: ${version}`);
```

See [README-NPM.md](README-NPM.md) for full npm API documentation and [examples/](examples/) directory for more examples.

## Configuration

Create `changelogen.toml` in your project root:

```toml
# Customize commit types
[types.feat]
title = "✨ Features"
semver = "minor"

[types.fix] 
title = "🐛 Bug Fixes"
semver = "patch"

# Scope mapping
[scopeMap]
"ui" = "frontend"
"api" = "backend"

# GitHub token for release syncing
[tokens]
github = "${GITHUB_TOKEN}"
```

Or use `[package.metadata.changelogen]` in `Cargo.toml`.

## Differences from JS Version

### Intentional Differences

| Feature                  | JavaScript Version               | Rust Version                       | Notes                                    |
| ------------------------ | -------------------------------- | ---------------------------------- | ---------------------------------------- |
| **Configuration**        | JSON/JS files                    | TOML files                         | Rust ecosystem standard                  |
| **Config location**      | `package.json` or `.changelogrc` | `changelogen.toml` or `Cargo.toml` | Cargo integration                        |
| **Parallel processing**  | Single-threaded                  | Optional multi-threaded (rayon)    | Performance optimization for large repos |
| **Package distribution** | npm                              | Cargo (npm via NAPI-RS planned)    | Native Rust tooling                      |
| **Binary size**          | Node.js required (~50MB+)        | Static binary (~5MB)               | No runtime dependency                    |

### Parity Guarantees

These behaviors match the JavaScript version **exactly**:

- ✅ **Commit classification**: Type detection, scope parsing, breaking change identification
- ✅ **Version inference**: Semver rules including pre-1.0 adjustments
- ✅ **Markdown output**: Format, section ordering, reference linking
- ✅ **Filtering rules**: Disabled types, `chore(deps)` handling
- ✅ **Contributors**: Deduplication, co-author detection, ordering
- ✅ **Idempotence**: Safe to rerun without duplication

See [PARITY_SPEC.md](PARITY_SPEC.md) for comprehensive parity documentation and verification strategy.

## Development

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed contribution guidelines.

### Quick Commands

```bash
# Using just (recommended)
just check          # Run all checks (format, lint, test)
just test           # Run tests
just lint           # Run clippy
just fmt            # Format code

# Manual commands
cargo build
cargo test
cargo clippy -- -D warnings
cargo fmt --all

# Run benchmarks
cargo bench
```

### Environment Variables

```bash
CHANGELOGEN_PARALLEL_THRESHOLD=50  # Parallel processing threshold (default: 50)
RUST_LOG=debug                     # Enable debug logging
GITHUB_TOKEN=xxx                   # GitHub API token for release sync
```

## Goals

- Parity with JavaScript Version
- Support more than just npm (cargo, go and others, contributions needed!)
- Performance
- Security
- else?

## License

MIT - See [LICENSE](LICENSE) for details

## Acknowledgments

This project is a Rust port of [@unjs/changelogen](https://github.com/unjs/changelogen) by the UnJS team.
