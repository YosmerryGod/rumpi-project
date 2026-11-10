# RUMPY

RUMPY is a social and communication platform designed to help people discover public content, meet others, and communicate while keeping control over their privacy.

- **Tagline:** Where conversations meet.
- **Primary color:** Merah Nusantara — `#D92738`.
- **Logo:** A minimal white speech bubble with three dots on a Merah Nusantara background.

> **Project status:** This README documents the current product direction and planned architecture. It is not a claim that these features have already been implemented.


## Application platform

**Product decision: RUMPY will be developed as a mobile application, not as a web app or PWA.**

- The primary user-facing product is an installable mobile app.
- Do not build the main RUMPY client as a website, Next.js web app, or PWA.
- Backend services and APIs remain separate from the mobile application.
- The mobile app communicates with RUMPY backend services for authentication, profiles, social content, native messaging, and connector management.
- The choice of mobile development framework and the initial target platforms (Android, iOS, or both) are still undecided. Do not assume a framework until the user confirms it.

## Product vision

RUMPY will initially use a WhatsApp connector to help acquire users and provide a familiar communication bridge. Over time, RUMPY will grow its own native user network and messaging infrastructure. The long-term goal is for RUMPY to work independently of WhatsApp.

The migration must be gradual:
1. Start with RUMPY accounts, social features, and an optional WhatsApp connection.
2. Build native RUMPY messaging between RUMPY users alongside the WhatsApp connector.
3. Grow the native network until WhatsApp is optional rather than foundational.

RUMPY's own accounts, content, contacts, and native conversations must continue to work if a user disconnects WhatsApp.

## Core product principles

### Public content, controlled communication

- Content published on RUMPY is discoverable by the public RUMPY audience, including people who are not contacts.
- Users can discover public profiles and content without exchanging phone numbers or becoming contacts.
- Social relationships and messaging permissions are separate.
- Following someone does **not** automatically make that person a contact or grant permission to send a private message.
- Private messaging is controlled by the recipient's settings.

Planned private-message access settings:
- **Contacts Only:** only contacts can start a private conversation.
- **Open to Chat:** any RUMPY user can start a conversation.
- **Request to Chat:** non-contacts can send a chat request; the recipient must approve it before a normal conversation begins.

The exact rules for public-post comments and other interactions remain to be specified separately. Do not assume that permission to view public content automatically grants permission to send private messages.

### RUMPY social features

Planned native RUMPY content includes:
- Public feed and user profiles.
- Stories.
- Short vertical videos.
- Photo and text posts.
- Likes and comments, subject to interaction and privacy rules.

RUMPY social content belongs to RUMPY. It is not automatically visible to ordinary WhatsApp users and must not depend on WhatsApp storage or delivery.

## Accounts and identity

Phone-number registration is part of RUMPY from the beginning, not something postponed until WhatsApp is removed.

- Users register with a phone number and verify ownership using an OTP flow.
- Every account receives a permanent internal `user_id`.
- A public `@username` is the main public identity.
- A WhatsApp account is an optional linked identity/connector, not the RUMPY account itself.
- A user must retain their RUMPY identity and native data when they unlink or stop using WhatsApp.

Suggested account fields:
- `user_id`: permanent internal RUMPY identifier.
- `phone_number`: verified phone number.
- `username`: unique public handle.
- `display_name`: display name.
- `avatar`: profile image.
- `bio`: profile description.
- WhatsApp link information stored separately from core account identity.

Never use a WhatsApp JID or WhatsApp phone identity as the primary RUMPY user ID.

## Phone-number privacy

Users control phone-number visibility, similar to the privacy model offered by messaging apps such as Telegram.

Planned setting: **Who can see my phone number?**
- **Everyone**
- **My Contacts**
- **Nobody**

A separate setting controls **Who can find me by my phone number?**
- **Everyone**
- **My Contacts**

These are independent controls. Hiding a phone number from a profile must not automatically determine whether a user can be found by number. Enforce both rules on the backend and in every relevant API; hiding a number only in the frontend is not sufficient.

