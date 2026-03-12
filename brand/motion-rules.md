# Akumuluj — Motion Rules
> Zasady animacji dla wszystkich HTML artifacts.
> Źródło: akumuluj_shorts_60s_mockup.html | Wersja: 1.0

---

## 1. Filozofia animacji

Animacje w Akumuluj NIE są dekoracyjne. Każda animacja ma cel:
- **Entrance** — wprowadza element, kieruje uwagę
- **Reveal** — odkrywa dane sekwencyjnie (lista, wykres)
- **Emphasis** — podkreśla kluczową liczbę lub wniosek
- **Transition** — płynnie przechodzi między scenami

**Zasada:** Jeden element wchodzi w jednym momencie. Nie animuj wszystkiego naraz.

---

## 2. Przejścia między scenami

```css
/* Standard scene transition */
.scene {
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.4s ease;
}
.scene.active {
  opacity: 1;
  pointer-events: auto;
}
```

**Timing:** 0.4s ease — krótkie, nie rozpraszające  
**Typ:** zawsze opacity fade — nigdy slide całej sceny  
**Nigdy:** transform na całą scenę, zoom, flip

---

## 3. Entrance animations — katalog

### 3.1 Fade + Slide Up (standard text)
Dla: nagłówki, body text, listy
```css
opacity: 0;
transform: translateY(16–18px);
transition: all 0.5–0.6s ease;
/* active: */
opacity: 1;
transform: translateY(0);
```

### 3.2 Fade + Slide Left (eyebrow labels, list items)
Dla: etykiety kategorii, pozycje list
```css
opacity: 0;
transform: translateX(-16px);
transition: all 0.5s ease;
/* active: */
opacity: 1;
transform: translateX(0);
```

### 3.3 Scale Bounce (big numbers, logo)
Dla: główna liczba sceny, logo w CTA
```css
opacity: 0;
transform: scale(0.6);
transition: all 0.6s cubic-bezier(0.34,1.56,0.64,1);
/* active: */
opacity: 1;
transform: scale(1);
```
Easing `cubic-bezier(0.34,1.56,0.64,1)` daje lekki overshoot — efekt "spring".

### 3.4 Scale Soft (question text)
Dla: pytania, cytaty
```css
opacity: 0;
transform: scale(0.9);
transition: all 0.6s ease;
/* active: */
opacity: 1;
transform: scale(1);
```

### 3.5 Width Expand (linie poziome, akcentujące)
Dla: l-accent pod lesson, q-divider
```css
width: 0;
transition: width 0.5–0.6s ease;
/* active: */
width: 48–80px;
```

### 3.6 Height Expand (linie pionowe)
Dla: teal-accent w hook, l-bar w lesson
```css
height: 0;
transition: height 0.5s ease;
/* active: */
height: 40–44px;
```

### 3.7 Chart Bars (słupki wykresu)
Dla: scena chart
```css
height: 0;
transition: height 0.7s cubic-bezier(0.34,1.2,0.64,1);
/* active + delay per bar */
```
Delay: +200ms per słupek (staggered reveal)

### 3.8 SVG Path Draw (linie wykresów)
Dla: linie w SVG chart
```css
stroke-dashoffset: [length];
transition: stroke-dashoffset 1.0–1.4s ease;
/* active: */
stroke-dashoffset: 0;
```
Mierzysz długość ścieżki przez `path.getTotalLength()` lub ustawiasz manualnie.

### 3.9 SVG Ring (clock ring, circle progress)
```css
stroke-dashoffset: [circumference];
transition: stroke-dashoffset 2.0–2.5s ease;
/* active: */
stroke-dashoffset: 0;
```

### 3.10 Clock Hand Rotation
```css
transform-origin: [center_x]px [center_y]px;
transform: rotate(0deg);
transition: transform 2.8–3.0s cubic-bezier(0.4,0,0.2,1);
/* active: */
transform: rotate(720deg); /* 2 pełne obroty */
```

---

## 4. Staggered delays — zasady

Lista lub sekwencja elementów: każdy kolejny element ma +200–250ms delay.

```css
/* Przykład list items */
.s-list.active .list-item:nth-child(2) { transition-delay: 0.20s; }
.s-list.active .list-item:nth-child(3) { transition-delay: 0.45s; }
.s-list.active .list-item:nth-child(4) { transition-delay: 0.70s; }
```

Ogólna zasada stagger:
- Pierwszy element: `0.1–0.2s` delay (scena potrzebuje chwili na "osadzenie")
- Każdy kolejny: +0.2–0.25s
- Maksymalny delay dla ostatniego elementu w scenie: ~0.8s (żeby zdążył wejść)

---

## 5. Animated counters (JS)

Dla: dużych liczb animowanych w czasie

```javascript
function animCounter(target, duration, element) {
  const start = Date.now();
  const tick = () => {
    const p = Math.min((Date.now() - start) / duration, 1);
    const ease = 1 - Math.pow(1 - p, 3); // ease-out cubic
    const val = Math.round(ease * target);
    element.textContent = val.toLocaleString('pl') + ' zł';
    if (p < 1) requestAnimationFrame(tick);
  };
  requestAnimationFrame(tick);
}
```

