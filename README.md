# Philosophical Worldview Quiz

A tiny, dependency‑free web app that gives users a personality quiz, based on a blog post I made up, to explore their **philosophical orientation** across three axes: **Empirical‑Scientific**, **Historical‑Traditionalist**, and **Freethinking‑Intuitionist**.

---

## ✨ Features

* **24 statements**, 8 per worldview; Likert‑style responses (0–3 pts).
* **Instant scoring** and animated meters with both **points** and **percentages**.
* **Shareable results** via URL query params (`?es=..&ht=..&fi=..`), plus one‑click **Copy result link**.
* **Review answers** list to jump back and edit individual questions.
* **Keyboard navigation**: `←` previous, `→` next.
* **Accessible**: announces question text and progress; semantic controls; decent contrast.
* **Print‑ready**: built‑in **Print** action for saving/printing your results.
* **No build step, no dependencies**: a single `index.html` with vanilla HTML/CSS/JS.
* **Theming** via CSS variables and cohesive dark gradient UI.

---

## 🧠 Worldviews

* **Empirical‑Scientific (ES):** defers to current science and empirical method.
* **Historical‑Traditionalist (HT):** honors the canon; favors continuity and careful reinterpretation.
* **Freethinking‑Intuitionist (FI):** starts from reflective plausibility; resists external authority.

The app reports each score independently; ties are shown as a **blend**.

---

## 🚀 Quick Start

1. **Clone or download** this repository.
2. Open `index.html` in any modern browser.

**Optional local server** (recommended for full Clipboard API support):

```bash
# Python 3
python -m http.server 8000
# then visit http://localhost:8000
```

> Note: Some browsers require a **secure context** (HTTPS or `localhost`) for `navigator.clipboard`. If copying the result link fails, the UI falls back gracefully and you can copy from the address bar.

---

## 🗂 Project Structure

```
.
└── index.html   # Single‑file app: markup, styles, and script
```

* **Styles**: CSS variables in `:root` set colors, radii, shadows, etc.
* **Script**: all logic is inline at the bottom (no external files).

---

## ⚙️ How It Works

### Data model

* **Worldviews** are defined in `WV` (key, name, color, description).
* **Questions** live in the `questions` array: 24 items, each `{ id, category: 'ES'|'HT'|'FI', text }`.
* **Choices** (`CHOICES`) define the Likert scale with `{ key, label, value }` where `value ∈ {0,1,2,3}`.

### State & navigation

* `state.responses` holds the selected value for each question (or `null`).
* The progress bar reflects `idx / total` while answering.
* Keyboard shortcuts: `ArrowLeft`/`ArrowRight` to navigate when the quiz is active.

### Scoring

* Each question contributes its **selected value** to its worldview’s total.
* Max per worldview = **8 questions × 3 pts = 24**.
* Percent = `Math.round((score / 24) * 100)`.
* The top percentage determines the **primary orientation**; exact ties display a blend.

### Sharing & deep links

* On finish, scores are encoded into the URL:

  * `?es=<ES_POINTS>&ht=<HT_POINTS>&fi=<FI_POINTS>`
* Visiting a URL with these params **skips the quiz** and renders results directly.

---

## 🎨 Theming & Customization

### Colors & look

Tweak CSS variables in `:root`:

```css
:root {
  --bg: #0f1226;
  --card: #161a36;
  --ink: #e8ebff;
  --muted: #aab0e5;
  --accent: #7b8cff;
  --accent-2: #35e0c6;
  --radius: 18px;
  --shadow: 0 20px 50px rgba(0,0,0,.35);
}
```

### Editing questions

Add, remove, or modify items in the `questions` array. If you change counts:

* Update the **max per worldview** (currently `8 * 3`) in `showResults`.
* Consider balancing the number of questions per category.

Example (adding a new ES statement):

```js
questions.push({ id: 25, category: 'ES', text: 'Measurement advances should inform epistemology.' });
```

### Adjusting the scale

Change the `CHOICES` array (labels or values). Values must remain numeric; the app sums them.

```js
const CHOICES = [
  { key: 'sd', label: 'Strongly disagree', value: 0 },
  { key: 'd',  label: 'Disagree',          value: 1 },
  { key: 'a',  label: 'Agree',             value: 2 },
  { key: 'sa', label: 'Strongly agree',    value: 3 },
];
```

### Copy link button text

The Clipboard handler updates the button label to **“Copied!”** briefly; tweak the timeout in `copyResultLink()` if desired.

---

## ♿ Accessibility Notes

* Question text and progress use `aria-live="polite"` and `aria-atomic="true"` to announce updates.
* Contrast‑friendly palette and large hit‑targets for options.
* Options are native radio inputs inside labels, enabling click or keyboard activation.
* Progress, meters, and decorative elements avoid essential info being visual‑only.

> Always validate with your preferred a11y tooling for your target audience.

---

## 🌐 Browser Support

Modern evergreen browsers (Chromium, Firefox, Safari). The code uses:

* ES2015+ features (`const`, `let`, arrow functions)
* `URLSearchParams`, `history.replaceState`, `scrollIntoView`, and the Clipboard API

If you need IE/legacy support, transpile or polyfill the features above.

---

## 📦 Deploying

Because this is a static single‑file app, you can host it on any static host:

* GitHub Pages, Netlify, Vercel, Firebase Hosting, S3/CloudFront, etc.

No special routing is required; query parameters are read client‑side.
---

## 🛡️ Privacy

* 100% client‑side; no analytics or network calls.
* Results persist **only** in the page URL (when you finish) and your clipboard if you copy the link.

---

## 🤝 Contributing

Issues and PRs are welcome. Please keep changes dependency‑free and mindful of accessibility.

* Add unit tests if you introduce complex logic.
* Try to preserve the single‑file experience when possible.

---

## 📜 License

Public domain (CC0), just like the blog post itself.
