# WIRE — Project Roadmap

**Owner:** moongrabber
**Status:** Active development
**Last updated:** 2026-03-14

---

## Phase 1 — Gateway Foundation (current)

- ✅ Produktkonzept und Architektur-Dokument (v2.4)
- ✅ WIRE Gateway API Spec v1 (Draft)
- ✅ OpenAPI 3.1 Definition
- ✅ Optionale Bindings — soft-default ([#2](https://github.com/moongrabber/wire-gateway-spec/issues/2))
- ✅ Device `enabled`-Feld für Parking/Deaktivierung ([#5](https://github.com/moongrabber/wire-gateway-spec/issues/5))
- ✅ Gateway-Implementierung (MVP) — Bun + Hono + SQLite, alle Spec-Endpoints
- ✅ Well-Known Endpoint — Gateway Discovery (`/.well-known/yodel.json`)
- ✅ Web-Dashboard für Device- und Agent-Management (MVP) — React + Vite
- ✅ `wire-walkie-web` — Browser/PWA mit Speech Recognition + TTS
- 🔲 Slug-Rename Strategie — deferred to v2 ([#1](https://github.com/moongrabber/wire-gateway-spec/issues/1))
- 🔲 Audio-Pairing Prototyp (KI-generierte Musik, doppelte Bestätigung)

---

## Phase 2 — Product

- 🔲 Account-System (Auth, Billing-Vorbereitung)
- 🔲 Freemium-Modell: Free / Pro / Lifetime
- 🔲 Pairing-Gateway (proprietärer Service)
- 🔲 WIRE MCP Server — Gateway-Discovery und -Verbindung als MCP-Tools
- 🔲 WIRE Agent Skill — Yodel-Verbindung als Agent Skill ([agentskills.io](https://agentskills.io)-Standard)
- 🔲 **WIRE Walkie Clients** — Weitere proprietäre Clients:
  - `wire-walkie-macos` — macOS nativ (Swift)
  - `wire-walkie-ios` — iOS (Swift)
  - `wire-walkie-android` — Android (Kotlin)

---

## Phase 3 — Scale

- 🔲 Multi-User / Team-Accounts
- 🔲 Rate Limiting und Usage Tracking
- 🔲 WIRE v2 Gateway (WebSocket-Support, passend zu Yodel v2)

---

WIRE baut auf dem offenen [Yodel-Protokoll](https://github.com/openyodel) auf. Für die Protokoll-Roadmap siehe [openyodel/.github/ROADMAP.md](https://github.com/openyodel/.github/blob/main/ROADMAP.md).
