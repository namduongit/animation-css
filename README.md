# CSS Animation Easing — A Beautiful Quick Guide

Smooth motion sells the illusion of life. This README gives you a concise, visual way to pick the right easing for your CSS transitions and animations, plus copy‑paste snippets.

---

## What is “easing”?

**Easing** controls **how speed changes over time** during an animation. Instead of moving at one constant speed, you can accelerate, decelerate, or do both for more natural motion.

> Think: **SP** = Start Pace, **MP** = Mid Pace, **ED** = End Pace.

---

## Example AnimationCSS
```css
.root {
    animation-name: spin;
    animation-duration: 5s;
    animation-delay: 1s;
    animation-iteration-count: infinite;
    animation-direction: reverse;
    animation-play-state: paused;
    /* Or */
    animation: name duration timing-function delay iteration-count direction fill-mode;
}
```

## Keyword Easings (Built‑in)

These 5 are supported in all modern browsers.

| Name             | SP (Start Pace) | MP (Mid Pace) | ED (End Pace) | Cubic Bezier                    | Notes                                                          |
| ---------------- | --------------- | ------------- | ------------- | ------------------------------- | -------------------------------------------------------------- |
| `linear`         | constant        | constant      | constant      | `cubic-bezier(0,0,1,1)`         | Mechanical, useful for loaders and progress bars.              |
| `ease` (default) | slow            | fast          | slow          | `cubic-bezier(0.25,0.1,0.25,1)` | Good default; gentle in/out.                                   |
| `ease-in`        | slow            | accelerating  | fast          | `cubic-bezier(0.42,0,1,1)`      | Starts soft, speeds up. Great for entering elements.           |
| `ease-out`       | fast            | decelerating  | slow          | `cubic-bezier(0,0,0.58,1)`      | Starts quick, settles nicely. Great for exits or button hover. |
| `ease-in-out`    | slow            | fast          | slow          | `cubic-bezier(0.42,0,0.58,1)`   | Symmetric; balanced enter/exit.                                |

> **Tip:** Use `ease-out` for UI micro‑interactions (feels snappy yet soft), and `ease-in-out` for larger movement.

---

## Drop‑in Example

```html
<!-- index.html -->
<div class="card">Hover me</div>

<style>
  :root {
    /* swap among: linear | ease | ease-in | ease-out | ease-in-out | or your own cubic-bezier */
    --easing: ease-out;
    --duration: 420ms;
  }

  .card {
    width: 220px; height: 140px;
    display: grid; place-content: center;
    border-radius: 16px; border: 1px solid #e5e7eb;
    background: linear-gradient(180deg,#ffffff, #f8fafc);
    box-shadow: 0 2px 6px rgba(0,0,0,.06);
    transform: translateY(0) scale(1);
    transition: transform var(--duration) var(--easing), box-shadow var(--duration) var(--easing);
    font: 600 16px/1.2 ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Inter, "Helvetica Neue", Arial;
  }
  .card:hover {
    transform: translateY(-6px) scale(1.02);
    box-shadow: 0 12px 30px rgba(0,0,0,.10);
  }
</style>
```

---

## Visual Intuition (ASCII curves)

> Rough sketches of speed change over time. **─** constant, **╭** accelerate, **╯** decelerate.

```
linear        ──────────
ease          ╭─────╮
              ╰─────╯
ease-in       ╭───────
ease-out      ───────╯
ease-in-out   ╭────╮
              ╰────╯
```

---

## Popular Custom Curves (Copy‑paste)

These aren’t keywords, but they’re great presets via `cubic-bezier(x1,y1,x2,y2)`.