**Czas trwania:** 1200–2500ms zależnie od dramatyzmu  
**Easing:** ease-out cubic (`1 - (1-p)^3`) — szybki start, zwalnia na końcu  
**Format liczb:** `toLocaleString('pl')` dla polskich separatorów (120 000, nie 120,000)

---

## 6. Subtitles — system napisów

Słowa pojawiają się sekwencyjnie, zsynchronizowane z voiceoverem.

```javascript
// Format danych
subs: [
  { w: 'słowo', t: 400 },          // t = timestamp w ms od startu sceny
  { w: 'kluczowe', t: 650, h: true } // h: true = kolor gold (#C9A84C)
]
```

```css
.sub-word {
  display: inline-block;
  font-size: 11px;
  font-weight: 600;
  color: #fff;
  background: rgba(0,0,0,0.75);
  padding: 2px 3px;
  margin: 1px;
  border-radius: 2px;
  opacity: 0;
  transform: translateY(3px);
  transition: all 0.12s ease; /* bardzo szybkie, niemal natychmiastowe */
}
.sub-word.vis { opacity: 1; transform: translateY(0); }
.sub-word.hi { color: #C9A84C; background: rgba(201,168,76,0.12); }
```

**Pozycja:** `bottom: 52px`, centered, z-index 20  
**Znikają:** 300ms przed końcem sceny

---

## 7. Progress bar

```css
.progress {
  position: absolute;
  bottom: 0; left: 0;
  height: 2px;
  background: linear-gradient(90deg, #C9A84C, #00C9A7);
  width: 0;
  z-index: 30;
  transition: width [duration]ms linear;
}
```

Animacja: `width: 0% → 100%` przez cały czas trwania sceny, linear.

---

## 8. Glow i ambient efekty

### Top gradient line (zawsze)
```css
.screen::after {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 2px;
  background: linear-gradient(90deg, 
    transparent, 
    #C9A84C 30%, 
    #C8CDD8 50%, 
    rgba(0,201,167,0.5) 70%, 
    transparent
  );
  z-index: 10;
}
```

### Ambient glow (zawsze)
```css
.glow {
  position: absolute;
  inset: 0;
  background:
    radial-gradient(ellipse 80% 40% at 50% 20%, rgba(0,201,167,0.05) 0%, transparent 60%),
    radial-gradient(ellipse 60% 40% at 20% 70%, rgba(201,168,76,0.04) 0%, transparent 50%);
  pointer-events: none;
}
```

### Grid texture (zawsze)
```css
.screen::before {
  content: '';
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(rgba(255,255,255,0.012) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,255,255,0.012) 1px, transparent 1px);
  background-size: 28px 28px;
  pointer-events: none;
}
```

---

## 9. Timing — window.AKUMULUJ_TIMING

Każdy HTML artifact MUSI czytać timing z `window.AKUMULUJ_TIMING`:

```javascript
const TIMING = window.AKUMULUJ_TIMING || [6, 8, 8, 8, 12, 8, 6, 4];
// array duration w sekundach per scena, od sceny 1 do N
```

Jeśli `window.AKUMULUJ_TIMING` nie jest ustawiony — artifact używa wartości domyślnych z mockupu.

Render server wstrzykuje:
```javascript
window.AKUMULUJ_TIMING = [/* z script.json scenes[].duration_s */];
```

---

## 10. Czego NIGDY nie robić

- ❌ Nie animuj tła całej sceny (flash, blink, color change)
- ❌ Nie używaj `animation: spin` na tekście
- ❌ Nie rób slide całej sceny (wchodzenie z boku)
- ❌ Nie używaj drop-shadow na tekście (tylko na elementach SVG/kart jeśli konieczne)
- ❌ Nie używaj więcej niż 3 różnych typów animacji w jednej scenie
- ❌ Nie skracaj entrance animations poniżej 0.3s (zbyt agresywne)
- ❌ Nie używaj `animation: bounce` z bibliotek — tylko custom cubic-bezier
- ❌ Nie mieszaj `transition` i `animation` na tym samym elemencie

---

## 11. Warianty scen compare — reference

Mockup zawiera 4 warianty sceny porównawczej. Każdy jest dopuszczalny:

| Wariant | Typ wizualizacji | Kiedy używać |
|---|---|---|
| **A** | Dwie linie SVG (flat vs DCA) | Porównanie wzrostu w czasie |
| **B** | Zegar + karty latowe | "Czas pracuje za ciebie", horyzont inwestycji |
| **C** | Dwa pierścienie SVG | Skuteczność dwóch strategii w % |
| **D** | Zegar + wykres liniowy (hybrid) | Połączenie czasu i wzrostu wartości |

AI generując nowy short może wybrać jeden z tych wariantów lub stworzyć nowy — o ile zachowuje brand DNA.
