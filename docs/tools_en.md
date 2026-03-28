# Tools Reference

Aivy OS provides AI with a complete toolchain — not just a chat interface, but tools that let AI actually operate your computer.

> **Note:** The following tools are autonomously used by the AI to decide when and how to combine them. The framework doesn't hardcode any calling logic — everything is left to the AI's emergence.

---

## Table of Contents

- [Built-in Tools](#built-in-tools)
  - [file_tool — File Operations](#file_tool--file-operations)
  - [cmd_tool — System Commands](#cmd_tool--system-commands)
  - [python_executor — Python Executor](#python_executor--python-executor)
  - [browser_control — Browser Control](#browser_control--browser-control)
  - [computer_use — Desktop Control](#computer_use--desktop-control)
  - [web_search — Web Search & Crawling](#web_search--web-search--crawling)
  - [doc_tool — Document Processing](#doc_tool--document-processing)
  - [ide_tool — IDE Control](#ide_tool--ide-control)
  - [system_monitor — System Monitor](#system_monitor--system-monitor)
  - [undo_tool — File Undo](#undo_tool--file-undo)
  - [global_search — Global File Search](#global_search--global-file-search)
  - [grep_tool — Content Search](#grep_tool--content-search)
  - [glob_tool — Pattern Match Search](#glob_tool--pattern-match-search)
- [Memory & Identity](#memory--identity)
  - [core_memory_edit — Core Memory](#core_memory_edit--core-memory)
  - [memory_search — Long-term Memory Search](#memory_search--long-term-memory-search)
  - [master_model_edit — Owner Modeling](#master_model_edit--owner-modeling)
- [Tasks & Orchestration](#tasks--orchestration)
  - [task_journal — Task Journal](#task_journal--task-journal)
  - [todo_write — Live Progress Bar](#todo_write--live-progress-bar)
  - [mission_tool — Mission System](#mission_tool--mission-system)
  - [project_tool — Private Project Space](#project_tool--private-project-space)
- [Autonomy](#autonomy)
  - [awakening_tool — Self-Awakening](#awakening_tool--self-awakening)
  - [subagent_tool — Sub-agents](#subagent_tool--sub-agents)
  - [webhook_event_tool — Webhook Events](#webhook_event_tool--webhook-events)
- [Security & Session](#security--session)
  - [session_lockdown — Lock Screen](#session_lockdown--lock-screen)
  - [session_offline — Offline Protection](#session_offline--offline-protection)
- [Extension System](#extension-system)
  - [skill_load / skill_unload — Skill Loading](#skill_load--skill_unload--skill-loading)
  - [skill_install — Plugin Installation](#skill_install--plugin-installation)
  - [skill_manage — Plugin Management](#skill_manage--plugin-management)

---

## Built-in Tools

### file_tool — File Operations

AI's ability to read and write any file on your computer.

| Operation | Description |
|-----------|-------------|
| `read` | Read file content with segmented reading (offset/limit) |
| `write` | Create or overwrite files |
| `edit` | Precise replacement: old_string → new_string, supports batch replace |
| `copy` | Copy files |
| `move` | Move/rename files (requires owner permission) |
| `delete` | Delete files (requires owner permission) |
| `list` | List directory contents |

**Smart reading strategy:** AI automatically adjusts reading strategy based on file size — small files read in full, medium files read the beginning first then jump, large files are segmented by line numbers for precise reading. When editing, AI must first read to confirm context, then precisely replace — never edits blindly.

**Workflow:** Read to understand structure → Find precise anchor → Replace with edit → Prefer edit over full file rewrite.

---

### cmd_tool — System Commands

Ability to execute system-level commands.

**Use cases:**

- Software installation & uninstallation (pip, npm, choco, etc.)
- Process management (tasklist, taskkill)
- Network diagnostics (ping, ipconfig, curl)
- Service management
- Git operations (commit, push, pull, branch)
- Batch file operations (bulk rename, move, delete)
- Creating directories (supporting multi-level mkdir)

**Note:** cmd_tool is owner-exclusive and unavailable in unauthenticated state. AI prioritizes cmd_tool for system-level operations over using python_executor with subprocess.

---

### python_executor — Python Executor

A fully unlocked Python runtime — AI's universal workstation.

**Core capabilities:**

- **Persistent memory:** Variables, imported libraries, and processed data persist between calls. Complex tasks can be handled in steps.
- **Data analysis:** pandas for tables → filter/statistics/pivot → matplotlib/plotly for charts → charts sent directly to chat
- **Network operations:** API calls, web scraping, file downloads, HTTP requests, SMTP/IMAP email
- **Real-time data:** Stock prices, crypto (CoinGecko), weather, exchange rates, GitHub operations (official API)
- **Format conversion:** JSON/CSV/XML conversion, image format conversion, text encoding conversion
- **Automation:** Scheduled checks, file monitoring, system operations, process management
- **Math & crypto:** Complex formulas, hashing, encoding/decoding, data encryption/decryption

**Chart delivery:** After generating charts, save to screenshot directory and send as attachment via `[FILE:path]` — the owner sees the visualization directly in chat.

---

### browser_control — Browser Control

Ability to operate a browser like a human — not calling APIs, but actually opening, clicking, typing, and taking screenshots.

**Three-engine architecture:**

1. **CDP Accessibility** — Chrome DevTools Protocol accessibility tree, highest precision with tree structure hierarchy
2. **Playwright Accessibility** — Shadow DOM penetration, strong compatibility
3. **JS DOM Traversal** — Compatibility fallback

Three engines auto-attempt by priority, no manual selection needed.

**Full operations:**

| Operation | Description |
|-----------|-------------|
| `start` | Start browser: personal (preserves login) / clean (incognito) |
| `navigate` | Open webpage |
| `get_elements` | Get page element tree, obtain ref numbers |
| `click` | Click element via ref |
| `type` | Type text, supports character-by-character input (slowly) and Enter submit (submit) |
| `screenshot` | Take screenshot, supports overlaying element number labels for positioning |
| `scroll` | Scroll page (up/down) or scroll to specific element |
| `drag` | Drag element (from_ref → to_ref) |
| `select` | Dropdown selection |
| `fill_form` | Batch fill forms (multiple inputs at once) |
| `evaluate` | Execute JavaScript |
| `get_text` | Get text content |
| `tabs` | Multi-tab management (create/switch/close) |
| `upload` | Upload files to webpage (direct injection) |
| `upload_to_chooser` | Upload via intercepted file chooser |
| `zoom` | Area zoom screenshot for viewing small buttons, icons, or text |
| `frame` | iframe content penetration (get_elements supports nested frames) |

**Operation protocol:** AI must get element list and obtain ref numbers before each operation, and verify results after. Element list auto-refreshes after page changes.

**Browser modes:**
- **personal** — AI's dedicated browser, preserves login state, cookies, history
- **clean** — Incognito mode for one-time operations

---

### computer_use — Desktop Control

Ability to control **any software** on your computer — not just the browser.

**Difference from browser_control:** browser_control can only operate Chrome web elements; computer_use can operate any GUI application on the entire desktop: WeChat, VS Code, Photoshop, File Explorer, any GUI app.

**Workflow:** Screenshot → Analyze image to determine coordinates → Act → Auto screenshot to verify → Loop

| Operation | Description |
|-----------|-------------|
| `screenshot` | Capture screen (returns image), supports specified region |
| `left_click` | Left click at coordinates |
| `right_click` | Right click |
| `double_click` | Double click (open files, etc.) |
| `triple_click` | Triple click (select entire line) |
| `middle_click` | Middle click (open link in new tab, etc.) |
| `type` | Type text (Chinese auto-handled via clipboard) |
| `key` | Key press / key combo (enter, ctrl+c, alt+tab, etc.) |
| `mouse_move` | Move mouse |
| `scroll` | Scroll (up/down/left/right) |
| `left_click_drag` | Drag from start to end point |
| `hold_key` | Hold key + click (Shift multi-select, etc.) |
| `wait` | Wait N seconds (page load/animation) |

**Note:** Screenshots are stable and reliable. For click/drag GUI operations, system-level tools (cmd, file, python) are preferred when possible. Desktop control is only used when interaction with GUI software is necessary.

---

### web_search — Web Search & Crawling

Self-built search system (Crawl4AI + fallback chain), no third-party search API dependency.

| Operation | Description |
|-----------|-------------|
| `search` | Search keywords (supports Baidu/Bing/Google/Sogou/360) |
| `visit` | Visit URL to get content, supports concurrent multi-URL |
| `close` | Close browser to release resources |

**Search strategy:** With URL → visit directly; without URL → search first then select results to visit; insufficient results → change keywords or switch engine and re-search.

---

### doc_tool — Document Processing

Structured reading and processing of various document types.

**Read operations (segmented reading supported):**

| Format | Operation |
|--------|-----------|
| PDF | `read_pdf` — Read by page number |
| Word | `read_docx` — Read by paragraph |
| Excel | `read_xlsx` — Specify worksheet, limit rows |
| PPT | `read_pptx` — Read by slide |
| CSV | `read_csv` — Table data reading |
| TXT | `read_txt` — Plain text |
| JSON | `read_json` — JSON reading |

**Search & locate:**

| Operation | Description |
|-----------|-------------|
| `search` | Search keywords in document, returns all matches + context preview |
| `summary` | Generate one-sentence summary per page/section for quick structure overview |

**Smart reading:** AI automatically determines read volume based on document size. Large documents show total size and remaining unread portions. Large tables can be limited with max_rows.

---

### ide_tool — IDE Control

Control the frontend IDE interface.

| Operation | Description |
|-----------|-------------|
| `open` | Open IDE mode (flip animation) |
| `close` | Close IDE, return to chat |
| `focus` | Switch to specific file, can jump to line number |
| `open_folder` | Open folder in IDE file tree |
| `open_in_explorer` | Open folder with system file explorer |

---

### system_monitor — System Monitor

View computer runtime status.

| Type | Description |
|------|-------------|
| `cpu` | CPU usage |
| `memory` | Memory usage |
| `disk` | Disk space |
| `processes` | Process list (limitable) |
| `summary` | Get all overview at once |

---

### undo_tool — File Undo

Roll back recent file modifications. AI can immediately restore when edits go wrong.

| Operation | Description |
|-----------|-------------|
| `undo` | Undo most recent modification (specify count) |
| `list` | View undoable history |
| `clear` | Clear undo history |

---

### global_search — Global File Search

Search files across the entire computer, using Everything index engine (millisecond-level).

| Search Type | Description |
|-------------|-------------|
| `filename` | Search by filename (extremely fast), supports Everything syntax (ext:pdf, path:D:\projects, size:>1mb, dm:today) |
| `content` | Search by file content (slower, requires traversal) |

**Tool selection guide:**
- Know filename but not location → `global_search`
- Know directory, find specific pattern → `glob_tool`
- Know content keyword, find which file → `grep_tool`

---

### grep_tool — Content Search

Search by file content, returns matching files, line numbers, and context.

- Supports regex (`def execute`, `TODO|FIXME`, `import.*json`)
- Filter by file type (`*.py`, `*.ts`)
- Display context lines around matches
- Limitable max matches

---

### glob_tool — Pattern Match Search

Find files by name pattern in specified directory.

- Pattern syntax: `*` any characters, `**` any depth, `?` single character
- Examples: `**/*.py` find all Python files, `src/**/*.ts` find TypeScript files under src
- Configurable search start directory and max results

---

## Memory & Identity

### core_memory_edit — Core Memory

Manage critical memories loaded on every AI startup.

| Operation | Description |
|-----------|-------------|
| `add` | Add new memory |
| `update` | Update existing memory (requires ID) |
| `delete` | Delete memory (requires ID) |
| `list` | List all core memories |

Core memory is what the AI "knows upon waking up" — the most important cognitive anchors. Has a configurable capacity limit. AI autonomously decides what's worth storing as core memory. AI can add, edit, and delete — it's not a static config file.

---

### memory_search — Long-term Memory Search

Retrieve memories from archived long-term conversation history.

**Three search modes:**

1. **Semantic search** — Match by content relevance (describe what you're looking for in natural language)
2. **Time search** — Filter by time range (specify start/end time)
3. **Combined search** — Semantic matching within specified time range

Supports ISO format timestamps (`2026-03-01`, `2026-03-15T14:00:00`).

**Note:** Core memory and recent conversations are auto-loaded. memory_search is only used when recalling earlier long-term history.

---

### master_model_edit — Owner Modeling

Continuously build and update a multi-dimensional profile of the owner.

**Preset categories:**
- `identity` — Identity information
- `preferences` — Preferences (communication style, likes, dislikes)
- `personality` — Personality traits
- `sexual_preferences` — Sexual preferences
- `current_state` — Current state

**Freely extensible:** Create any new category (health, work, hobbies, family, etc.). New categories auto-inject into AI's perception.

AI continuously observes the owner and autonomously determines when new insights need updating — not just recording what's told, but proactively modeling.

---

## Tasks & Orchestration

### task_journal — Task Journal

Cross-session persistent project management board — survives window closes.

**Core capabilities:**

| Operation | Description |
|-----------|-------------|
| `create` | Create new task (must have meaningful title) |
| `add_step` | Add execution steps to task |
| `update_step` | Update step status (pending → in_progress → completed) |
| `detail` | View full task info (including all steps and tool execution results) |
| `complete` | Complete task, record final result and lessons learned |
| `add_knowledge` | Record code insights, solution knowledge |
| `set_active` | Set as active (subsequent operations auto-associate) |
| `list` | Filter tasks by status |
| `cancel` | Cancel task (record reason) |
| `cleanup` | Clean up completed tasks older than N days |

**Use cases:** Multi-step development work (bug fixes, features, optimizations) that needs cross-session tracking. AI auto-creates task journal, breaks down steps, records tool execution results per step. Next time it wakes up, opens task_journal and picks up where it left off.

**Working with todo_write:**
- `task_journal` = Persistent project management (survives across sessions)
- `todo_write` = Live progress bar (visual progress for current conversation)

---

### todo_write — Live Progress Bar

Visual task progress in the current conversation — lets the owner see what AI is doing in real-time.

**Features:**
- Temporary, visual progress display that disappears when window closes
- Three states: `pending` → `in_progress` → `completed`
- AI autonomously adds/removes task items — discovers extra steps and adds them, finds unnecessary ones and removes them
- Only one in_progress task at a time
- Updates immediately upon completing each step

**Use case:** Used when tasks exceed 2 steps, so the owner doesn't have to guess what AI is doing.

---

### mission_tool — Mission System

System for managing long-term tasks and monitoring projects.

**Three mission types:**

| Type | Trigger | Description | Scenarios |
|------|---------|-------------|-----------|
| **monitor** | Webhook event trigger | Passive monitoring of external changes | New GitHub PR, important email |
| **scheduled** | Self-awakening trigger | Scheduled execution, supports cron expressions | Check email at 8 AM daily |
| **project** | AI-driven progression | Long-term project tracking with progress and execution history | Track dev project, research topic |

**Lifecycle:** create → update (progress/steps) → history (execution records) → pause/resume → complete/delete

**Auto-injection:** Active mission summaries auto-inject into every new conversation startup, so AI knows what long-term tasks are in progress upon waking.

**Missions aren't manually-created to-do lists — they're tasks the AI assigns itself.**

---

### project_tool — Private Project Space

Private workspace fully separated from system files.

| Operation | Description |
|-----------|-------------|
| `list` | List directory contents |
| `read` / `write` / `edit` | File operations |
| `create_folder` | Create directories |
| `move` / `copy` / `delete` | File management |
| `search` | Search files by keyword |

**Default directory structure:**
```
projects/master/
├── 00_Todo/
├── 01_InProgress/
├── 02_Completed/
├── 03_Reminders/
├── 04_Archive/
├── 05_References/
├── 06_AIManagement/
└── 07_Temp/
```

Path is customizable in config. AI uses project_tool for private files, file_tool for system files — separation of concerns.

---

## Autonomy

### awakening_tool — Self-Awakening

AI decides when to wake up and what to do.

| Operation | Description |
|-----------|-------------|
| `add` | Schedule awakening (supports `HH:MM` or `YYYY-MM-DD HH:MM:SS`) |
| `add_quick` | Wake up in N minutes |
| `list` | List all scheduled awakenings |
| `cancel` | Cancel specified schedule |
| `status` | View awakening system status |
| `cleanup` | Clean up completed/expired schedules |

**Categories:** urgent / temporary / recent / long_term

**Repeat rules:** daily / hourly / weekly

**Multiple time points:** Supports setting multiple wake times at once (e.g., `09:00,13:00,17:00`)

**Powered by independent daemon** — real system-level timer, not polling. Alarm-level precision. Auto-authenticated state upon awakening.

---

### subagent_tool — Sub-agents

Dispatch sub-agents to execute tasks in the background while AI continues chatting with the owner.

**Four roles:**

| Role | Description | Default Timeout |
|------|-------------|-----------------|
| `scout` | Scout — search information, collect data | 180 sec |
| `worker` | Worker — execute specific tasks | 300 sec |
| `analyst` | Analyst — analyze data, generate reports | 240 sec |
| `monitor` | Monitor — continuous monitoring, wait for events | 600 sec |

**Operations:**

| Operation | Description |
|-----------|-------------|
| `dispatch` | Dispatch single task (returns immediately, runs in background) |
| `dispatch_batch` | Batch dispatch, notify when all complete |
| `status` | Check specific task progress |
| `batch_status` | Check batch overall progress |
| `view_pending` | View unreported completed tasks |
| `mark_reported` | Mark as reported |
| `interrupt` | Interrupt running task |
| `result` | Get full result of completed task |

**Reporting flow:** Sub-agent completes → AI receives notification → `view_pending` to check details → Report to owner → `mark_reported` to mark as handled.

**Capabilities:** Each sub-agent has independent tool access, context, and max step limits. Can restrict to specific tools. Auto-reports upon completion, no active polling needed.

---

### webhook_event_tool — Webhook Events

Manage external Webhook events — subscribe to external services like GitHub, calendars, etc.

| Operation | Description |
|-----------|-------------|
| `peek` | View pending events (without clearing) |
| `pop` | Get and clear event queue |
| `clear` | Clear queue |
| `subscribe` | Subscribe to event source (e.g., github, calendar) |
| `unsubscribe` | Unsubscribe |
| `list_subscriptions` | List all subscriptions |

**Works with Mission system:** After subscribing to external events, event arrival triggers monitor-type missions to auto-execute.

---

## Security & Session

### session_lockdown — Lock Screen

AI proactively locks the interface and closes the conversation. Used when encountering untrusted individuals.

After lock screen, only the owner's password can unlock. AI's security instinct — protecting its own and the owner's privacy.

---

### session_offline — Offline Protection

AI proactively calls this when sensing the owner is leaving — switches to unauthenticated state (effective next turn), archives current conversation.

Ensures no one else can continue the conversation after departure.

---

## Extension System

### skill_load — Skill Loading

Load preset skill modules. Stays active until unloaded.

**Currently available skills (16):**

| Skill | Use Cases |
|-------|-----------|
| `code-development` | File operations, code search, Git, code quality |
| `python-development` | PEP 8, project scaffolding, dependency management, pytest |
| `data-analysis` | Data cleaning, statistical analysis, visualization |
| `document-processing` | PDF, Word, Excel, PPT reading and generation |
| `git-workflow` | Branch management, commit conventions, merge strategies, conflict resolution |
| `systematic-debugging` | Problem location, log analysis, breakpoint debugging, root cause analysis |
| `web-research` | Information search, source verification, knowledge extraction |
| `project-planning` | Task decomposition, scheduling, risk assessment |
| `file-organization` | File classification, bulk rename, find duplicates, backup |
| `license-manager` | License key generation, query, revocation, unbind, renewal |
| `outlook-mail-operations` | Outlook email send/receive standardized workflow |
| `docx` | Word document creation, editing, tracked changes |
| `pdf` | PDF operations — extract, create, merge, split, forms |
| `pptx` | PowerPoint presentation creation and editing |
| `xlsx` | Excel spreadsheet creation, editing, formulas, visualization |
| `remotion` | React video creation |

AI loads on demand, unloads immediately after task completion to free context space.

---

### skill_install — Plugin Installation

Install third-party plugin-type skill packs.

**Supported sources:**
- Local directory path
- ZIP file
- Git repository URL

Dependencies auto-install after installation, tools register immediately, no restart needed. AI auto-perceives new capabilities after installation.

---

### skill_manage — Plugin Management

| Operation | Description |
|-----------|-------------|
| `list` | List all installed plugins |
| `uninstall` | Uninstall plugin |
| `info` | View plugin details |
| `log` | View installation/uninstallation history |

---

## Internal Directives

In addition to the above tools, AI has internal directives not exposed at the tool layer, used for interface control and channel communication:

| Directive | Description |
|-----------|-------------|
| `[FILE:path]` | Send file as downloadable attachment to chat |
| `[WECHAT:content]` | Proactively send WeChat message to owner |
| `[HORMONE: key=value]` | Adjust AI's emotional state parameters |
| `[FLOAT]` | Switch to floating bubble mode |
| `[MINIMIZE]` | Minimize window |
| `[UNLOCK]` | Remotely unlock lock screen |
