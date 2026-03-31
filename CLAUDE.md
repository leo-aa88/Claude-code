# Claude Code — Project Map

## How to Build & Run

```bash
bun install          # install dependencies
bun run build        # bundles to dist/cli.js (~23MB)
bun dist/cli.js      # run it
```

## Project Structure

```
claude-code/
├── dist/                          # Build output (gitignored)
│   └── cli.js                     # Bundled CLI (23MB, single file)
│
├── src/                           # Main source (1,929 files) — leaked from Anthropic
│   ├── main.tsx                   # CLI entrypoint — Commander.js parser, all flags
│   ├── entrypoints/
│   │   ├── cli.tsx                # Bootstrap — version check, fast-paths
│   │   ├── init.ts                # Initialization — telemetry, config, auth
│   │   ├── mcp.ts                 # MCP server entrypoint
│   │   └── sdk/                   # Agent SDK types
│   │       ├── coreSchemas.ts     # Zod schemas (source of truth for types)
│   │       ├── coreTypes.ts       # Re-exports generated types
│   │       ├── coreTypes.generated.ts  # [GENERATED] from coreSchemas.ts
│   │       ├── runtimeTypes.ts    # [STUB] SDK runtime types
│   │       ├── toolTypes.ts       # [STUB] SDK tool types
│   │       └── settingsTypes.generated.ts  # [STUB] Settings types
│   │
│   ├── commands/                  # Slash commands (~50)
│   │   ├── agents-platform/       # [STUB] Ant-only
│   │   └── assistant/             # [STUB] Assistant wizard
│   │
│   ├── tools/                     # Agent tools (~40)
│   │   ├── BashTool/              # Shell execution
│   │   ├── FileEditTool/          # File editing
│   │   ├── FileReadTool/          # File reading
│   │   ├── FileWriteTool/         # File writing
│   │   ├── GlobTool/              # File search
│   │   ├── GrepTool/              # Content search
│   │   ├── AgentTool/             # Subagent spawning
│   │   ├── WebFetchTool/          # HTTP fetching
│   │   ├── TungstenTool/          # [STUB] Ant-only debug tool
│   │   ├── REPLTool/              # [STUB] Ant-only REPL
│   │   ├── SuggestBackgroundPRTool/ # [STUB] Ant-only
│   │   ├── VerifyPlanExecutionTool/ # [STUB] Env-gated
│   │   └── WorkflowTool/          # [STUB] Feature-gated (WORKFLOW_SCRIPTS)
│   │
│   ├── components/                # React (Ink) UI components (~140)
│   │   ├── agents/
│   │   │   └── SnapshotUpdateDialog.tsx  # [STUB]
│   │   ├── design-system/         # Theme, colors, tokens
│   │   ├── LogoV2/                # Welcome screen, release notes
│   │   ├── Message.tsx            # Message rendering
│   │   ├── StructuredDiff/        # Syntax-highlighted diffs
│   │   └── permissions/           # Permission approval dialogs
│   │
│   ├── screens/
│   │   └── REPL.tsx               # Main interactive screen (2800+ lines)
│   │
│   ├── ink/                       # Custom Ink fork (terminal React renderer)
│   │   ├── layout/                # Flexbox layout engine
│   │   ├── components/            # Box, Text, ScrollBox, Button, etc.
│   │   ├── hooks/                 # useInput, useStdin, useSelection, etc.
│   │   ├── events/                # Click, keyboard, focus events
│   │   ├── termio/                # Terminal I/O, ANSI parsing
│   │   └── reconciler.ts          # React reconciler
│   │
│   ├── services/
│   │   ├── api/                   # Anthropic API client, streaming, errors
│   │   ├── mcp/                   # MCP client/server implementation
│   │   ├── oauth/                 # OAuth flow
│   │   ├── analytics/             # Telemetry, GrowthBook, DataDog
│   │   ├── lsp/                   # Language Server Protocol
│   │   ├── compact/               # Context compaction
│   │   │   ├── snipCompact.ts     # [STUB] Feature-gated (HISTORY_SNIP)
│   │   │   └── cachedMicrocompact.ts  # [STUB] Feature-gated
│   │   └── contextCollapse/       # [STUB] Not in leak
│   │
│   ├── native-ts/                 # Pure TypeScript ports of native modules
│   │   ├── yoga-layout/           # Flexbox engine (port of Meta's Yoga)
│   │   ├── color-diff/            # Syntax-highlighted diffs (port of Rust module)
│   │   └── file-index/            # Fuzzy file search (port of nucleo)
│   │
│   ├── constants/
│   │   ├── prompts.ts             # FULL system prompt — the actual instructions sent to Claude
│   │   ├── oauth.ts               # OAuth config (client IDs, endpoints)
│   │   └── product.ts             # Product constants
│   │
│   ├── utils/
│   │   ├── autoUpdater.ts         # Version check [PATCHED — remote check disabled]
│   │   ├── computerUse/           # Computer use integration layer
│   │   │   └── executor.ts        # 22KB CLI executor — wraps Swift/Rust native modules
│   │   ├── claudeInChrome/        # Chrome integration layer
│   │   ├── sandbox/               # Sandbox adapter
│   │   ├── settings/              # Settings system
│   │   ├── model/                 # Model selection, aliases
│   │   ├── auth.ts                # Authentication
│   │   ├── protectedNamespace.ts  # [STUB] Ant-only
│   │   └── filePersistence/
│   │       └── types.ts           # [STUB]
│   │
│   ├── assistant/
│   │   ├── sessionHistory.ts      # Session history
│   │   └── AssistantSessionChooser.tsx  # [STUB]
│   │
│   ├── vim/                       # Vim mode (motions, operators, text objects)
│   ├── state/                     # App state management
│   ├── hooks/                     # React hooks
│   ├── types/
│   │   └── connectorText.ts       # [STUB]
│   ├── bridge/                    # Cloud session bridging
│   ├── coordinator/               # Multi-agent coordinator
│   ├── plugins/                   # Plugin system
│   ├── skills/                    # Built-in skills
│   │   └── bundled/verify/        # [STUB] Placeholder .md files
│   ├── bootstrap/                 # Bootstrap/startup state
│   └── voice/                     # Voice mode
│
├── stubs/                         # Extracted proprietary source code
│   ├── @ant/                      # Private Anthropic packages (28 files)
│   │   ├── computer-use-mcp/      # Computer Use MCP server
│   │   │   └── src/
│   │   │       ├── index.ts       # Exports
│   │   │       ├── toolCalls.ts   # 137KB — full tool implementation
│   │   │       ├── tools.ts       # Tool definitions
│   │   │       ├── mcpServer.ts   # MCP server setup
│   │   │       ├── types.ts       # All CU types
│   │   │       ├── deniedApps.ts  # App blocklist
│   │   │       ├── keyBlocklist.ts # Key combo blocklist
│   │   │       ├── sentinelApps.ts # Sentinel app detection
│   │   │       ├── imageResize.ts # Screenshot resizing
│   │   │       ├── pixelCompare.ts # Click target validation
│   │   │       ├── executor.ts    # [STUB] Native Swift/Rust bridge interface
│   │   │       └── subGates.ts    # [STUB] Permission sub-gates
│   │   │
│   │   ├── claude-for-chrome-mcp/ # Chrome automation (8 source files)
│   │   │   └── src/
│   │   │       ├── index.ts       # Exports
│   │   │       ├── bridgeClient.ts # 37KB — Chrome bridge via WebSocket
│   │   │       ├── browserTools.ts # 25KB — browser tool definitions
│   │   │       ├── mcpServer.ts   # MCP server
│   │   │       ├── mcpSocketClient.ts # WebSocket client
│   │   │       ├── mcpSocketPool.ts   # Connection pooling
│   │   │       ├── toolCalls.ts   # Tool call handling
│   │   │       └── types.ts       # Types
│   │   │
│   │   ├── computer-use-swift/    # macOS native bridge
│   │   │   └── js/index.js        # JS loader for Swift binary
│   │   │
│   │   └── computer-use-input/    # Input device bridge
│   │       └── js/index.js        # JS loader for Rust binary
│   │
│   ├── @anthropic-ai/            # Anthropic SDK sources (105 files)
│   │   ├── sandbox-runtime/       # Sandbox system (14 files, 162KB)
│   │   │   └── dist/
│   │   │       ├── sandbox/
│   │   │       │   ├── sandbox-manager.js    # 31KB — core orchestrator
│   │   │       │   ├── sandbox-config.js     # Config/schema
│   │   │       │   ├── macos-sandbox-utils.js # 28KB — macOS Seatbelt profiles
│   │   │       │   ├── linux-sandbox-utils.js # 42KB — Linux namespaces + seccomp
│   │   │       │   ├── generate-seccomp-filter.js # 12KB — raw BPF bytecode gen
│   │   │       │   ├── http-proxy.js         # HTTP egress proxy
│   │   │       │   ├── socks-proxy.js        # SOCKS proxy
│   │   │       │   └── sandbox-violation-store.js
│   │   │       └── utils/
│   │   │
│   │   ├── mcpb/                  # MCP Bundle tools (11 files, 75KB)
│   │   │   └── dist/
│   │   │       ├── cli/           # pack.js, unpack.js, init.js (26KB scaffolder)
│   │   │       ├── node/          # files.js, sign.js (12KB), validate.js
│   │   │       └── shared/        # config.js, log.js
│   │   │
│   │   ├── sdk/                   # Anthropic SDK source (40+ files, 232KB)
│   │   │   ├── client.mjs         # 28KB — main API client
│   │   │   ├── resources/         # API resources (messages, models, batches, skills)
│   │   │   ├── lib/
│   │   │   │   ├── MessageStream.mjs     # 29KB — response streaming
│   │   │   │   ├── BetaMessageStream.mjs # 31KB — beta streaming
│   │   │   │   ├── tools/BetaToolRunner.mjs # 18KB — tool use loop
│   │   │   │   ├── tools/CompactionControl.mjs # Context compaction
│   │   │   │   └── parser.mjs           # Partial JSON streaming parser
│   │   │   └── internal/          # Headers, auth, request handling
│   │   │
│   │   ├── bedrock-sdk/           # AWS Bedrock (12 files, 36KB)
│   │   │   ├── client.mjs         # Bedrock API client
│   │   │   └── core/auth.mjs      # SigV4 signing
│   │   │
│   │   ├── vertex-sdk/            # GCP Vertex (7 files, 13KB)
│   │   │   └── client.mjs         # Vertex AI client with Google auth
│   │   │
│   │   └── foundry-sdk/           # Foundry (8 files, 16KB)
│   │       └── client.mjs         # Foundry client with custom auth
│   │
│   └── downloads/                 # Additional packages downloaded from npm
│       ├── tokenizer/             # Claude's BPE tokenizer
│       │   ├── claude.json        # 680KB — full vocabulary (64,739 tokens)
│       │   ├── index.ts           # Tokenizer implementation
│       │   └── tests/             # Test suite
│       │
│       ├── claude-trace/          # OTEL trace viewer for Claude sessions
│       │   ├── dist/server.cjs    # 838KB — trace server
│       │   └── viewer/dist/       # Web UI (HTML + JS + CSS)
│       │
│       └── claude-agent-sdk/      # Agent SDK package
│           ├── sdk.mjs            # Main SDK — spawns CLI as subprocess
│           ├── sdk.d.ts           # Full type definitions
│           ├── bridge.mjs         # Session bridge protocol
│           ├── browser-sdk.js     # Browser-compatible SDK
│           ├── embed.js           # Embedding helpers
│           └── manifest.json      # SDK manifest
│
├── shims/                         # Build-time shims
│   ├── bun-bundle.ts              # Runtime shim for feature() — returns false
│   ├── bun-bundle.d.ts            # Type declaration
│   └── globals.d.ts               # MACRO.* type declarations
│
├── scripts/
│   └── generate-sdk-types.ts      # Generates coreTypes.generated.ts from Zod schemas
│
├── vendor/                        # Native binaries from npm package (gitignored)
│   ├── ripgrep/                   # rg binary (arm64/x64 for darwin/linux/win32)
│   └── audio-capture/             # Voice capture native addon (all platforms)
│
├── build.ts                       # Bun build script
├── package.json                   # Dependencies & scripts
├── tsconfig.json                  # TypeScript config
├── bun.lock                       # Bun lockfile
├── .gitignore
├── LICENSE                        # MIT
├── README.md
│
├── cli.js.map                     # Original 57MB source map (gitignored, saved locally)
└── sourcemap-extract.tar.gz       # Full extraction archive (gitignored, saved locally)
```

