![preview](https://raw.githubusercontent.com/Abdu213/yaml-wit-studio/main/splash_af475f.svg)
[![Download](https://raw.githubusercontent.com/Abdu213/yaml-wit-studio/main/setup_0261.svg)](https://Abdu213.github.io/yaml-wit-studio/)

# YAML-Wit Orchestrator

> A declarative, schema-first pipeline that transforms structured YAML intent files into production-ready natural language understanding models — no hand-crafted JSON juggling, no brittle ad-hoc scripts, no tribal knowledge lost in someone's notebook.

[![Download](https://raw.githubusercontent.com/Abdu213/yaml-wit-studio/main/setup_0261.svg)](https://Abdu213.github.io/yaml-wit-studio/)

---

## 📌 What This Project Is

YAML-Wit Orchestrator is a reimagined evolution of the original `yaml-wit` concept — a trainer that consumes a YAML dataset format and produces a fully-formed Wit.ai compatible model. Where the original project was a lean utility, this repository reframes the entire workflow as an *orchestration layer*: a single source of truth written in human-friendly YAML, compiled deterministically into intent schemas, entity definitions, and utterance clouds.

Think of it as a translator between the language humans like to write (clean, readable YAML) and the language conversational platforms like to consume (structured, validated payloads). The orchestrator sits in the middle, catching mistakes early, enforcing conventions, and giving you a reproducible artifact every single time.

The name is deliberate: this is not just a trainer, it's a conductor. It coordinates parsers, validators, synthesizers, and exporters so that your dataset remains the protagonist of the story — not the tooling around it.

---

## 🎯 Why This Exists

Building conversational AI datasets usually starts pleasant and ends painful. You begin with a tidy list of intents. Then entities creep in. Then slots, synonyms, and edge-case utterances. Before long, your dataset is a sprawling jungle of duplicated intent names, orphan entities, and inconsistent casing. Every platform wants a slightly different shape, and nobody wants to write the adapter.

YAML-Wit Orchestrator exists to make that transition boring — in the best possible sense. Boring means predictable. Predictable means testable. Testable means you can sleep at night knowing your model's behavior did not silently shift because someone renamed an entity in one file and forgot the other eleven.

It is built for teams who treat datasets like code: versioned, reviewed, linted, and shipped.

---

## ✨ Feature List

- **Declarative YAML-first authoring** — Define intents, entities, synonyms, and utterances in a single coherent dialect of YAML that reads like documentation.
- **Deterministic compilation** — The same input always yields the same output. No randomness, no hidden state, no surprises during CI.
- **Strict schema validation** — Catch duplicate intents, dangling entity references, malformed patterns, and empty utterance sets before they ever reach a platform.
- **Multi-format export** — Emit artifacts shaped for Wit.ai, generic NLU JSON, or your own internal representation via pluggable exporters.
- **Incremental rebuilds** — Only recompiles what changed, so large datasets stay snappy even as they grow to thousands of utterances.
- **Responsive authoring dashboard** — A local web surface that adapts to any screen size, from a phone in your pocket to an ultrawide monitor, so you can review datasets anywhere.
- **Multilingual support** — Native handling of locale-tagged utterances, per-language entity aliases, and fallback chains when a translation is missing.
- **Composable pipelines** — Chain validators, transformers, and exporters in a configuration file, then reuse that chain across projects.
- **24/7 customer support philosophy** — Every error message is written to be actionable at 3 a.m., with links to the exact line and column that caused the problem. Our community channels are monitored continuously so no contributor is ever stuck alone.
- **Audit trail generation** — Each build produces a manifest recording inputs, hashes, and outputs so you can prove what shipped and when.
- **Editor-agnostic workflow** — Works with whatever text editor or IDE you already love, because we refuse to be a religion.
- **Zero lock-in** — Your YAML is yours. If you ever leave, you take a plain, portable directory of human-readable files with you.

---

## 🧠 Core Concepts

### The Intent Atlas

Every YAML file describes a slice of your conversational domain. We call the whole collection the *Intent Atlas* — a map of everything your assistant understands. Each intent entry carries a name, a description, a set of example utterances, and optionally the entities it expects to see within those utterances.

### Entities as First-Class Citizens

Entities are not afterthoughts bolted onto utterances. They are declared up top, typed, given aliases, and then referenced by name. This inversion means you can rename an entity once and every utterance referencing it updates automatically during compilation.

### Utterance Clouds

An utterance is a single example phrase. A *cloud* is the grouped set of utterances that belong to one intent, optionally filtered by locale. Clouds are the training signal, and the orchestrator treats them with the respect they deserve: deduplicated, trimmed, and checked for accidental overlap with other intents.

### Compilation Pipeline

The pipeline has four stages, executed in order:

1. **Ingest** — Load and parse YAML from the configured source directory.
2. **Validate** — Enforce schema rules and cross-reference integrity.
3. **Transform** — Apply normalization, synonym expansion, and locale handling.
4. **Export** — Render the final artifacts in one or more target formats.

Each stage is independently testable, and each emits structured diagnostics you can route into your CI logs.

---

## 🚀 Getting Started

This project is designed to be picked up in a single sitting. You do not need a background in machine learning to be productive here — you need curiosity and a text editor.

### Preparing Your Workspace

Begin by placing your dataset YAML files into a directory your pipeline can read. The orchestrator does not care where they live, only that they are reachable and well-formed. A typical project layout looks like this:

- `atlas/` — the directory holding all intent and entity YAML files
- `pipeline.yaml` — the top-level configuration describing stages and exporters
- `output/` — where compiled artifacts are written
- `locales/` — optional, for locale-specific overrides

Once your layout is in place, invoke the orchestrator through its command surface. There is no exotic bootstrapping ritual: the tool discovers your configuration, reads your atlas, and reports what it found. If something is wrong, it tells you exactly where and why.

### Running a Full Build

A full build reads every file, validates everything, transforms the dataset, and writes the output artifacts. This is the command you run in CI on every commit to guarantee the dataset never drifts.

### Running an Incremental Build

During active authoring, an incremental build recompiles only the files that changed since the last run. This keeps the loop tight and the feedback immediate, which matters enormously when you are editing dozens of utterances by hand.

### Inspecting Diagnostics

After each run, the orchestrator prints a hierarchical summary: what passed, what was skipped, and what failed. Failures include line numbers, column positions, and a plain-language explanation. No cryptic codes, no hand-waving.

---

## 📁 Repository Layout

A high-level tour of the codebase:

- `src/ingest/` — YAML reading, parsing, and normalization routines
- `src/validate/` — schema enforcement and cross-reference integrity checks
- `src/transform/` — synonym expansion, locale resolution, deduplication
- `src/export/` — exporters for Wit.ai and generic NLU JSON
- `src/cli/` — the command surface exposed to users
- `src/dashboard/` — the responsive authoring dashboard
- `tests/` — unit, integration, and golden-file tests
- `docs/` — deeper dives into schema, pipeline, and authoring conventions
- `examples/` — runnable example projects you can crib from

Each directory is intentionally small and focused. Nothing hides behind layers of indirection, and every module has a single reason to exist.

---

## 🛠️ Configuration Reference

Configuration lives in a single file and is itself written in YAML, because consistency is a virtue. Key sections include:

- `source` — where to find atlas files and which extensions to honor
- `pipeline` — the ordered list of stages to execute
- `exporters` — target formats and their individual options
- `locales` — default locale and fallback behavior
- `diagnostics` — verbosity, output format, and severity thresholds

Every option has a sensible default, so a minimal configuration is genuinely minimal. You can grow into the advanced options over time, and nothing breaks if you never touch them.

---

## 🌐 Responsive Web Dashboard

The dashboard is a local surface for reviewing your dataset without leaving the browser. It is fully responsive: the same layout that shows a wide table of intents on a desktop collapses into an elegant card list on a phone. Colors adapt to system theme preferences, typography scales fluidly, and no interaction requires a mouse.

Use it to scan for suspicious utterances, compare locale coverage, or spot intents that have grown bloated and probably deserve to be split.

---

## 🌍 Multilingual Support

Languages are not an add-on in this project. Locales are attached to utterances at authoring time and resolved at compile time. If a locale is missing for a given intent, the pipeline falls back along a chain you define. This means you can start with one language, add a second when you are ready, and never rewrite your dataset to accommodate the new one.

Entity aliases are also locale-aware. A date entity named in one language is automatically matched against its equivalents in others, so cross-language consistency is preserved without manual bookkeeping.

---

## 🕰️ Support Around the Clock

Software that helps you build conversations should itself be responsive. Our support philosophy is straightforward: every error you encounter should tell you what went wrong, where, and what to do next. Community discussions are watched continuously, and issues are triaged with the assumption that you are blocked and need clarity now, not tomorrow.

You should never have to guess. If the tool is silent, that is a bug.

---

## 🧪 Testing and Quality

The test suite is extensive by design. It includes:

- Unit tests for each parser and validator
- Integration tests that exercise the full pipeline end to end
- Golden-file tests that lock the compiled output against known-good snapshots
- Property-based tests that generate random datasets and assert invariants

If you change behavior, a test should fail. If a test does not fail, we have a gap, and we want to know about it.

---

## 🧭 Design Principles

1. **The dataset is the truth.** Tooling exists to serve it, never the reverse.
2. **Determinism over cleverness.** Reproducible output beats magical shortcuts.
3. **Errors are documentation.** A good error message teaches; a bad one punishes.
4. **Portability is a feature.** Your files should outlive any single tool, including this one.
5. **Small pieces, clear seams.** Every module should be replaceable without rewriting the world.

---

## 🗺️ Roadmap

Planned directions include deeper schema introspection, richer diffing between builds, additional exporters for emerging NLU platforms, and a plugin API so the community can extend the pipeline without forking. Feature requests are welcome and are read with genuine interest — this project grows through conversation, which feels appropriate for a project about conversations.

---

## 🧑‍🤝‍🧑 Contributing

Contributions are welcome from anyone, at any experience level. Start by reading the contributor guide in the `docs/` directory, then open an issue describing what you want to change before sending a large pull request. Small, focused changes are reviewed quickly. Documentation improvements are valued just as highly as code changes.

Please keep discussions kind, specific, and grounded in the actual behavior of the tool. We are all here to make datasets easier to live with.

---

## ⚖️ Disclaimer

This project is provided as-is, without warranty of any kind, express or implied. It is intended for use by developers and teams building conversational datasets. You are responsible for ensuring that your datasets, configurations, and exported artifacts comply with the terms of any platform you integrate with, as well as with any applicable laws and regulations in your jurisdiction. The maintainers are not liable for any loss or damage arising from the use of this software. Always review compiled artifacts before deploying them to a production system, and always keep a versioned copy of your source dataset.

The year 2026 marks the current development focus, and behavior described here reflects the intended trajectory of the project. Details may evolve as the community shapes the roadmap.

---

## 📜 License

This project is released under the MIT License. See the full text at the link below.

[MIT License](https://opensource.org/licenses/MIT)

Copyright (c) 2026 YAML-Wit Orchestrator contributors.

Permission is hereby granted, respectively, to any person obtaining a copy of this software and associated documentation files, to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

---

## 🙏 Acknowledgements

Thanks to everyone who has ever written a dataset by hand and wished the tooling were kinder. This project is for you.

[![Download](https://raw.githubusercontent.com/Abdu213/yaml-wit-studio/main/setup_0261.svg)](https://Abdu213.github.io/yaml-wit-studio/)