| Nickname        | Character         | Bezier                              | Feels like…             |
| --------------- | ----------------- | ----------------------------------- | ----------------------- |
| **Sine‑in**     | soft accel        | `cubic-bezier(0.12, 0, 0.39, 0)`    | Gentle start            |
| **Sine‑out**    | soft decel        | `cubic-bezier(0.61, 1, 0.88, 1)`    | Gentle stop             |
| **Sine‑in‑out** | soft both         | `cubic-bezier(0.37, 0, 0.63, 1)`    | Smooth both             |
| **Quad‑in**     | stronger accel    | `cubic-bezier(0.11, 0, 0.5, 0)`     | Punchier start          |
| **Quad‑out**    | stronger decel    | `cubic-bezier(0.5, 1, 0.89, 1)`     | Punchier stop           |
| **Cubic‑in**    | very strong accel | `cubic-bezier(0.32, 0, 0.67, 0)`    | Bold entry              |
| **Cubic‑out**   | very strong decel | `cubic-bezier(0.33, 1, 0.68, 1)`    | Bold settle             |
| **Circ‑out**    | dramatic stop     | `cubic-bezier(0, 0.55, 0.45, 1)`    | Natural, circular feel  |
| **Back‑out**    | overshoot         | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Tiny bounce past target |

**Use:**

```css
.element { transition: transform 380ms cubic-bezier(0.34, 1.56, 0.64, 1); }
```

---

## Animation Keyframes Example

```html
<div class="chip">Add to cart</div>

<style>
  .chip {
    display:inline-block; padding:.6rem .9rem; border-radius:999px;
    background:#111827; color:#fff; font:500 14px/1 ui-sans-serif, system-ui;
    animation: pop-in 600ms ease-out both;
  }
  @keyframes pop-in {
    0%   { transform: translateY(8px) scale(.96); opacity: 0; }
    60%  { transform: translateY(-2px) scale(1.02); opacity: 1; }
    100% { transform: translateY(0) scale(1); }
  }
</style>
```

---

## Picking the Right Easing

* **Micro‑interactions (hover, press):** `ease-out` or a custom gentle `…1, 0.88, 1)` curve.
* **Entering elements:** `ease-in` or `ease-in-out` if movement is large.
* **Exiting elements:** `ease-out` for a clean settle.
* **Mechanical motion (timers/progress):** `linear`.
* **Playful UI:** try `back-out` (small overshoot) but keep durations short.

**Duration heuristics**

* Hover/press: **120–220 ms**
* Small UI move: **180–280 ms**
* Large panel/sheet: **300–480 ms**
* Page transitions: **400–700 ms**

---

## How cubic‑bezier Works (quick math)

`cubic-bezier(x1, y1, x2, y2)` defines two control points between (0,0) and (1,1). X = time, Y = progress.

* `x1, x2`: how quickly you accelerate/decelerate.
* `y1, y2` beyond 0–1 cause overshoot/bounce‑like behavior.

---

## One‑liner Utility (SCSS)

```scss
// usage: transition: transform ease($out); or ease($back-out)
$linear:      cubic-bezier(0, 0, 1, 1);
$ease:        cubic-bezier(0.25, 0.1, 0.25, 1);
$in:          cubic-bezier(0.42, 0, 1, 1);
$out:         cubic-bezier(0, 0, 0.58, 1);
$in-out:      cubic-bezier(0.42, 0, 0.58, 1);
$back-out:    cubic-bezier(0.34, 1.56, 0.64, 1);

@mixin ease($curve) {
  transition-timing-function: $curve;
}
```

---

## TL;DR Table (SP / MP / ED)

| Name        | SP       | MP           | ED       |
| ----------- | -------- | ------------ | -------- |
| linear      | constant | constant     | constant |
| ease        | slow     | fast         | slow     |
| ease-in     | slow     | accelerating | fast     |
| ease-out    | fast     | decelerating | slow     |
| ease-in-out | slow     | fast         | slow     |

---

## Further ideas

* Chain easings across **multiple properties** (e.g., `opacity` faster than `transform`).
* Reduce motion for users with preferences using `@media (prefers-reduced-motion: reduce)`.

```css
@media (prefers-reduced-motion: reduce) {
  * { animation: none !important; transition: none !important; }
}
```

---

