Translations: [한국어](README.md) | **English** | [日本語](README.ja.md)

# Flow OS

> A shell system designed to keep you in the flow by reducing the cognitive friction of switching back to high-priority tasks.

## Quick Start

```bash
git clone git@github.com:aimldl/flow-os.git ~/.flow-os
cd ~/.flow-os && chmod +x install.sh && ./install.sh
```

**Restart the shell**

For Bash (Linux, etc.)
```bash
bash
```

or Z Shell (macOS, etc.)

```bash
exec zsh
```

See [INSTALL.md](INSTALL.md) for the full installation guide.


## What is Flow OS?

Flow OS is a shell configuration layer that lives at `~/.flow-os`. It reduces the mental friction of starting work by always showing you exactly where you left off — no remembering, no deciding, just resuming.

```
[ Cognitive Relief ] → [ Frictionless Return ] → [ Instant Resumption ]
```

## How It Works

Opening a terminal shows a dashboard with your TODO list and the path to resume. One command `go` drops you directly into your last working directory.

```
terminal start → Dashboard → go → work → clean → quit
```

## Structure

```
~/
├── .flow-os/          # OS layer (this repo)
└── my/                # Data layer
    ├── .flow/         # State layer
    │   ├── resume.md
    │   ├── todo.md
    │   ├── memo.md
    │   ├── next.md
    │   └── v-*.md     # Monthly archives (v marks completed state)
    ├── craft/         # Purpose layer
    ├── forge/
    ├── quests/
    ├── temp/
    └── vault/
```

## Key Commands

| Command | What it does |
|---------|-------------|
| `go` | cd to path recorded in resume.md |
| `fmemo <text>` | Capture a stray thought to memo.md |
| `q` | git push → save resume → review → close terminal |
| `qq` | git push → save resume → close terminal (no review) |
| `cleanmemo` | Archive memo.md to history |
| `cleantodo` | Archive completed items, keep open items |

## Documentation

| Doc | Description |
|-----|-------------|
| [docs/workflow.md](docs/workflow.md) | Daily workflow guide |
| [docs/commands.md](docs/commands.md) | Full command reference |
| [docs/philosophy.md](docs/philosophy.md) | Design philosophy & brain science |
| [docs/git-sync.md](docs/git-sync.md) | Git automation setup |
| [docs/memo-design.md](docs/memo-design.md) | Memo system design |
