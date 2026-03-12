# Akumuluj — Brand Specification
> Source of truth dla wszystkich warstw wizualnych i komunikacyjnych.
> Wersja: 1.0 | Extracted from: akumuluj_shorts_60s_mockup.html

---

## 1. Tożsamość marki

**Nazwa:** Akumuluj  
**Format:** YouTube Shorts (1080×1920px) + Instagram Reels  
**Tematyka:** Finanse osobiste — oszczędzanie, ETF, DCA, procent składany, budowanie majątku  
**Grupa docelowa:** Zwykli Polacy bez finansowego backgroundu  
**Pozycjonowanie:** Mądry znajomy, który rozumie pieniądze i mówi po ludzku  
**Cel biznesowy:** Lejek sprzedażowy → kursy, e-booki, narzędzia  

---

## 2. Paleta kolorów

### Kolory podstawowe
| Nazwa | Hex | Użycie |
|---|---|---|
| **Void** | `#07080D` | Tło ekranu (główne) |
| **Deep Dark** | `#0a0a0a` | Tło strony / outer |
| **Dark Layer** | `#0D0F1A` | Wewnętrzne elementy (logo inner circle) |
| **Silver** | `#C8CDD8` | Tekst drugorzędny, subtelne elementy UI |
| **White Text** | `#F4F5F8` | Główny tekst, nagłówki |
| **Teal** | `#00C9A7` | Akcent, eyebrow labels, pozytywne wskaźniki |
| **Gold** | `#C9A84C` | Główny akcent premium, liczby, CTA, logo |

### Kolory z opacity (często używane)
```css
rgba(201,168,76, 0.2)   /* Gold subtle — obramowania */
rgba(201,168,76, 0.05)  /* Gold barely-there — tła */
rgba(200,205,216,0.45)  /* Silver — tekst pomocniczy */
rgba(200,205,216,0.25)  /* Silver dim — bardzo ciche elementy */
rgba(0,201,167, 0.06)   /* Teal glow — box shadow */
rgba(255,255,255,0.03)  /* White barely — tła kart */
rgba(255,255,255,0.06)  /* White subtle — obramowania kart */
```

### Zasady użycia kolorów
- **Void** (`#07080D`) zawsze jako tło ekranu — nigdy nie zastępuj innym ciemnym kolorem
- **Gold** to kolor premium — stosuj do: liczb, kluczowych słów, CTA, logo, akcentów
- **Teal** to kolor akcji/ruchu — stosuj do: eyebrow labels, strzałek, pozytywnych wskaźników, linii postępu
- **Silver** to kolor informacji — tekst pomocniczy, podtytuły, etykiety osi
- Nigdy nie używaj jaskrawych kolorów poza paletą
- Nigdy nie używaj białego tła ani jasnych backgroundów

---

## 3. Typografia

### Fonty
```
Playfair Display (serif) — Google Fonts
Inter (sans-serif) — Google Fonts

Import:
https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700&family=Inter:wght@300;400;500;600&display=swap
```

### Hierarchia typograficzna
| Rola | Font | Rozmiar | Weight | Kolor |
|---|---|---|---|---|
| **Headline / Big Number** | Playfair Display | 26–56px | 400/700 | `#F4F5F8` lub `#C9A84C` |
| **Lesson / Quote** | Playfair Display | 20–22px | 400 | `#F4F5F8` |
| **Question** | Playfair Display | 22px | 400 | `rgba(244,245,248,0.7)` |
| **Answer** | Playfair Display | 28px | 400 | `#C9A84C` |
| **Brand Name** | Playfair Display | 9–18px | 400 | `rgba(200,205,216,0.45–0.6)` |
| **Body / List text** | Inter | 13px | 400/600 | `rgba(244,245,248,0.85)` |
| **Eyebrow / Labels** | Inter | 8–9px | 400 | `#00C9A7` |
| **Subtitles** | Inter | 11px | 600 | `#fff` |
| **Captions / Meta** | Inter | 8–10px | 300/400 | `rgba(200,205,216,0.3–0.5)` |

### Zasady typografii
- Letter-spacing na eyebrow labels: minimum `0.2em`
- Letter-spacing na brand name: `0.28–0.3em`
- Text-transform: uppercase TYLKO dla eyebrow labels i brand name
- Line-height na headline: `1.3–1.5`
- Playfair Display = emocja, ciężar, premium
- Inter = informacja, czytelność, UI

