# Repository layout

Each product lives in `projects/<name>/` and can evolve over multiple releases. Start from its README to run it, find its current design, or pick up work on it. The repository root is the Obsidian vault for all of NOR Studio. Use relative Markdown links so the same notes and project references work on GitHub. Keep personal Obsidian settings and plugins local.

Create each directory with its first useful file. This map shows where future content belongs.

```text
nor-studio/
├── README.md
├── AGENTS.md
├── projects/<name>/
│   ├── README.md
│   ├── AGENTS.md                 # Project-specific instructions, when needed
│   ├── docs/                     # Current product behavior and its rationale
│   ├── prototypes/<issue>-<topic>/
│   └── …                         # Source, dependencies and build configuration
├── docs/
│   ├── notes/                    # Findings and lessons, organized by topic
│   ├── agents/                   # Procedures that apply across the studio
│   └── adr/                      # Studio-wide architecture decisions
├── tools/                        # Tools used to work on the repository
├── config/                       # Shared, non-sensitive configuration
└── .github/                      # GitHub configuration and workflows
```

## Requirements and design

Keep the spec and implementation tickets in [GitHub Issues](agents/issue-tracker.md). Link the implementation PR to the issues it addresses. The project README links to the current Figma design. An issue can link directly to the frame or prototype used for that change, with a version link when the reviewed design needs to remain identifiable.

Keep issue-specific design material in the project that owns it. For example, `prototypes/42-quick-capture/flow.excalidraw` could preserve a sketch that informed the capture flow. A text prototype or a short comparison can sit beside it if it helps explain the choice. Link to that material from the issue. A Figma link alone can live directly in the issue.

For a runnable prototype made with Matt Pocock's `/prototype` skill, try the variants on their intended page or near the module under study. Preserve the experiment on its own branch and link it from the implementation issue with the question it answered and the outcome. The product ships the selected behavior.

## Knowledge after delivery

Update the project's `docs/` when a change affects what future developers need to know about the product, including the reasons for decisions that still constrain it. Use the [domain documentation conventions](agents/domain.md) for vocabulary and architecture records.

Review a practice in a dedicated conversation using the original discussion, design artifacts and observed outcomes. The person doing the practice confirms their own intent and judgment. Keep the conclusions where they will be used next. A lesson about when to test an interaction belongs in `docs/notes/` under that topic, with the issue as an example. A verified tool procedure for future agents belongs in `docs/agents/`, linked from the root `AGENTS.md`. Instructions specific to one product belong in its own `AGENTS.md`.

## Independent builds

Each product declares its own build inputs. Use a clean checkout in CI to check that the project can build with only its declared dependencies. Shared download caches are fine.

| Stack | Files kept with the project | Local installation |
| --- | --- | --- |
| Node | `package.json` and the package manager's lockfile | Install in the project. Keep `node_modules/` out of Git. |
| Python | Dependency declaration such as `pyproject.toml` and the application's lockfile for the chosen tool | Recreate the project's `.venv/` locally. |
| Go | `go.mod` per independent module and `go.sum` when generated | The Go toolchain can share its module cache across projects. |

Give studio tools their own dependencies under `tools/`, or at the root when required by the tool. A product build should succeed without installing the studio tools. Put product-specific environment settings in the project. Use root `config/` for settings consumed across the studio, and use a tool's required discovery path where applicable.

For a containerized product, put its `Dockerfile` and `.dockerignore` in the project and run `docker build .` there. Use that same project directory as the CI build context. Exclude local environments, credentials and unused design materials from the context. Add `compose.yaml` when running the project involves companion services such as a database.

## Public repository and secrets

Keep credentials in a local keychain or secret manager and supply them to the process that needs them. If a tool requires a local environment file, keep it ignored. Commit only variable names and dummy values in examples such as `.env.example`. Treat browser bundles, issue bodies, prototype branches, screenshots, design links, logs and notes as public material. Check what a design link exposes before sharing it.

Scope deployment credentials to the project and environment. Prefer short-lived credentials through OIDC when supported, or inject secrets from GitHub Environments or the deployment platform. Use a BuildKit secret mount when a Docker build needs credentials. Enable and verify GitHub push protection as an additional check. If a real credential becomes public, revoke or rotate it first.