The public profile should use `@username` and should not expose a phone number unless the account's privacy rules permit it.

## Contact graph and social graph

Keep these as separate concepts and data models:

- **Contact graph:** relationships that determine who can start private chats under the user's messaging settings.
- **Social graph:** follows, followers, and social-content relationships.

A follow is not a contact. Viewing public content is not permission to start a private chat.

## WhatsApp connector and native messaging

### WhatsApp Connector — initial bridge

The initial connector may use the unofficial `@whiskeysockets/baileys` library to link a user's WhatsApp session and provide WhatsApp chat access inside RUMPY.

Important limitations:
- Baileys is an unofficial WhatsApp Web implementation, not an official Meta WhatsApp API.
- Use can carry account restriction or blocking risks, and protocol changes can break the integration.
- Do not describe this integration as officially supported or risk-free.
- Keep the connector isolated and replaceable. Connector failures must not take down native RUMPY accounts, social features, or messaging.

### RUMPY Native Messaging — independent system

Build a separate native messaging service for conversations between RUMPY users. It must not rely on WhatsApp JIDs, WhatsApp sessions, or WhatsApp databases.

WhatsApp Connector and RUMPY Native Messaging are separate channels with separate lifecycle and data ownership. WhatsApp should become an optional integration as the RUMPY network grows.

## Architecture principles

- RUMPY owns its user identity, profiles, social graph, contact graph, content, and native messages.
- Keep external messaging connectors modular (starting with WhatsApp; other connectors may be considered later).
- Store connector credentials securely, encrypt sensitive session data, and isolate access by account.
- Apply authorization, rate limits, abuse prevention, and privacy checks on the server.
- Unlinking a connector must not delete the RUMPY account or native RUMPY data.
- Do not couple public content distribution to a user's WhatsApp contact list.

## Roadmap

### Phase 1 — RUMPY accounts and WhatsApp bridge
- Phone-number registration and OTP verification.
- Permanent RUMPY user identity and public username.
- Profile and privacy settings.
- WhatsApp connector as an optional bridge.
- Public social feed, Stories, and posts.

### Phase 2 — Native RUMPY network
- Native private messaging between RUMPY users.
- Contact management and chat permission enforcement.
- Native notifications and media handling.
- Continue supporting WhatsApp as an optional connector.

### Phase 3 — Independent RUMPY ecosystem
- Native RUMPY communication is the primary experience.
- WhatsApp connection is optional.
- Users can register, discover, publish, and communicate without having WhatsApp.

This roadmap is directional. Do not mark planned features as implemented until the repository contains working code and they have been verified.

## Brand assets

- `public/brand/rumpy-icon.svg` — scalable master app icon.
- `public/brand/rumpy-icon.png` — 1024 × 1024 PNG app icon, if present in the repository.
- `public/favicon.svg` — browser favicon.
- `BRAND.md` — color, logo, and usage guidelines.

## Design principles

1. Keep the symbol recognizable without the RUMPY wordmark.
2. Prefer flat shapes and strong contrast.
3. Use Merah Nusantara as the primary brand color and white for the symbol.
4. Avoid gradients, extra outlines, mascots, and decorative details in the core identity.

## Instructions for AI assistants working on this repository

**This README.md is the living source of truth for RUMPY's product direction, decisions, and implementation status.**

1. Read this README before making repository changes.
2. Whenever you read this README as part of a repository task, update `README.md` in the same task to incorporate any new decisions, requirements, architecture changes, or verified implementation progress from the conversation and work performed.
3. Keep the README accurate and concise. Preserve existing decisions unless the user explicitly changes them.
4. Clearly distinguish **planned**, **in progress**, and **implemented** features. Never claim a feature is implemented without verifying the code.
5. If a user changes a product decision, update the relevant section so the newest decision becomes authoritative; remove or correct conflicting outdated guidance.
6. Update the README together with related code/documentation changes, preferably in the same branch and task. Do not silently leave important new decisions only in chat.
7. Do not invent requirements, implementation details, test results, or completed work. If something is undecided, label it as undecided.
8. Keep changes limited to the user's request and the information needed to keep this document current.
