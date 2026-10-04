# ASCE SBT Registry — 3V-IP Concept Prototype for Discussion

A polished React/Vite proof-of-concept for demonstrating a non-transferable professional credential registry. This is **not an official ASCE platform** and does not connect to ASCE systems, blockchain networks, wallets, or identity providers.

## What works

- Home/about explanation for non-technical viewers
- Record submission form with work + personal email, required-field validation, repeatable claim/evidence pairs, and optional application/resume file selection
- Simulated ASCE review, verification, and validation workflow with loading state
- Unique SBT ID and local hash-like identifier generation
- Visually polished generated credential with locally rendered QR code
- Provenance section
- Searchable registry
- Filter by record type and verification status
- Sort newest/oldest
- Automatic summary statistics
- Full record detail view
- Browser `localStorage` persistence across refreshes
- Reset Demo Data action
- Six fictional seeded records

## Run locally

Prerequisite: Node.js 18+ (Node 20+ recommended).

```bash
npm install
npm run dev
```

Vite will print a local URL, usually `http://localhost:5173`.

For a production build:

```bash
npm run build
npm run preview
```

## File structure

```text
asce-sbt-registry/
├─ index.html
├─ package.json
├─ README.md
└─ src/
   ├─ main.jsx
   └─ styles.css
```

## Architecture

This prototype intentionally uses a very small architecture:

- **React** for UI and state
- **Vite** for local development/building
- **localStorage** for persistence, avoiding a backend for the demo
- **lucide-react** for interface icons
- **qrcode.react** for an entirely local QR visual
- Seed data lives in `src/main.jsx`
- The UI is organized into page-like React components but uses a lightweight internal view state instead of a router, keeping the demo simple

A production system could replace the local data functions with authenticated API calls while keeping most of the UI structure.

## SBT data model

Each record contains fields like:

```js
{
  sbtId,
  ownerName,
  memberId,
  workEmail,
  personalEmail,
  recordType,
  title,
  description,
  issuer,
  issueDate,
  expirationDate,
  verificationStatus,
  claims,
  evidenceReviewed,
  applicationFileName,
  resumeFileName,
  reviewerContext,
  provenance,
  createdAt,
  recordHash,
  transferable: false,
  demo: true
}
```

### Why these fields matter

- `sbtId`: persistent public identifier for the record
- `ownerName` / `memberId`: subject the record is bound to
- `recordType`, `title`, `description`: what the record represents
- `issuer`: organization responsible for the credential/record
- `verificationStatus`: prototype review state (for example, Verified & Validated)
- `claims`: repeatable claim/evidence pairs so each statement can be reviewed against its own support
- `provenance`: human-readable trail explaining where the record came from
- `createdAt`: record creation timestamp
- `recordHash`: prototype fingerprint generated locally from the record contents and timestamp
- `transferable: false`: visually and structurally reinforces the SBT concept

The record reference is **not cryptographic blockchain proof**. It is a local prototype identifier. The interface intentionally uses plain language for the ASCE-facing demo; technical terms such as API, HTTP, and blockchain are reserved for the implementation/production discussion.

## How to modify the demo

### Wording
Edit text directly in `src/main.jsx`. Search for phrases such as:

- `3V-IP Concept Prototype for Discussion`
- `What is an SBT`
- `Proposed ASCE use case`
- `Demo Verification`

### Colors / visual design
Edit CSS variables and color values in `src/styles.css`. The main brand colors are built around:

- dark engineering blue: `#0b3b66`
- navy text: `#102c43`
- light neutral background: `#f5f8fb`

### Registration fields
Edit the `Register` component in `src/main.jsx`. The `f` state object defines the current form model. Update validation in the `submit` function if required fields change.

### Record types
Edit the `TYPES` array near the top of `src/main.jsx`.

### Sample records
Edit the `seed` array near the top of `src/main.jsx`. Keep demo data fictional if this is used for presentations.

### Reset behavior
`Reset Demo Data` replaces browser data with the `seed` array. The storage key is `asce-sbt-registry-v1`.

## What would change for production

A real registry would need substantially more than this concept demo, including:

1. **Authenticated users and role-based permissions** for members, issuers, reviewers, administrators, and auditors.
2. **ASCE membership integration** or another authoritative membership source.
3. **Real issuer verification** and an issuer trust/authorization model.
4. **Secure backend/database** instead of localStorage, with audit history and backups.
5. **Cryptographic signing** of records and/or verifiable credentials. A blockchain should only be introduced if its governance and trust benefits justify the added complexity.
6. **Immutable audit trail / versioning** so corrections do not erase provenance.
7. **Evidence storage policy** with retention, privacy, access controls, and tamper-evident references.
8. **Revocation, suspension, expiration, and supersession states** rather than only `Verified`.
9. **Privacy and consent design**, especially because professional identity data may be sensitive or not intended to be public.
10. **API layer** separating the UI from record creation, verification, search, and reporting services.
11. **Standards review**, potentially using W3C Verifiable Credentials / Decentralized Identifiers where useful rather than inventing an incompatible format.
12. **Security testing, logging, monitoring, disaster recovery, accessibility, and governance** before organizational use.

## Notes

This prototype intentionally does **not** implement Ethereum, cryptocurrency, wallets, MetaMask, smart contracts, token minting, ASCE APIs, ASCE authentication, or real identity verification.

## Prototype v5 edits
- Separates SBT issuer/signature verification from ASCE staff claim validation.
- Shows a prototype JWT signature and issuer identity.
- Adds member-controlled disclosure dropdowns for audience, visible field scope, and discoverability.
- Makes the JSON/object-store section explicitly machine-readable and not intended as the member-facing interface.
- Adds a placeholder "Launch GitHub Object Store Preview" button for a future repository/preview URL.
- Updates the AI sandbox trace to enforce issuer verification, ASCE validation, and member permissions before retrieval.

Note: issuer IDs, JWTs, validators, and records in this demo are fictional placeholders. The GitHub button is intentionally a prototype action until a real object-store URL is selected.