---

## 4. Struktura ekranu (1080×1920px)
```
┌─────────────────────────┐
│  [top gradient line]    │  ← 2px, gold→silver→teal
│                         │
│  [glow overlay]         │  ← radial teal top, gold bottom-left
│  [grid texture]         │  ← 28x28px subtle grid
│                         │
│  [SCENE CONTENT]        │  ← padding: 28px 22px
│                         │
│  [subtitles]            │  ← bottom: 52px
│  [brand bar]            │  ← bottom: 16px
│  [progress bar]         │  ← bottom: 0px, 2px height
└─────────────────────────┘
```

### Stałe elementy ekranu (zawsze obecne)
1. **Top gradient line** — 2px, `linear-gradient(90deg, transparent, #C9A84C 30%, #C8CDD8 50%, rgba(0,201,167,0.5) 70%, transparent)`
2. **Grid texture** — `rgba(255,255,255,0.012)`, 28×28px, z-index 1
3. **Glow overlay** — radial gradient teal na górze, gold po lewej na dole
4. **Brand bar** — logo SVG (14px) + "AKUMULUJ" w Playfair Display, bottom 16px, centered
5. **Progress bar** — 2px, `linear-gradient(90deg, #C9A84C, #00C9A7)`, bottom 0
6. **Subtitles bar** — bottom 52px, słowa pojawiają się sekwencyjnie

---

## 5. Logo Akumuluj

Plik: `brand/assets/akumuluj_logo.svg`

### Warianty rozmiarów
- **Full CTA**: 52px (scena CTA)
- **Brand bar**: 14px (stały element dolny)

### Zasady
- Logo SVG jest zawsze inline w HTML artifacts, nigdy jako `<img src>`
- Przy inline użyciu: skopiuj kod SVG z `brand/assets/akumuluj_logo.svg`
- Skaluj przez `width` i `height` na elemencie SVG

---

## 6. Tone of voice

### Zasady komunikacji
- **Konkretny** — liczby, przykłady, nie abstrakcje
- **Bezpośredni** — "ty", nie "wy", nie "inwestor"
- **Ludzki** — jak znajomy który zna się na finansach
- **Bez żargonu** — zamiast "aktywa" → "pieniądze"
- **Nie motywacyjny** — nie "możesz to zrobić!", ale "tak to działa"

### Struktura narracji shorta
```
1. HOOK        — prowokacja, zaskoczenie, konkretna liczba lub historia
2. LICZBA      — jeden mocny fakt liczbowy
3. PORÓWNANIE  — kontekst, dlaczego to ważne
4. LEKCJA      — główny wniosek, jedna myśl
5. LISTA       — 3 kroki lub 3 fakty (opcjonalna)
6. WYKRES      — wizualizacja danych (opcjonalna)
7. PYTANIE     — retoryczne, angażujące
8. CTA         — subtelne, bez agresji
```

### Czego unikać w copy
- "Pamiętaj żeby..." / "Nie zapomnij..."
- Korporacyjny żargon: "monetyzacja", "optymalizacja portfela"
- Nadmierne wykrzykniki
- Clickbait bez pokrycia

### Przykład dobrego hook
> "200 złotych miesięcznie zmieniło życie tej nauczycielki na zawsze."

### Przykład złego hook
> "Odkryj sekret bogactwa który banki przed tobą ukrywają!"

---

## 7. Sceny — typologia

| Typ sceny | Opis | Typowy czas |
|---|---|---|
| `hook` | Prowokacyjne zdanie otwierające | 5–7s |
| `number` | Jedna duża animowana liczba | 7–9s |
| `compare` | Porównanie dwóch podejść/wyników | 7–9s |
| `lesson` | Główna lekcja / cytat | 6–8s |
| `list` | Lista 2–3 kroków lub faktów | 9–13s |
| `chart` | Wykres słupkowy lub liniowy | 7–9s |
| `question` | Pytanie retoryczne + odpowiedź | 5–7s |
| `cta` | Brand reveal + subskrypcja | 4–5s |

---

## 8. Synchronizacja z voiceoverem
```javascript
window.AKUMULUJ_TIMING = [6, 8, 8, 8, 12, 8, 6, 4]; // sekundy per scena
```

Render server wstrzykuje ten array przed nagraniem. HTML artifact musi go czytać.
