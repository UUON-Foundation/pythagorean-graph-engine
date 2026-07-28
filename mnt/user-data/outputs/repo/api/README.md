# API Layer — F=(P,E,R,C) Implementation

This directory is reserved for the server-side implementation of the
F=(P,E,R,C) API documented in the root README.

## Status

Not yet implemented. The contract (endpoints, schema, compression metrics)
is fully specified in `README_EXTENSION.md`.

## Planned Stack

- Runtime: Node.js / TypeScript (consistent with UUON production stack)
- Database: Neon PostgreSQL (graph persistence)
- Auth: API key middleware (`UUON_API_KEY` from `.env`)
- Deploy target: Railway

## Files to be added here

```
api/
├── server.ts          # Express entry point
├── routes/
│   ├── generate.ts    # POST /generate
│   ├── encode.ts      # POST /encode
│   ├── reconstruct.ts # POST /reconstruct
│   ├── render.ts      # POST /render
│   ├── fingerprint.ts # POST /fingerprint
│   └── compare.ts     # POST /compare
├── lib/
│   ├── topology.ts    # Core recursive graph builder (port from HTML engine)
│   ├── hash.ts        # SHA-256 of P and E
│   └── schema.ts      # Graph JSON schema validator
└── middleware/
    └── auth.ts        # API key validation
```

## Origin

UUON Foundation Inc. — Phillip Aguilar Ruiz III
phi1@uuonfoundation.com
