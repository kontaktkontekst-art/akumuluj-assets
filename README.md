# Akumuluj — Brand & Pipeline Repository

> Source of truth dla systemu automatycznych YouTube Shorts.

## Struktura repo
```
akumuluj-assets/
├── brand/
│   ├── brand-spec.md
│   ├── motion-rules.md
│   └── assets/
│       └── akumuluj_logo.svg
├── prompts/
│   ├── script-gen.md
│   └── html-gen.md
├── contracts/
│   ├── script.schema.json
│   └── job.schema.json
└── artifacts/
    ├── approved/
    └── drafts/
```

## Pipeline
```
Telegram → n8n WF-1 → Script draft → Review
                                        ↓ approve
                              WF-3: ElevenLabs + HTML gen
                                        ↓
                              Render (Hetzner + Puppeteer)
                                        ↓
                              Telegram: draft video → approve
                                        ↓
                              artifacts/approved/ (GitHub)
```

## Brand DNA — quick reference

| Element | Wartość |
|---|---|
| Tło | `#07080D` |
| Gold | `#C9A84C` |
| Teal | `#00C9A7` |
| Tekst | `#F4F5F8` |
| Font nagłówki | Playfair Display |
| Font tekst | Inter |
| Format | 1080×1920px pionowy |

## window.AKUMULUJ_TIMING

Każdy HTML artifact czyta:
```javascript
const TIMING = window.AKUMULUJ_TIMING || [6, 8, 8, 8, 12, 8, 6, 4];
```
Render server wstrzykuje array z `scenes[].duration_s` przed nagraniem.
