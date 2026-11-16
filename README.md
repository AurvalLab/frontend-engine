# Frontend Engine

**English** · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · [한국어](README.ko.md)

An adaptive frontend creation skill for Codex, connecting visual direction, implementation, and verification within a single workflow. Frontend Engine scales its process to the scope of a task, from a focused interface adjustment to a complete site or a complex product flow.

Web is the default target. SwiftUI is supported when the request explicitly calls for an Apple-native interface or the existing project uses SwiftUI.

## Overview

Frontend Engine provides instructions for an AI coding agent. It is not a frontend framework, a component library, or an application generator that runs independently of Codex.

Its primary goals are to:

- Establish a usable visual target before implementation when design exploration is needed.
- Preserve the existing project's framework, components, design tokens, data contracts, and file structure.
- Deliver the smallest complete implementation that satisfies the request.
- Apply verification appropriate to the actual changes and their risks.
- Report what was implemented, what was checked, and what remains unverified.

Typical uses include landing pages, product interfaces, responsive components, redesigns, and explicitly requested SwiftUI screens. Isolated logic fixes and backend-only work fall outside the skill's intended scope.

## Requirements and optional capabilities

| Capability | When it is needed |
| --- | --- |
| Codex with local skill support | Required to load and use the skill instructions. |
| Git | Required for the clone and update commands below. |
| Existing project toolchain | Required for builds, type checks, and tests applicable to that project. The skill does not impose a new framework. |
| Product Design and image generation | Optional capabilities used for visual exploration or implementation from a selected visual target. |
| A browser automation or inspection tool | Needed for actual Web visual and interaction verification. |
| macOS with suitable Xcode tools | Needed for SwiftUI compilation, previews, and simulator checks when those checks are requested or applicable. |

These capabilities are supplied by the host environment. Installing this repository does not install plugins, image models, browser tools, animation libraries, or Xcode.

## Installation

Clone the repository into the Codex skills directory. The default location is `~/.codex/skills/frontend-engine`; when `CODEX_HOME` is configured, use its `skills/frontend-engine` subdirectory.

### macOS and Linux

```bash
skill_root="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$skill_root"
git clone https://github.com/AurvalLab/frontend-engine.git "$skill_root/frontend-engine"
```

### Windows PowerShell

```powershell
$codexRoot = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $env:USERPROFILE '.codex' }
$skillRoot = Join-Path $codexRoot 'skills'
New-Item -ItemType Directory -Force -Path $skillRoot | Out-Null
git clone https://github.com/AurvalLab/frontend-engine.git (Join-Path $skillRoot 'frontend-engine')
```

Confirm that `SKILL.md` is directly inside the installed `frontend-engine` directory. If the destination already exists and is not a Git checkout, back it up and synchronize the skill files manually rather than cloning over it.

## Usage

Invoke the skill with `$frontend-engine` and describe the intended result. Provide the project location, target page or component, references, required behavior, and acceptance criteria when available. State whether the agent should design only or complete the implementation.

### Explore a new visual direction

```text
Use $frontend-engine to create a product landing page for a reading app.
The primary action is joining the waitlist. Show three visual directions
before implementation, and wait for me to choose one.
```

### Implement an existing reference

```text
Use $frontend-engine to implement this screenshot in the existing React project.
Reuse the current components and theme. Make the layout responsive and connect
the primary action to the existing registration flow.
```

### Make a focused change

```text
Use $frontend-engine to fix the mobile layout of the pricing section.
Preserve its content and desktop layout. Check the affected breakpoints
and keyboard interaction.
```

### Request a single stage or a native interface

```text
Use $frontend-engine for Stage 1 only. Produce a blueprint; do not implement it.
```

```text
Use $frontend-engine to build a SwiftUI settings screen in this existing project.
Preserve its deployment target and state model. Support Dynamic Type and Reduce Motion.
```

## Adaptive workflow

The skill selects an execution path based on scope and risk. The seven stages remain available as named responsibilities; they are not seven mandatory approval gates for every task.

| Task | Execution path | Documentation |
| --- | --- | --- |
| Focused adjustment | Inspect the existing interface and affected components → change → verify affected layouts, states, and flows. | Usually a concise delivery summary or an update to existing project notes. |
| Single page or component | Brief design → implementation and visual refinement → verification. Expand motion work only when needed. | Short design decisions; save a blueprint when future handoff benefits from it. |
| Complete site, complex flow, or broad redesign | Expand the relevant design, token, implementation, refinement, motion, and QA responsibilities in dependency order. | Persist necessary design decisions and verification evidence, reusing project documentation. |

Simple work can combine design with token decisions, implementation with visual refinement, and basic transitions with component development. Complex motion requires explicit structural and lifecycle agreements. Static work does not need empty motion specifications or animation modules.

A small component can still need broader checks when it affects shared layout, complex state, security, or important data writes. Simplifying the process does not remove those responsibilities.

## Seven-stage reference