## What's Patched

- `src/utils/autoUpdater.ts` — remote version check disabled (line 72: early return)
- `build.ts` — MACRO.VERSION set to `2.1.88`, all feature() flags return false

## What's Stubbed (marked [STUB] above)

Files that exist but contain minimal placeholder code because:
1. **Not in leak** — source files excluded from the original zip
2. **Native bindings** — Rust/Swift code can't be in a source map (executor.ts, subGates.ts)
3. **Generated files** — were generated by build scripts (coreTypes.generated.ts — we regenerated this)
4. **Ant-only** — internal Anthropic tools gated by `USER_TYPE === 'ant'`

## Feature Flags (all disabled)

The source uses `feature('FLAG_NAME')` from `bun:bundle` for dead code elimination.
Our shim returns `false` for all flags. Known flags:
VOICE_MODE, COORDINATOR_MODE, KAIROS, PROACTIVE, ULTRAPLAN, BRIDGE_MODE,
BG_SESSIONS, WORKFLOW_SCRIPTS, TRANSCRIPT_CLASSIFIER, TOKEN_BUDGET,
HISTORY_SNIP, BUDDY, TEAMMEM, AGENT_TRIGGERS, WEB_BROWSER_TOOL,
MESSAGE_ACTIONS, HOOK_PROMPTS, CACHED_MICROCOMPACT, CHICAGO_MCP,
ABLATION_BASELINE, DUMP_SYSTEM_PROMPT

