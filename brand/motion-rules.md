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

**Timing:** 0.4s ease  
**Typ:** zawsze opacity fade — nigdy slide całej sceny  
**Nigdy:** transform na całą scenę, zoom, flip

---

## 3. Entrance animations — katalog

### 3.1 Fade + Slide Up (standard text)
Dla: nagłówki, body text, listy
```css
opacity: 0;
transform: translateY(16px);
transition: all 0.6s ease;
/* active: */
opacity: 1;
transform: translateY(0);
```

### 3.2 Fade + Slide Left (eyebrow labels, list items)
```css
opacity: 0;
transform: translateX(-16px);
transition: all 0.5s ease;
/* active: */
opacity: 1;
transform: translateX(0);
```

### 3.3 Scale Bounce (big numbers, logo)
```css
opacity: 0;
transform: scale(0.6);
transition: all 0.6s cubic-bezier(0.34,1.56,0.64,1);
/* active: */
opacity: 1;
transform: scale(1);
```

### 3.4 Scale Soft (question text)
```css
opacity: 0;
transform: scale(0.9);
transition: all 0.6s ease;
/* active: */
opacity: 1;
transform: scale(1);
```

### 3.5 Width Expand (linie poziome)
```css
width: 0;
transition: width 0.5s ease;
/* active: */
width: 48px;
```

### 3.6 Height Expand (linie pionowe)
```css
height: 0;
transition: height 0.5s ease;
/* active: */
height: 44px;
```

### 3.7 Chart Bars
```css
height: 0;
transition: height 0.7s cubic-bezier(0.34,1.2,0.64,1);
```
Delay: +200ms per słupek (staggered)

### 3.8 SVG Path Draw
```css
stroke-dashoffset: [length];
transition: stroke-dashoffset 1.0s ease;
/* active: */
stroke-dashoffset: 0;
```

### 3.9 SVG Ring
```css
stroke-dashoffset: [circumference];
transition: stroke-dashoffset 2.0s ease;
/* active: */
stroke-dashoffset: 0;
```

### 3.10 Clock Hand
```css
transform-origin: [cx]px [cy]px;
transform: rotate(0deg);
transition: transform 2.8s cubic-bezier(0.4,0,0.2,1);
/* active: */
transform: rotate(720deg);
```

---

## 4. Staggered delays
```css
.s-list.active .list-item:nth-child(2) { transition-delay: 0.20s; }
.s-list.active .list-item:nth-child(3) { transition-delay: 0.45s; }
.s-list.active .list-item:nth-child(4) { transition-delay: 0.70s; }
```

- Pierwszy element: 0.1–0.2s delay
- Każdy kolejny: +0.2–0.25s
- Maksymalny delay ostatniego: ~0.8s

---

## 5. Animated counters (JS)
```javascript
function animCounter(target, duration, element) {
  const start = Date.now();
  const tick = () => {
    const p = Math.min((Date.now() - start) / duration, 1);
    const ease = 1 - Math.pow(1 - p, 3);
    const val = Math.round(ease * target);
    element.textContent = val.toLocaleString('pl') + ' zł';
    if (p < 1) requestAnimationFrame(tick);
  };
  requestAnimationFrame(tick);
}
```

- Easing: ease-out cubic
- Format: `toLocaleString('pl')` — polskie separatory
- Czas trwania: 1200–2500ms

---

## 6. Subtitles
```javascript
subs: [
  { w: 'słowo', t: 400 },
  { w: 'kluczowe', t: 650, h: true } // h:true = złoty kolor
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
  transition: all 0.12s ease;
}
.sub-word.vis { opacity: 1; transform: translateY(0); }
.sub-word.hi  { color: #C9A84C; background: rgba(201,168,76,0.12); }
```

Pozycja: `bottom: 52px`, centered, z-index 20  
Znikają: 300ms przed końcem sceny

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

---

## 8. Stałe efekty ambient

### Top gradient line
```css
height: 2px;
background: linear-gradient(90deg,
  transparent,
  #C9A84C 30%,
  #C8CDD8 50%,
  rgba(0,201,167,0.5) 70%,
  transparent
);
```

### Ambient glow
```css
background:
  radial-gradient(ellipse 80% 40% at 50% 20%, rgba(0,201,167,0.05) 0%, transparent 60%),
  radial-gradient(ellipse 60% 40% at 20% 70%, rgba(201,168,76,0.04) 0%, transparent 50%);
```

### Grid texture
```css
background-image:
  linear-gradient(rgba(255,255,255,0.012) 1px, transparent 1px),
  linear-gradient(90deg, rgba(255,255,255,0.012) 1px, transparent 1px);
background-size: 28px 28px;
```

---

## 9. window.AKUMULUJ_TIMING
```javascript
const TIMING = window.AKUMULUJ_TIMING || [6, 8, 8, 8, 12, 8, 6, 4];
```

Render server wstrzykuje przed nagraniem:
```javascript
window.AKUMULUJ_TIMING = [/* scenes[].duration_s z script.json */];
```

---

## 10. Czego NIGDY nie robić

- ❌ Nie animuj tła całej sceny
- ❌ Nie używaj `animation: spin` na tekście
- ❌ Nie rób slide całej sceny
- ❌ Nie używaj więcej niż 3 typów animacji w jednej scenie
- ❌ Nie skracaj entrance animations poniżej 0.3s
- ❌ Nie używaj bibliotek JS (jQuery, GSAP itp.)
- ❌ Nie mieszaj `transition` i `animation` na tym samym elemencie
