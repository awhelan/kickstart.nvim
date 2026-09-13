# Shared Neovim configuration

This configuration is for everyday editing and code reading on personal and work
machines. It contains no AI account selection. Ordinary yank/delete/paste stays
inside Neovim; Space-y and Space-p explicitly use the system clipboard (Space-Y
and Space-P provide the corresponding line/before variants).

## Baseline — 2026-09-09

The tracked lazy-lock.json matches spa's installed plugins, verified with Neovim
0.12.4 on macOS. Headless startup and explicit clipboard mappings passed there.
Ovid has Neovim 0.11.6 and different installed plugin revisions; its plugins have
not been upgraded or restored to this baseline.

Use `:Lazy restore` to install the locked revisions, not `:Lazy update`. Review
and commit deliberate lockfile updates from a personal machine. First startup
can download plugins, language tools and parsers, so allow internet access during
setup. The plugin lockfile does not pin Neovim, Mason tools, native build tools,
or all parser artifacts. Existing functionality is preserved; no IDE or AI
features were added as part of reconciliation.

On a new Mac, use a compatible Neovim version, install command-line build tools
for native plugins, and run `:checkhealth` after installation. Check file search,
syntax highlighting and clipboard mappings on a disposable file before adopting
the configuration for daily work.

## Headless Linux validation — 2026-09-13

The same plugin lockfile was restored on Amazon Linux 2023 arm64 with Neovim
0.12.5 and tree-sitter CLI 0.27.0 (built locally for the host's glibc). Startup,
explicit clipboard mappings, Lua syntax parsing, Telescope's native fzf
extension, and Lua language-server initialization passed. All configured
parsers and Mason tools were installed. Optional provider/toolchain warnings
from `:checkhealth` do not imply those unrelated runtimes are required.

For scripted setup, set `NVIM_BOOTSTRAP=1` to defer automatic parser and Mason
installs until after the lockfile is restored. Then explicitly wait for
`require('nvim-treesitter').install(require('custom.parsers')):wait(300000)`
and run `:MasonToolsInstallSync` before exiting. The parent dotfiles repository's
`bootstrap-terminal.sh` implements this sequence. Exiting while a native
parser is compiling can leave a stale tree-sitter cache lock; verify no build
is running before removing only the affected stale lock and rerunning setup.

Ovid's older Neovim still needs a separate compatibility/upgrade pass before
restoring this baseline there. Reconciliation of the source histories does
not imply deployment to every machine.