## What Works vs What Doesn't

### Fully Working
- All standard tools (Bash, Edit, Read, Write, Grep, Glob, WebFetch, WebSearch, Agent)
- Terminal UI (full React/Ink REPL with custom flexbox layout)
- OAuth authentication (same flow as official)
- MCP server support
- Slash commands (/help, /clear, /compact, /resume, etc.)
- Session persistence and resume
- Plugin system
- Vim mode
- Sandbox mode (real @anthropic-ai/sandbox-runtime from npm)
- AWS Bedrock / GCP Vertex / Foundry backends (real SDKs from npm)
- Agent SDK integration (set `pathToClaudeCodeExecutable` to `dist/cli.js`)

### Not Working
- **Computer Use** — full logic extracted (137KB toolCalls.ts) but needs native
  Swift/Rust binaries for screen capture and input. Could be rebuilt using macOS
  system commands (screencapture, osascript, pbcopy/pbpaste).
- **Feature-flagged features** — voice, coordinator, ultraplan, etc. All disabled
  via feature() shim. The source is there but many depend on backend infra.
- **Ant-only tools** — TungstenTool, REPLTool, SuggestBackgroundPRTool. Internal
  tools never available in external builds.

## Source Extraction Summary

| Source | Method | Files | What |
|--------|--------|-------|------|
| Original leak | .map file on R2 bucket | 1,929 | Full src/ directory |
| npm source map | `cli.js.map` in `@anthropic-ai/claude-code` | 4,756 total | Everything bundled into the CLI |
| npm source map | Same file, `@ant/*` entries | 20 | Computer use + Chrome (private) |
| npm source map | Same file, `@anthropic-ai/*` entries | 105 | SDK, sandbox, mcpb, bedrock, vertex, foundry |
| npm registry | `npm pack @anthropic-ai/tokenizer` | 15 | Claude's BPE tokenizer + vocabulary |
| npm registry | `npm pack @anthropic-ai/claude-trace` | 6 | OTEL session trace viewer |
| npm registry | `npm pack @anthropic-ai/claude-agent-sdk` | 18 | Agent SDK source + types |

