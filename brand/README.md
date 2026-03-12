# Akumuluj — Brand & Pipeline Repository

> Source of truth dla systemu automatycznych YouTube Shorts.

---

## Struktura repo

```
akumuluj/
├── brand/
│   ├── brand-spec.md          ← kolory, typografia, tone of voice, logo
│   └── motion-rules.md        ← zasady animacji, timing, easing, subtitles
│
├── prompts/
│   ├── script-gen.md          ← system + user prompt do generowania script.json
│   └── html-gen.md            ← system + user prompt do generowania HTML artifact
│
├── contracts/
│   ├── script.schema.json     ← JSON schema kontraktu skryptu
│   └── job.schema.json        ← JSON schema state machine joba
│
├── artifacts/
│   ├── approved/              ← zatwierdzone HTML artifacts (few-shot dla AI)
│   └── drafts/                ← drafty (czyszczone co tydzień)
│
└── workflows/
    └── n8n-exports/           ← JSON eksporty workflowów n8n
```

---

## Pipeline — overview

```
Telegram input
    ↓
WF-1: Script Generation
    ↓
Telegram: draft do review
    ↓ (approve)
WF-3: Production
  ├── ElevenLabs voiceover
  └── Claude HTML artifact
    ↓
Render server (Hetzner + Puppeteer)
    ↓
Telegram: draft video do review
    ↓ (approve)
artifacts/approved/ (GitHub)
```

---

## Kontrakty danych

### script.json — format per short
Pełny schemat: `contracts/script.schema.json`

Kluczowe pola:
- `job_id` — unikalny ID
- `voiceover.text` — tekst ~120 słów do ElevenLabs
- `scenes[]` — tablica scen z `type`, `duration_s`, `headline`, `visual_hint`
- `output` — URLe do gotowych artifacts

### window.AKUMULUJ_TIMING — timing contract
Każdy HTML artifact czyta:
```javascript
const TIMING = window.AKUMULUJ_TIMING || [6, 8, 8, 8, 12, 8, 6, 4];
```
Render server wstrzykuje array z `scenes[].duration_s` przed nagraniem.

---

## Brand DNA — quick reference

| Element | Wartość |
|---|---|
| Tło | `#07080D` |
| Akcent gold | `#C9A84C` |
| Akcent teal | `#00C9A7` |
| Tekst | `#F4F5F8` |
| Font nagłówki | Playfair Display |
| Font tekst | Inter |
| Format | 1080×1920px pionowy |

---

## Dodawanie zatwierdzonego artifact

Po zatwierdzeniu shorta na Telegramie, n8n automatycznie pushuje HTML do:
```
artifacts/approved/akumuluj-YYYYMMDD-NNN-topic-slug.html
```

Commit message format: `feat: approved [job_id] - [topic_slug]`

Te pliki są używane jako few-shot examples przy generowaniu kolejnych shortów.

---

## Wersjonowanie

- `main` — produkcja, tylko zatwierdzone artifacts
- PRy nie są potrzebne — n8n pushuje bezpośrednio przez GitHub API
- Brand spec i prompty zmieniasz ręcznie (edit na GitHubie lub lokalnie)