| Stage | Responsibility | Typical result |
| --- | --- | --- |
| 1 — Blueprint | Define page structure, content, visual direction, core actions, and necessary states. | Design decisions or a saved blueprint. |
| 2 — Tokens | Reuse or define the colors, type scale, spacing, and geometry needed by the task. | Updates to the existing theme or token definitions. |
| 3 — Motion Spec | Describe complex motion triggers, targets, structural dependencies, cleanup, and fallbacks. | Motion agreements, optionally saved as a separate specification. |
| 4 — Skeleton | Build responsive structure, core behavior, necessary states, and accessibility semantics. | A running page, component, or native view. |
| 5 — Taste Polish | Refine hierarchy, typography, spacing, assets, and interaction states against the selected direction. | A visually refined interface; dependency records for complex motion when needed. |
| 6 — Motion | Implement the agreed motion with lifecycle cleanup and reduced-motion support. | Working motion and appropriate fallbacks. |
| 7 — QA | Inspect real output, exercise key flows, fix defects, and record evidence. | Verified changes with explicit remaining limitations. |

Single-stage requests stay within the requested scope. Required upstream decisions are checked against the current project; missing critical decisions are resolved rather than silently assumed complete.

## Visual exploration and selection

When a new product lacks a clear visual target, or the user requests alternatives, the skill prefers the available `product-design:ideate` workflow. By default, it generates three independent images and waits for the user's selection before implementation begins. The selected image and subsequent feedback become the implementation target.

Existing screenshots, selected mockups, Figma references, design systems, and existing interface code can provide a target directly. Focused changes do not automatically trigger a new round of style exploration. A user can explicitly request skipping visual selection; a request merely to work quickly does not itself waive an active selection workflow.

If Product Design is unavailable, a concise written design can precede implementation. Explicit image requests and missing named references must be resolved rather than represented as completed. SwiftUI may reuse applicable visual exploration, but native implementation stays within the SwiftUI branch.

## Implementation principles

- Reuse project components and patterns before introducing new abstractions or dependencies.
- Preserve existing interfaces, validation, error handling, data behavior, and accessibility.
- Complete core actions and necessary loading, empty, error, and success states; do not present placeholders as working integrations.
- Resolve structural defects at their source, update affected agreements, and recheck dependent behavior.
- Keep animation optional for access to essential content. Handle reduced motion, initialization failure, cancellation, unmounting, and re-entry where relevant.
- Keep SwiftUI state ownership, view identity, deployment targets, and native interaction conventions intact.

## Verification and delivery

For ordinary Web work, verification normally includes a narrow and a desktop viewport, plus both sides of affected breakpoints. Complete sites and complex responsive work expand coverage to the reference matrix of 320, 375, 768, 1440, and 2560 CSS pixels, adjusted to the project's supported range.

Checks cover relevant visual differences, overflow, long content, interaction states, keyboard access, forms, runtime errors, and motion fallbacks. Project build, type, and behavior checks are used where appropriate. Performance claims require measurements; smooth appearance alone does not establish a frame-rate result.

SwiftUI checks use the actual supported platforms, window sizes, text scaling, appearances, and available native tools. Browser emulation and simulators do not establish physical-device behavior. When the required tools are unavailable, the corresponding checks are reported as unverified.

A delivery should identify:

- What changed and how to run or preview it.
- Which routes, components, states, sizes, and environments were checked.
- Relevant results and defects fixed.
- Remaining failures, unverified items, and important limitations.

Small tasks can use a concise response. Complex tasks or work requiring handoff should persist the necessary records in existing project documentation or `.agent-context/`. A blueprint, motion specification, or audit report is saved only when its content is useful; file names are mapped to the project's structure.

Successful skill validation or a local build does not establish visual acceptance, deployment success, production readiness, or real-device compatibility.

## Repository structure

| File | Purpose |
| --- | --- |
| [SKILL.md](SKILL.md) | Main instructions, scope routing, design entry points, and delivery rules. |
| [agents/openai.yaml](agents/openai.yaml) | Skill display metadata and an example invocation prompt. |
| [references/pipeline.md](references/pipeline.md) | Detailed Web stage responsibilities and verification guidance. |
| [references/quality.md](references/quality.md) | Shared engineering checks for APIs, state, resources, accessibility, and feedback. |
| [references/swiftui.md](references/swiftui.md) | Native platform mapping, implementation rules, and verification boundaries. |
| [references/third-party-notices.md](references/third-party-notices.md) | Source attribution, adaptation scope, and third-party license notices. |
| `README.md`, `README.zh-CN.md`, `README.ja.md`, `README.ko.md` | English, Simplified Chinese, Japanese, and Korean documentation. |

## Updating

For an installation cloned from this repository, run the following inside the skill directory:

```bash
git pull --ff-only
```

Review and preserve local modifications before updating. A manually copied installation must be synchronized manually; it is not automatically converted into a Git checkout.

## Documentation languages

English is the default README. Use the links at the top to switch to Simplified Chinese, Japanese, or Korean. These translations describe the same workflow; `SKILL.md` and the operational references currently remain in Simplified Chinese. Specify your preferred response language in the task when needed.

## Sources and licensing

The seven-stage framework was adapted from the user-provided `FrontendEngine.docx`. Selected engineering guidance was adapted from [Ponytail](https://github.com/DietrichGebert/ponytail) and [SwiftUI Agent Skill](https://github.com/twostraws/SwiftUI-Agent-Skill).

See the [source record](references/pipeline.md#来源说明) and [third-party notices](references/third-party-notices.md) for the recorded versions, adaptation boundaries, and applicable license texts.

The source document did not specify a license. The third-party MIT notices apply to their respective adapted portions; this repository does not claim a unified MIT license or grant blanket redistribution rights for all material.