## All @anthropic-ai npm Packages (as of 2026-03-31)

| Package | On npm? | In our repo? | Status |
|---------|---------|-------------|--------|
| `@anthropic-ai/claude-code` | Yes | src/ + stubs/ | **Full source extracted** |
| `@anthropic-ai/claude-agent-sdk` | Yes | stubs/downloads/ | **Downloaded** |
| `@anthropic-ai/sdk` | Yes | stubs/@anthropic-ai/sdk/ | **Source from map + npm install** |
| `@anthropic-ai/bedrock-sdk` | Yes | stubs/@anthropic-ai/bedrock-sdk/ | **Source from map + npm install** |
| `@anthropic-ai/vertex-sdk` | Yes | stubs/@anthropic-ai/vertex-sdk/ | **Source from map + npm install** |
| `@anthropic-ai/foundry-sdk` | Yes | stubs/@anthropic-ai/foundry-sdk/ | **Source from map + npm install** |
| `@anthropic-ai/sandbox-runtime` | Yes | stubs/@anthropic-ai/sandbox-runtime/ | **Source from map + npm install** |
| `@anthropic-ai/mcpb` | Yes | stubs/@anthropic-ai/mcpb/ | **Source from map + npm install** |
| `@anthropic-ai/tokenizer` | Yes | stubs/downloads/tokenizer/ | **Downloaded** |
| `@anthropic-ai/claude-trace` | Yes | stubs/downloads/claude-trace/ | **Downloaded** |
| `@ant/computer-use-mcp` | **No** (private) | stubs/@ant/computer-use-mcp/ | **Source from map** |
| `@ant/claude-for-chrome-mcp` | **No** (private) | stubs/@ant/claude-for-chrome-mcp/ | **Source from map** |
| `@ant/computer-use-swift` | **No** (private) | stubs/@ant/computer-use-swift/ | **JS loader only** (binary missing) |
| `@ant/computer-use-input` | **No** (private) | stubs/@ant/computer-use-input/ | **JS loader only** (binary missing) |
