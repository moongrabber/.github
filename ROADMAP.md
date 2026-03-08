# WIRE — Project Roadmap

**Owner:** moongrabber
**Status:** Active development
**Last updated:** 2026-03-08

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────┐
│                   moongrabber                       │
│                   (proprietary)                     │
│                                                     │
│  gateway-spec  gateway  dashboard  hardware         │
└────────────────────────┬────────────────────────────┘
                         │
                    builds on
                         │
┌────────────────────────┴────────────────────────────┐
│                    openyodel                        │
│               (open source, MIT)                    │
│                                                     │
│            spec (✓)    swift    js                   │
└─────────────────────────────────────────────────────┘
```

---

## Phase 0 — Foundation (current)

**Goal:** Protocol exists, spec is implementable, first release done.

| Task | Repo | Status |
|------|------|--------|
| Yodel Protocol Spec v1 | openyodel/spec | ✓ Done (v1.0-draft.1) |
| OpenAPI 3.1 for Yodel | openyodel/spec | ✓ Done |
| WIRE Gateway API Spec | moongrabber/wire-gateway-spec | In progress |

---

## Phase 1 — First Voice (next)

**Goal:** Speak into a device, get a response from an AI backend. Prove the concept works.

**Key decision:** Build the client FIRST, directly against a backend (Ollama, Claude API). No gateway needed yet. Yodel is designed to work without a gateway — the client just sends a standard OpenAI-compatible request with optional Yodel headers. This gets us to a working demo in days, not weeks.

| Task | Repo | Details | Priority |
|------|------|---------|----------|
| iOS SDK (wire-swift) | openyodel/swift | SwiftUI, Apple SpeechAnalyzer for STT, Yodel request/response handling, SSE streaming parser | High |
| PWA SDK (wire-js) | openyodel/js | React/TypeScript, Web Speech API for STT, SSE streaming | High |
| iOS reference app | openyodel/swift | Minimal UI: pick a backend URL + model, press mic, speak, see streaming response. Direct connection, no gateway. | High |
| PWA reference app | openyodel/js | Same as iOS but in browser. Fastest path to a shareable demo. | High |
| Test against Ollama | — | Local Ollama instance, llama3, direct Yodel request. First end-to-end test. | High |
| Test against Claude API | — | Direct Yodel request to Claude API. Proves backend-agnosticism. | Medium |

**Definition of done:** A 30-second video of someone speaking into a phone/browser and getting a streamed response from a local LLM.

---

## Phase 2 — The Gateway

**Goal:** WIRE Gateway is running. Devices register, pull config, and talk to backends through the proxy. Backend API keys never touch the device.

| Task | Repo | Details | Priority |
|------|------|---------|----------|
| Gateway implementation | moongrabber/wire-gateway | Bun + SQLite (start simple). REST API per gateway-spec. Device registration, agent CRUD, config pull, Yodel proxy. | High |
| Device secret rotation | moongrabber/wire-gateway | `POST /v1/devices/{id}/rotate-secret` | Medium |
| Yodel proxy | moongrabber/wire-gateway | Forward Yodel requests to backends, inject API keys server-side. SSE passthrough. | High |
| Web dashboard (MVP) | moongrabber/wire-dashboard | Simple UI for managing devices and agents. | Medium |
| Update iOS/PWA clients | openyodel/swift + js | Add gateway support: register, pull config, route through proxy. Fallback to direct mode if no gateway. | High |
| Musical audio pairing (PoC) | moongrabber/wire-gateway + clients | ggwave integration. Dashboard plays music clip with encoded session ID, device decodes and registers. Double confirmation. | Medium |
| QR code pairing (fallback) | moongrabber/wire-gateway + clients | Standard QR pairing as fallback for devices with cameras. | Low |

**Definition of done:** A device registers with the gateway, pulls its agent config, and speaks to an LLM through the Yodel proxy — without knowing the backend URL or API key.

---

## Phase 3 — Integrations & Channels

**Goal:** Yodel becomes a channel for agent platforms. Smart home integration. Car integration.

| Task | Repo | Details | Priority |
|------|------|---------|----------|
| OpenClaw integration | — | OpenClaw registers as agent platform at the gateway. WIRE becomes a voice channel for OpenClaw agents. | High |
| CarPlay client | openyodel/swift | WIRE as CarPlay app. Talk to your agent in the car. | Medium |
| Android Auto client | openyodel/kotlin (new) | Android SDK + Android Auto integration. | Medium |
| Home Assistant integration | openyodel or moongrabber | Custom integration or add-on. Voice commands to local LLM agent via Yodel. | Medium |
| Node-RED Yodel node | openyodel | Yodel as trigger/action in Node-RED flows. | Low |
| MQTT bridge | openyodel | Yodel-to-MQTT gateway for IoT devices. | Low |

---

## Phase 4 — Yodel v2 & Device Mesh

**Goal:** Bidirectional streaming. Devices talk to each other. Proactive agent updates.

| Task | Repo | Details | Priority |
|------|------|---------|----------|
| Yodel v2 spec | openyodel/spec | WebSocket transport. Bidirectional streaming. Device mesh routing. Agent status push. | High |
| Device mesh routing | moongrabber/wire-gateway | Route audio/responses between devices. Speak into phone, hear answer on speaker. | High |
| Cross-device handoff | moongrabber/wire-gateway | Seamlessly transfer conversation from one device to another. | Medium |
| Proactive agent messages | openyodel/spec + gateway | Backend pushes updates to client without request. | Medium |

---

## Phase 5 — Hardware & Scale

**Goal:** Dedicated WIRE device. Commercial launch.

| Task | Repo | Details | Priority |
|------|------|---------|----------|
| Hardware prototype | moongrabber/wire-hardware | Microphone, speaker, WiFi, one button. Chip platform TBD (market moving fast). | Medium |
| Gateway scaling | moongrabber/wire-gateway | PostgreSQL migration, multi-region, rate limiting, billing integration. | High |
| Freemium launch | moongrabber | Free tier (1 device, 1 agent), Pro tier (~5–10€/mo), Lifetime (~99–149€). | High |
| OVOS / open smart speaker | openyodel | Yodel as voice backend for Open Voice OS. | Low |
| Industrial terminals | moongrabber | B2B: voice agents for warehouse, healthcare, field service. | Low |

---

## Principles

- **Client first, gateway second.** Always have a working demo before adding infrastructure.
- **Yodel works without WIRE.** The open protocol must always function standalone. WIRE adds convenience, not dependency.
- **Native clients, shared protocol.** No cross-platform frameworks. Each platform gets a native SDK.
- **Ship small, ship often.** Every phase has a concrete "definition of done" — a thing you can show.
- **Hardware chip decisions are deferred.** The edge-AI market is moving too fast to commit now.

---

## Repository Map

| Repo | Org | Visibility | Purpose |
|------|-----|------------|---------|
| spec | openyodel | Public | Yodel protocol specification |
| swift | openyodel | Public | iOS/macOS SDK |
| js | openyodel | Public | Web/PWA SDK |
| kotlin | openyodel | Public | Android SDK (Phase 3) |
| wire-concept | moongrabber | Private | Product concept and architecture |
| wire-gateway-spec | moongrabber | Private | WIRE Gateway API specification |
| wire-gateway | moongrabber | Private | WIRE Gateway implementation (Phase 2) |
| wire-dashboard | moongrabber | Private | WIRE web dashboard (Phase 2) |
| wire-hardware | moongrabber | Private | Hardware device design (Phase 5) |
