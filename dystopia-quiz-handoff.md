# CURSOR HANDOFF SPEC
## "Which Dystopia Are You Living In?" — Interactive Quiz App
### Version: Aligned with shipped `index.html` (`bananasutra/which-dystopia`) | Author: Bananasutra | Live: [bananasutra.github.io/which-dystopia](https://bananasutra.github.io/which-dystopia/)

---

## CURSOR OPENING PROMPT

Paste this as your very first Cursor message:

> *"Build a self-contained single HTML file quiz app called 'Which Dystopia Are You Living In?' — full spec attached. Aesthetic: minimal typewriter retro-futuristic. Think classified government terminal — 1984 meets cold war bureaucracy meets literary zine. Special Elite + Courier Prime fonts. Near-black background, off-white typewriter text, CRT scanline overlay. Use character-by-character typewriter *animation* on short lines (landing title, question lines, result headlines); use smooth fade-in on long body copy for a cinematic read. No emoji — typewriters didn't have them. Sharp borders, no rounded corners, no gradients. Everything mechanical and deliberate. Start with the HTML skeleton, CSS variables, and landing screen. We'll build screen by screen."*

**Recommended build order:**
1. HTML skeleton + CSS variables + fonts
2. Landing screen
3. Quiz question component + typewriter reveal
4. Progress dots
5. Scoring logic
6. Result screen (two-stage reveal)
7. Share mechanic (URL hash + clipboard + tweet)
8. Bananasutra easter egg
9. Polish pass (transitions, mobile, texture)

---

## 1. CONCEPT OVERVIEW

A literary personality quiz that assigns users a **dual-archetype result**: the dystopian *world* they inhabit + their *resistance style*. Results are shareable via URL — shared links land directly on the result screen with a prompt to take the quiz. A secret 6th result unlocks when the **scores refuse a clean winner**: either a **tie** at the top for that dimension, or a **spread** profile (no archetype crosses the dominance threshold — see §5).

**Tone:** Dark wit. Literary. Politically urgent. Never bleak without an exit.
**Audience:** Substack readers — philosophically literate, politically awake, culturally sharp.
**Platform:** GitHub Pages, single HTML file, no backend.

---

## 2. FILE STRUCTURE

```
which-dystopia/           ← GitHub repo name
├── index.html            ← entire app, fully self-contained
├── README.md             ← GitHub Pages deploy instructions
└── dystopia-quiz-handoff.md   ← this spec (optional in repo)
```

- **Vanilla JS only** — no frameworks, no build step, no npm
- All CSS in `<style>` block, all JS in `<script>` block
- Google Fonts loaded via `<link>` in `<head>`
- Deployable as-is to `https://bananasutra.github.io/which-dystopia/`

---

## 3. ARCHETYPES — MASTER REFERENCE

| ID | Author | Register | Accent Color |
|---|---|---|---|
| `orwell` | George Orwell | Weaponized truth, surveillance state | `#5A7A99` (cold steel blue, desaturated) |
| `huxley` | Aldous Huxley | Pleasure sedation, algorithmic comfort | `#B08830` (warm amber, desaturated) |
| `bradbury` | Ray Bradbury | Ambient apathy, memory erosion | `#B85522` (ember orange, desaturated) |
| `kafka` | Franz Kafka | Bureaucratic absurdity, systemic disorientation | `#6B6545` (desaturated olive) |
| `atwood` | Margaret Atwood | Bodily control, compliance as doctrine | `#7A3348` (deep burgundy, desaturated) |
| `banana` | Bananasutra | SECRET — unlocks on **tie** or **spread** (§5) | `#FFE600` (electric yellow — full strength) |

All archetype colors should feel like **faded classification stamps**, not UI buttons. Desaturated except Banana which gets full electric yellow.

**World headline spelling:** Use standard literary demonyms — all-caps titles **ORWELLIAN**, **HUXLEYAN**, **BRADBURIAN**, **KAFKAESQUE**, **ATWOODIAN**. For Bradbury, the demonym is *Bradburian*; the all-caps headline **BRADBURIAN REALITY** keeps the “Bradbury” shape readable (more intuitive than dropping a **B**).

---

## 4. QUESTIONS & ANSWER MAPPING

### DIMENSION 1 — WORLD
**Questions: Q1, Q3, Q6, Q7, Q8, Q9** (6 questions, max score per archetype = 6)
*These questions probe: what external reality the user perceives.*

---

**Q1. What worries you most today?**
- Lies disguised as truth → `orwell`
- Endless distraction → `huxley`
- Growing apathy → `bradbury`
- Absurd, senseless bureaucracy → `kafka`
- Bodies controlled by doctrine → `atwood`

**Q3. What's more dangerous?**
- Mass surveillance → `orwell`
- Mass sedation → `huxley`
- Mass ignorance → `bradbury`
- Mass absurdity → `kafka`
- Mass compliance dressed as morality → `atwood`

**Q6. What is truth?**
- Constantly under attack → `orwell`
- Whatever feels good now → `huxley`
- An endangered species → `bradbury`
- A running joke → `kafka`
- Whatever they need it to be today → `atwood`

**Q7. What's your biggest enemy?**
- Authoritarian rulers → `orwell`
- Boredom and sedation → `huxley`
- Forgetfulness and apathy → `bradbury`
- Paperwork from hell → `kafka`
- Enforced silence and performed smiles → `atwood`

**Q8. When you hear "freedom," you think:**
- A risk worth dying for → `orwell`
- A comfy beach chair → `huxley`
- A banned book in a fire → `bradbury`
- A hallway with no doors → `kafka`
- A word they redefined when I wasn't looking → `atwood`

**Q9. You wake up tomorrow and reality feels different. What's happening?**
- New lies overnight → `orwell`
- New pleasures sold overnight → `huxley`
- Everyone forgot to care overnight → `bradbury`
- New rules that contradict yesterday's rules → `kafka`
- New rules about my body overnight → `atwood`

---

### DIMENSION 2 — RESISTANCE / COPING
**Questions: Q2, Q4, Q5, Q10** (4 questions, max score per archetype = 4)
*These questions probe: how the user internally responds and copes.*

---

**Q2. You realize you made a mistake at work. What's your first reaction?**
- Fear of punishment → `orwell`
- Shrug, scroll memes → `huxley`
- Meh, mistakes happen → `bradbury`
- Dread the paperwork → `kafka`
- Smile and pretend it never happened → `atwood`

**Q4. Your coping strategy?**
- Resist and question everything → `orwell`
- Escape into fun, friends, treats → `huxley`
- Read secret books and daydream → `bradbury`
- Write weird poetry nobody reads → `kafka`
- Tell your story in secret, in fragments → `atwood`

**Q5. You find a forbidden book. What do you do?**
- Memorize it before they erase it → `orwell`
- Forget it, there's a new Netflix drop → `huxley`
- Hide it but forget where you put it → `bradbury`
- Assume it's a bureaucratic trap → `kafka`
- Read it in the dark, then pass it on → `atwood`

**Q10. Deep down, what gives you hope?**
- Truth and courage → `orwell`
- Beauty and love → `huxley`
- Imagination and memory → `bradbury`
- Humor, dreams, and tiny rebellions → `kafka`
- Solidarity, sisterhood, and the long memory of women → `atwood`

---

## 5. SCORING LOGIC (full implementation)

**Rules (evaluate in order per dimension):**

1. **Tie at top** — If two or more archetypes share the highest score → that dimension is **`banana`** (refuses to classify).
2. **Spread profile** — Else if world max score is **≤ 2** (out of 6) → `worldResult = 'banana'`. Else if coping max score is **≤ 1** (out of 4) → `copingResult = 'banana'`.
3. **Clear winner** — Else the single archetype with the max score wins that dimension.

```javascript
// Initialize score objects (only the five literary archetypes accrue points)
const worldScores  = { orwell: 0, huxley: 0, bradbury: 0, kafka: 0, atwood: 0 }
const copingScores = { orwell: 0, huxley: 0, bradbury: 0, kafka: 0, atwood: 0 }

const WORLD_QS  = [1, 3, 6, 7, 8, 9]  // Q index (1-based)
const COPING_QS = [2, 4, 5, 10]

function recordAnswer(questionIndex, archetype) {
  if (WORLD_QS.includes(questionIndex)) {
    worldScores[archetype]++
  } else if (COPING_QS.includes(questionIndex)) {
    copingScores[archetype]++
  }
  updateProgressDots()  // position only — see §9 Progress Dots
}

function updateProgressDots() {
  // Re-render bottom indicators from current question index (no archetype coloring)
}

function resolveDimension(scores, { spreadMax }) {
  const max = Math.max(...Object.values(scores))
  const winners = Object.keys(scores).filter(k => scores[k] === max)
  if (winners.length > 1) return 'banana'
  if (max <= spreadMax) return 'banana'
  return winners[0]
}

function getResults() {
  return {
    worldResult:  resolveDimension(worldScores,  { spreadMax: 2 }),
    copingResult: resolveDimension(copingScores, { spreadMax: 1 }),
  }
}
```

**Note:** `spreadMax` applies only when there is exactly one top scorer (tie check runs first).

**Threshold tuning:** The numeric spread cutoffs (`world` 2, `coping` 1) are candidates for playtesting — adjust the **values** only; keep this **evaluation order** (tie → spread → clear winner) and the same code shape.

---

## 6. APP SCREENS & FLOW

```
URL with no hash:    LANDING → QUIZ (Q1–Q10) → RESULT
URL with hash:       RESULT (direct) → only [ TAKE THE QUIZ YOURSELF → ] (large CTA; no copy/tweet/retake)
```

### 6a. LANDING SCREEN

**Content:**
```
[title]    WHICH DYSTOPIA
           ARE YOU LIVING IN?

[subtitle] 10 questions. 5 literary nightmares.
           One uncomfortable mirror.

[button]   [ BEGIN TRANSMISSION ]
```

- Full viewport, vertically centered
- Title uses Special Elite, large, typewriter-reveals on load
- Subtitle fades in after title finishes
- Button appears last, blinks once to draw attention
- If the URL fragment is `#world=X&coping=Y` (query-style params after `#`, not `location.search`) → skip landing entirely, render Result screen directly

### 6b. QUIZ SCREEN (Q1–Q10)

**Layout:**
- Full viewport per question
- Question text: top-center, typewriter reveal (~30ms per character)
- 5 answer option cards: appear staggered after question finishes (~50ms delay each), fade in from slight offset
- Progress dots: bottom-center, always visible
- No back button, no skip

**Answer card interaction:**
- Hover / selected: border and text use **that option’s** archetype accent (from `data-archetype`), not a global “leading score” color
- Click: card fills with that archetype accent at 15% opacity, border goes solid accent color
- 400ms pause after click (let selection register visually), then transition to next question
- Transition: quick flash to `#0A0A08` black, then new question types in — like a screen refresh/terminal clear

**Answer card markup pattern:**
```html
<div class="answer-card" data-archetype="orwell">
  Lies disguised as truth
</div>
```

### 6c. RESULT SCREEN

**Two-stage reveal:**
1. **World archetype block** — typewriter-reveals first (slower, ~20ms/char for gravitas)
2. **Resistance/coping block** — fades in 800ms after world block completes

**Layout structure:**
```
[ CLASSIFICATION REPORT ]          ← small label, all caps, dim

YOU ARE LIVING IN AN
[WORLD ARCHETYPE NAME] REALITY     ← large, Special Elite

[world description — 3 sentences]

────────────────────────────────   ← thin rule separator

YOUR RESISTANCE PROFILE: [COPING ARCHETYPE NAME]

[resistance style — 2 sentences]

**YOUR MOVE:** [call to action]

**READ THIS NEXT:** [book rec]

────────────────────────────────

[ + SEE THE FOOL DYSTOPIAN SYSTEM ]   ← `<details>` / reference table (Bananasutra row when applicable)

────────────────────────────────

**After quiz result (not a shared landing):** a dim hint line, then share row, then retake:

Copy your transmission link, then paste it wherever you broadcast the signal.

[ COPY YOUR TRANSMISSION LINK ]   [ POST TO THE BIRD SITE ]   ← equal width (~50/50), compact type

[ RUN AGAIN ]   ← full width on the next row

- Clipboard button briefly shows `[ COPIED — PASTE TO TRANSMIT ]` (~2.2s), then reverts.
- Tweet intent label: `[ POST TO THE BIRD SITE ]` (opens X/Twitter intent in a new tab).

**Shared URL (`#world=X&coping=Y`, `fromShare: true`):** only a large primary  
`[ TAKE THE QUIZ YOURSELF → ]` — no copy, bird-site, or run-again controls.

**Special case — same archetype for both dimensions:**
Replace two-part layout with unified single block:
```
YOU ARE FULLY, COMPLETELY,
UNAPOLOGETICALLY [ARCHETYPE].
[unified description — 4 sentences]
**YOUR MOVE:** [call to action]
**READ THIS NEXT:** [book rec]
```

**Result screen background:** shifts to active world archetype's accent color at 10% opacity.

---

## 7. ALL RESULT COPY — COMPLETE

### WORLD ARCHETYPES

---

**`orwell` — ORWELLIAN REALITY**
*Subtitle: Big Brother saw you hesitate before answering.*

You live in a world where truth is the first casualty — then language, then memory. Power doesn't just lie; it makes you doubt your own version of events. You notice the spin before the sentence is finished — that's not paranoia, that's pattern recognition in a broken system. Your superpower is refusing to call obvious lies "complicated." Staying lucid takes energy most people are saving for the next outrage cycle. Keep questioning anyway. Reality depends on rebels like you.

**READ THIS NEXT:** *Nineteen Eighty-Four — George Orwell*

---

**`huxley` — HUXLEYAN REALITY**
*Subtitle: Have you tried the new limited-edition dopamine drop?*

You live in a sweetened trap with excellent UX. Nobody burned your books — they just gave you infinite content until reading felt exhausting. The cage is upholstered in pleasure: targeted ads, viral joy, algorithmic comfort food served at exactly the right temperature. You sense something missing, but the notification arrives right on time. Boredom is not the enemy. Boredom is the door.

**READ THIS NEXT:** *Brave New World — Aldous Huxley*

---

**`bradbury` — BRADBURIAN REALITY**
*Subtitle: The fire is inside you. Guard it.*

You live in a world where forgetting is the real crime — and nobody's forcing it. Apathy does the burning now, one distracted scroll at a time. You feel the loss of things people stopped caring about before they were gone: nuance, memory, the long story. You still carry a flame. That flame is imagination — rarer and more dangerous than any banned book. Hold a conversation with someone you disagree with. Read something slow. Remember that caring is a radical act.

**READ THIS NEXT:** *Fahrenheit 451 — Ray Bradbury*

---

**`kafka` — KAFKAESQUE REALITY**
*Subtitle: Your appeal form was lost. Please resubmit in triplicate.*

You are fluent in absurdity's mother tongue. The system isn't cruel exactly — it's indifferent in ways that feel personal. Rules contradict rules. Doors open onto hallways. Guilt arrives before the accusation. And yet: you're still here, still dreaming, writing weird poetry that terrifies the paper-pushers precisely because it doesn't follow procedure. Don't let them convince you the confusion is your fault.

**READ THIS NEXT:** *The Trial — Franz Kafka*

---

**`atwood` — ATWOODIAN REALITY**
*Subtitle: They didn't take your voice. They taught you to silence it yourself.*

You live in a world where control arrives wrapped in concern — for your safety, your health, your own good. Language is tender and totalitarian at once. Your body is a policy. Your choices are managed by systems that smile while they constrain. But you notice. You remember what words meant before they were repurposed. You pass the truth on in fragments, in whispers, in the margins. That is not weakness. That is resistance with a long memory.

**READ THIS NEXT:** *The Handmaid's Tale — Margaret Atwood*

---

**`banana` — BANANASUTRA REALITY** *(SECRET)*
*Subtitle: You refused to be sorted. Suspicious. Impressive. Very on-brand.*

You don't fit neatly inside any single nightmare — which means you've been paying attention to all of them simultaneously. You are fluent in multiple dystopias, which is either a superpower or a therapy bill. Probably both. The system cannot classify you and that, my slippery contrarian, is the most subversive thing you could possibly be.

**READ THIS NEXT:** *All of the above. Start anywhere. Keep going.*

---

### RESISTANCE STYLES (COPING DIMENSION)

---

**`orwell` coping — THE TRUTH-TELLER**
You resist by naming things accurately. When language gets slippery, you reach for precision like a weapon. You document, you challenge, you refuse euphemism — not to be difficult, but because reality depends on someone saying the true word.

**YOUR MOVE:** Say the true word out loud, in public, without softening it.

---

**`huxley` coping — THE PLEASURE REFUSER**
You resist by choosing what you consume. You know the trap is sweet — and sometimes you eat the cookie anyway — but you notice. That noticing is everything. The radical act is choosing where your attention goes.

**YOUR MOVE:** Put the phone down for one hour and sit with what surfaces.

---

**`bradbury` coping — THE MEMORY KEEPER**
You resist by remembering on everyone's behalf. You hold stories, names, feelings, context that the algorithm never indexed and the news cycle never preserved. You are the long game.

**YOUR MOVE:** Tell someone a story they've never heard. Slowly. Without a point.

---

**`kafka` coping — THE ABSURDIST**
You resist by refusing to take the chaos personally. You find the dark joke in the impossible form, the poetry in the bureaucratic wall. That's not avoidance — that's philosophical judo, and it is rarer than it looks.

**YOUR MOVE:** Write the absurdity down. Make it funny. Send it to one person.

---

**`atwood` coping — THE WITNESS**
You resist by carrying the record. You pass the truth on in fragments, in whispers, in margins. You understand that memory is political and testimony is survival — and that the archive doesn't maintain itself.

**YOUR MOVE:** Write something true that you've been keeping quiet. Even if only for yourself.

---

**`banana` coping — THE TRICKSTER**
You resist by refusing the frame entirely. You are not a coper — you are a category-buster. Your tool is absurdist joy deployed with surgical precision, at exactly the moment the system least expects it.

**YOUR MOVE:** Do the unexpected, generous, ridiculous thing. Today.

---

### SAME-ARCHETYPE UNIFIED RESULTS
*(When world = coping, use these instead of the two-part layout)*

**`orwell` unified:**
You are fully, completely, unapologetically Orwellian. You live in a world of weaponized truth and you fight back the only way that works: by naming things accurately, out loud, without flinching. Your alertness is exhausting and essential. The spin cycle cannot catch you because you catalogued every rotation. Keep the record. Reality depends on rebels like you.

**YOUR MOVE:** Say the true word out loud, in public, without softening it.
**READ THIS NEXT:** *Nineteen Eighty-Four — George Orwell*

**`huxley` unified:**
You are fully, completely, unapologetically Huxleyan. You live inside the sweetened trap and you know it — which puts you one crucial step ahead of everyone blissfully scrolling beside you. Your resistance isn't rage, it's attention. Choosing where yours goes, deliberately and repeatedly, is the most rebellious thing you can do in a world engineered to steal it.

**YOUR MOVE:** Put the phone down for one hour and sit with what surfaces.
**READ THIS NEXT:** *Brave New World — Aldous Huxley*

**`bradbury` unified:**
You are fully, completely, unapologetically Bradburian. You feel the slow burn of a world that stopped caring, and you carry the flame everyone else let go out. Memory is your superpower. Imagination is your act of defiance. You are the long game in a world obsessed with the next five seconds.

**YOUR MOVE:** Tell someone a story they've never heard. Slowly. Without a point.
**READ THIS NEXT:** *Fahrenheit 451 — Ray Bradbury*

**`kafka` unified:**
You are fully, completely, unapologetically Kafkaesque. You live in the absurdity and you refuse to be destroyed by it — which is itself a profound act of resistance. The system cannot break what refuses to be surprised by its own nonsense. Your dark humor is not a coping mechanism. It is a philosophy.

**YOUR MOVE:** Write the absurdity down. Make it funny. Send it to one person.
**READ THIS NEXT:** *The Trial — Franz Kafka*

**`atwood` unified:**
You are fully, completely, unapologetically Atwoodian. You inhabit a world of softened controls and you resist through the oldest tool available: the faithful, stubborn record. You remember what they renamed. You keep the archive others abandoned. Every fragment you preserve is an act of survival — for you, and for whoever comes after.

**YOUR MOVE:** Write something true that you've been keeping quiet. Even if only for yourself.
**READ THIS NEXT:** *The Handmaid's Tale — Margaret Atwood*

**`banana` unified:**
You are fully, completely, unapologetically Bananasutra. You slipped through every classification net, refused every archetype, and somehow turned chaos into a creative act. You are not ungovernable by accident — you are ungovernable by philosophy. That is simultaneously the most dangerous and most hopeful thing a person can be right now.

**YOUR MOVE:** Do the unexpected, generous, ridiculous thing. Today.
**READ THIS NEXT:** *All of the above. Start anywhere. Keep going.*

---

## 8. SHARE MECHANIC

### URL Hash Encoding
```javascript
// After calculating results, encode to URL hash
const shareURL = `${window.location.origin}${window.location.pathname}#world=${worldResult}&coping=${copingResult}`

// Example: https://bananasutra.github.io/which-dystopia/#world=atwood&coping=kafka

// On load + on hashchange: parse fragment; if valid world+coping, skip landing
function tryHash() {
  const hash = window.location.hash.slice(1)
  if (!hash) return false
  const params = new URLSearchParams(hash)
  const world = params.get('world')
  const coping = params.get('coping')
  if (world && coping && isValidArchetype(world) && isValidArchetype(coping)) {
    showResult(world, coping, { fromShare: true })
    return true
  }
  const legacy = params.get('archetype')
  if (legacy && isValidArchetype(legacy)) {
    showResult(legacy, legacy, { fromShare: true })
    return true
  }
  return false
}
```

### Share copy strings (X intent)

Use a single **`SHARE_LABEL`** map (one literary adjective per archetype for each of the six IDs in `VALID_ARCHETYPES`). World and coping slots in the tweet both pull from this map. **Do not** invent adjectives in code.

**Tweet body template (implemented):**

```
My dystopia: [WORLD_ADJ] world, [COPING_ADJ] resistance. Which are you?
```

Both `[WORLD_ADJ]` and `[COPING_ADJ]` come from `SHARE_LABEL[id]` (e.g. Orwellian, Huxleyan, …). This is shorter than spelling out resistance *names* (THE TRUTH-TELLER, etc.). The URL is passed separately: `https://twitter.com/intent/tweet?text=...&url=...` (both parameters URL-encoded).

### Share button behavior (normal result only)

**Row 1 — two actions, equal width (~50/50), compact uppercase:**

1. **`[ COPY YOUR TRANSMISSION LINK ]`** — copies `shareURL` to clipboard (transmission-themed, echoes `[ BEGIN TRANSMISSION ]`).
   - Prefer `navigator.clipboard.writeText()` on **HTTPS**. If unavailable or rejected, fall back to a temporary `<textarea>` + `document.execCommand('copy')`, or `prompt('Copy this URL:', text)` on failure.
   - Button briefly shows **`[ COPIED — PASTE TO TRANSMIT ]`** (~2.2s), then reverts to the copy label.

2. **`[ POST TO THE BIRD SITE ]`** — opens the X/Twitter intent in a new tab (`rel="noopener noreferrer"`).

**Row 2:** **`[ RUN AGAIN ]`** — full width; clears hash and returns to landing.

A dim hint line above row 1 explains copy-then-paste (see §6c).

### Shared URL landing (`fromShare: true`)

When the hash is a valid `world` + `coping` pair:

- Skip landing; call `showResult(world, coping, { fromShare: true })`.
- Result body renders immediately (no typewriter on headlines).
- **Actions:** only **`[ TAKE THE QUIZ YOURSELF → ]`** — large, primary, links to `pathname` with no hash (landing). No copy, bird-site, or run-again buttons.

---

## 9. VISUAL DESIGN SYSTEM

### Aesthetic Direction
**Minimal Typewriter Retro-Futuristic.**
Think: classified government document from a dystopian future. Cold war terminal. Literary zine photocopied at 2am. The interface feels like opening a file on yourself — and finding it was always there.

### Color Palette
```css
:root {
  /* Base */
  --bg:           #0A0A08;   /* near-black, warm undertone — aged paper in the dark */
  --text:         #E8E4D9;   /* off-white — old typewriter ink on cream */
  --text-dim:     rgba(232, 228, 217, 0.35);  /* for labels, dots unfilled */
  --text-mid:     rgba(232, 228, 217, 0.65);  /* for secondary text */
  --border:       rgba(232, 228, 217, 0.15);  /* default card borders */
  --border-hover: rgba(232, 228, 217, 0.5);   /* hover state */

  /* Archetype accent colors — desaturated, faded stamp feel */
  --orwell:       #5A7A99;
  --huxley:       #B08830;
  --bradbury:     #B85522;
  --kafka:        #6B6545;
  --atwood:       #7A3348;
  --banana:       #FFE600;   /* full strength — this one earns it */

  /* World accent on result screen — set dynamically via JS from final world archetype */
  --active:       var(--orwell);  /* default; quiz UI uses per-card --accent, not this */
}
```

### Typography
Load fonts with `<link rel="preconnect">` + `<link href="...fonts.googleapis.com...">` in `<head>` (same URLs as §13). **Do not** use `@import` in CSS — it blocks rendering and duplicates loading if `<link>` is already present.

```css
/* Usage */
.question-text     { font-family: 'Special Elite', cursive; }
.answer-option     { font-family: 'Courier Prime', monospace; }
.result-headline   { font-family: 'Special Elite', cursive; }
.result-body       { font-family: 'Courier Prime', monospace; letter-spacing: 0.02em; }
.label             { font-family: 'Courier Prime', monospace; text-transform: uppercase; letter-spacing: 0.15em; font-size: 0.7em; }
```

### Texture & Atmosphere
```css
/* CRT scanlines — full-page overlay */
body::after {
  content: '';
  position: fixed;
  inset: 0;
  background: repeating-linear-gradient(
    0deg,
    transparent,
    transparent 2px,
    rgba(0, 0, 0, 0.03) 2px,
    rgba(0, 0, 0, 0.03) 4px
  );
  pointer-events: none;
  z-index: 9999;
}

/* Paper grain — SVG noise filter */
/* Apply feTurbulence SVG filter as background or use CSS noise overlay */
/* Grain should be very subtle — 4-6% opacity max */

/* Result screen background tint — set dynamically */
.result-screen {
  background-color: color-mix(in srgb, var(--active) 10%, var(--bg) 90%);
  transition: background-color 0.8s ease;
}
/* Fallback when color-mix() unsupported: set a solid near-bg tint in JS, or omit mix */
```

### Accessibility (classified terminal, minimal)

- **Focus:** visible `:focus-visible` outlines on buttons and answer cards (sharp rectangle, no radius — e.g. `outline: 1px solid var(--text-mid)`).
- **Live regions:** `aria-live="polite"` on the question container when the typewriter finishes (or `role="status"` for the current question line).
- **Reduced motion:** `@media (prefers-reduced-motion: reduce)` — shorten or skip typewriter interval (show full text), skip glitch/static flashes on Banana, use instant or opacity-only transitions.
- **Contrast:** keep body text on `--bg` above WCAG AA where possible; archetype accents are decorative, not sole signal of meaning.

### Layout & Spacing
- Max content width: `680px` (quiz) / `720px` (result), centered
- Base font size: `16px` mobile, `18px` desktop
- Generous vertical padding — questions need breathing room
- **No rounded corners anywhere** — `border-radius: 0` globally
- **No box shadows** — use `border` only
- Thin horizontal rules: `border-top: 1px solid var(--border)` — used as section dividers

### Answer Cards
```css
.answer-card {
  border: 1px solid var(--border);
  padding: 1rem 1.25rem;
  cursor: pointer;
  transition: border-color 0.15s, color 0.15s, background-color 0.15s;
  font-family: 'Courier Prime', monospace;
}

/* Per-card accent — set via data-archetype in JS or attribute selectors, e.g.: */
.answer-card[data-archetype="orwell"] { --accent: var(--orwell); }
/* …repeat for huxley, bradbury, kafka, atwood */

.answer-card:hover {
  border-color: var(--accent, var(--border-hover));
  color: var(--accent, var(--text));
}

.answer-card.selected {
  border-color: var(--accent, var(--border-hover));
  background-color: color-mix(in srgb, var(--accent, var(--text)) 15%, transparent 85%);
  color: var(--accent, var(--text));
}
```

**Mobile layout:** Answer cards stacked full-width, single column
**Desktop (≥768px):** Answer cards in 2-column CSS grid (5th card spans full width or centers)

### Progress Dots
Replace standard circles with typographic characters — **progress only** (how far through the quiz), not a live “leading archetype” indicator:
```
· · · · · · · · · ·    ← all upcoming (dim)
▪ ▪ ▪ □ · · · · · ·    ← answered (▪ filled), active (□ current step), upcoming (·)
```
- `·` = unanswered / upcoming — `color: var(--text-dim)`
- `▪` = answered — `color: var(--text-mid)`
- `□` = current question — `color: var(--text)` or `var(--border-hover)` (neutral; **not** archetype-colored)
- Spaced with `letter-spacing: 0.4em`, centered bottom of screen
- Position: `fixed` bottom, `padding-bottom: 2rem`

### Buttons
```css
.btn {
  font-family: 'Courier Prime', monospace;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  border: 1px solid var(--border);
  background: transparent;
  color: var(--text);
  padding: 0.75rem 1.5rem;
  cursor: pointer;
  transition: border-color 0.15s, color 0.15s;
}

.btn:hover {
  border-color: var(--active);
  color: var(--active);
}

.btn-primary {
  border-color: var(--text-mid);
}
```

Labels use bracket convention: `[ BEGIN TRANSMISSION ]`, `[ COPY YOUR TRANSMISSION LINK ]`, `[ POST TO THE BIRD SITE ]`, `[ RUN AGAIN ]`, `[ TAKE THE QUIZ YOURSELF → ]`

---

## 10. ANIMATION & MOTION SYSTEM

### Typewriter Effect
```javascript
function typewriter(element, text, speed = 30, callback) {
  element.textContent = ''
  let i = 0
  const cursor = document.createElement('span')
  cursor.className = 'cursor'
  cursor.textContent = '|'
  element.appendChild(cursor)

  const interval = setInterval(() => {
    if (i < text.length) {
      element.insertBefore(document.createTextNode(text[i]), cursor)
      i++
    } else {
      clearInterval(interval)
      cursor.remove()
      if (callback) callback()
    }
  }, speed)
}

// Usage speeds:
// Landing title:     40ms/char (slow, deliberate)
// Questions:         28ms/char (readable pace)
// Result headline:   20ms/char (slower = gravitas)
// Result body:       fade-in (cinematic); typewriter *fonts* everywhere, not typewriter animation on long paragraphs
// prefers-reduced-motion: see §9 Accessibility — shorten or skip intervals
```

```css
/* Blinking cursor — neutral during quiz; avoid tying to result --active */
.cursor {
  animation: blink 0.8s step-end infinite;
  color: var(--text-mid);
}
@keyframes blink {
  0%, 100% { opacity: 1; }
  50%       { opacity: 0; }
}
```

### Question Transition
```javascript
// Between questions:
// 1. Flash screen to black (80ms)
// 2. Render new question (hidden)
// 3. Fade/flash back in (80ms)
// 4. Begin typewriter reveal

function transitionToQuestion(nextIndex) {
  document.body.style.backgroundColor = '#000'
  setTimeout(() => {
    renderQuestion(nextIndex)
    document.body.style.backgroundColor = ''
    // begin typewriter
  }, 80)
}
```

### Answer Stagger
After typewriter finishes on question text, answer cards appear with staggered delay:
```css
.answer-card { opacity: 0; transform: translateY(4px); }
.answer-card.visible {
  animation: fadeUp 0.2s ease forwards;
}
@keyframes fadeUp {
  to { opacity: 1; transform: translateY(0); }
}
```
```javascript
// Stagger each card: 50ms * index delay
cards.forEach((card, i) => {
  setTimeout(() => card.classList.add('visible'), 50 * i)
})
```

### Result Two-Stage Reveal
```javascript
// Stage 1: World archetype block — typewriter headline, then fade body
// Stage 2: 800ms after stage 1 complete — coping block fades in
// Stage 3: 400ms after stage 2 — share/retake buttons appear
```

---

## 11. BANANASUTRA EASTER EGG

**Triggered when:** `worldResult === 'banana'` OR `copingResult === 'banana'` (either or both)

**Animation sequence on result reveal:**
1. Brief static/noise flash (CSS animation, 150ms) — like a bad transmission
2. Screen-wide color wash: `#FFE600` at 25% opacity, 200ms, fades out
3. Headline renders with glitch effect before resolving to final text

```css
/* Glitch on headline */
@keyframes glitch {
  0%   { opacity: 1;   transform: skewX(0deg);   letter-spacing: normal; }
  15%  { opacity: 0.7; transform: skewX(-4deg);  letter-spacing: 0.15em; }
  30%  { opacity: 1;   transform: skewX(3deg);   letter-spacing: -0.05em; }
  50%  { opacity: 0.8; transform: skewX(-2deg);  letter-spacing: 0.08em; }
  70%  { opacity: 1;   transform: skewX(1deg);   letter-spacing: normal; }
  100% { opacity: 1;   transform: skewX(0deg);   letter-spacing: normal; }
}

.glitch-text {
  animation: glitch 0.7s ease forwards;
}

/* Static noise flash */
@keyframes static-flash {
  0%   { opacity: 0; }
  30%  { opacity: 1; background-color: rgba(255,230,0,0.08); }
  60%  { opacity: 0.6; }
  100% { opacity: 0; }
}
```

**Text beat:** For 300ms before the final headline resolves, briefly show:
```
[ ARCHETYPE NOT FOUND ]
[ CLASSIFICATION ERROR ]
[ ... ]
BANANASUTRA REALITY
```
Each line typewriters in fast, previous line disappears — terminal-scanning feel.

---

## 12. RESPONSIVE LAYOUT

### Mobile (default, <768px)
- Single column throughout
- Answer cards: full width, stacked
- Question text: `font-size: clamp(1.2rem, 4vw, 1.6rem)`
- Result headline: `font-size: clamp(1.4rem, 5vw, 2rem)`
- Dots: bottom fixed, `font-size: 1.1rem`
- Share row: copy + bird-site stay **side by side** (50/50) with smaller padding/font so both fit; run-again stays **full width** below

### Desktop (≥768px)
- Answer cards: CSS grid, 2 columns
  - 5th card: `grid-column: 1 / -1` (spans full, centered via `justify-self: center`, `max-width: 50%`)
- Question text: `font-size: 1.5rem`
- Result headline: `font-size: 2.2rem`
- Share row: same 50/50 + full-width run-again pattern; typography may scale up slightly at wider breakpoints
- Content max-width enforced with `margin: 0 auto`

---

## 13. COMPLETE HTML SKELETON REFERENCE

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Which Dystopia Are You Living In?</title>
  <meta name="description" content="10 questions. 5 literary nightmares. One uncomfortable mirror.">
  
  <!-- Open Graph for sharing -->
  <meta property="og:title" content="Which Dystopia Are You Living In?">
  <meta property="og:description" content="10 questions. 5 literary nightmares. One uncomfortable mirror.">
  <meta property="og:url" content="https://bananasutra.github.io/which-dystopia/">
  <meta name="twitter:card" content="summary">

  <!-- Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Special+Elite&family=Courier+Prime:ital,wght@0,400;0,700;1,400&display=swap" rel="stylesheet">

  <style>
    /* === CSS VARIABLES === */
    /* === RESET & BASE === */
    /* === SCANLINE OVERLAY === */
    /* === LAYOUT === */
    /* === LANDING SCREEN === */
    /* === QUIZ SCREEN === */
    /* === ANSWER CARDS === */
    /* === PROGRESS DOTS === */
    /* === RESULT SCREEN === */
    /* === BUTTONS === */
    /* === ANIMATIONS === */
    /* === RESPONSIVE === */
    /* === BANANASUTRA GLITCH === */
  </style>
</head>
<body>

  <!-- SCREENS — show/hide via JS class toggling -->
  <div id="screen-landing" class="screen active"></div>
  <div id="screen-quiz"    class="screen"></div>
  <div id="screen-result"  class="screen"></div>

  <!-- PROGRESS DOTS — fixed, always visible during quiz -->
  <div id="progress-dots" class="hidden"></div>

  <script>
    // === DATA: QUESTIONS ===
    // === DATA: RESULT COPY ===
    // === STATE ===
    // === SCORING ===
    // === TYPEWRITER ===
    // === SCREEN NAVIGATION ===
    // === RENDER: LANDING ===
    // === RENDER: QUESTION ===
    // === RENDER: RESULT ===
    // === SHARE MECHANIC ===
    // === URL HASH CHECK (run on load) ===
    // === INIT ===
  </script>
</body>
</html>
```

---

## 14. DATA STRUCTURE REFERENCE

```javascript
// Questions array
const QUESTIONS = [
  {
    index: 1,
    dimension: 'world',
    text: "What worries you most today?",
    answers: [
      { text: "Lies disguised as truth",          archetype: "orwell"   },
      { text: "Endless distraction",              archetype: "huxley"   },
      { text: "Growing apathy",                   archetype: "bradbury" },
      { text: "Absurd, senseless bureaucracy",    archetype: "kafka"    },
      { text: "Bodies controlled by doctrine",    archetype: "atwood"   },
    ]
  },
  // ... Q2–Q10
]

// Results copy object
const RESULTS = {
  world: {
    orwell:   { title: "ORWELLIAN REALITY",    subtitle: "...", body: "...", book: "..." },
    huxley:   { title: "HUXLEYAN REALITY",     subtitle: "...", body: "...", book: "..." },
    bradbury: { title: "BRADBURIAN REALITY",   subtitle: "...", body: "...", book: "..." },
    kafka:    { title: "KAFKAESQUE REALITY",   subtitle: "...", body: "...", book: "..." },
    atwood:   { title: "ATWOODIAN REALITY",    subtitle: "...", body: "...", book: "..." },
    banana:   { title: "BANANASUTRA REALITY",  subtitle: "...", body: "...", book: "..." },
  },
  coping: {
    orwell:   { name: "THE TRUTH-TELLER",   body: "...", cta: "..." },
    huxley:   { name: "THE PLEASURE REFUSER", body: "...", cta: "..." },
    bradbury: { name: "THE MEMORY KEEPER",  body: "...", cta: "..." },
    kafka:    { name: "THE ABSURDIST",      body: "...", cta: "..." },
    atwood:   { name: "THE WITNESS",        body: "...", cta: "..." },
    banana:   { name: "THE TRICKSTER",      body: "...", cta: "..." },
  },
  unified: {
    orwell:   { body: "...", cta: "...", book: "..." },
    huxley:   { body: "...", cta: "...", book: "..." },
    bradbury: { body: "...", cta: "...", book: "..." },
    kafka:    { body: "...", cta: "...", book: "..." },
    atwood:   { body: "...", cta: "...", book: "..." },
    banana:   { body: "...", cta: "...", book: "..." },
  }
}

// Valid archetypes (for URL hash validation)
const VALID_ARCHETYPES = ['orwell', 'huxley', 'bradbury', 'kafka', 'atwood', 'banana']

// Tweet / share line — one adjective per archetype; used for BOTH world and coping slots in the intent text
const SHARE_LABEL = {
  orwell: 'Orwellian',
  huxley: 'Huxleyan',
  bradbury: 'Bradburian',
  kafka: 'Kafkaesque',
  atwood: 'Atwoodian',
  banana: 'Bananasutra',
}
// Template: `My dystopia: ${SHARE_LABEL[world]} world, ${SHARE_LABEL[coping]} resistance. Which are you?`
// Resistance *display names* (THE TRUTH-TELLER, etc.) live in result copy only — not in the tweet string.
```

---

## 15. DEPLOY INSTRUCTIONS (README.md content)

Canonical copy lives in repo **`bananasutra/which-dystopia`**. Summary:

```markdown
# Which Dystopia Are You Living In?

Live: https://bananasutra.github.io/which-dystopia/

## Deploy to GitHub Pages

1. Repo: `bananasutra/which-dystopia`
2. Push `index.html` and `README.md` to `main`
3. Settings → Pages → Deploy from branch → `main` → `/ (root)`
4. Site URL: `https://bananasutra.github.io/which-dystopia/`

## Share URL format

`https://bananasutra.github.io/which-dystopia/#world=atwood&coping=kafka`

Legacy `#archetype=…` is accepted for old bookmarks (maps to unified result).
```

---

## 16. EDGE CASES & GOTCHAS

| Scenario | Handling |
|---|---|
| User refreshes mid-quiz | Returns to landing screen (state is in-memory only, intentional) |
| Shared URL has invalid archetype value | Fall through to landing screen, ignore hash |
| Legacy hash `#archetype=X` (valid ID) | `showResult(X, X, { fromShare: true })` — unified layout |
| Two+ archetypes tied for max score on a dimension | That dimension = **banana** (§5) |
| Single dominant archetype but max score ≤ spread threshold (world ≤2, coping ≤1) | That dimension = **banana** |
| All world answers are same archetype (max score 6) | Clear winner, no tie; spread does not apply (max > 2) |
| World banana + coping banana | Full Bananasutra result — both dimensions show banana copy |
| World banana + coping normal | Banana world description + normal coping style |
| Same archetype both dimensions (and not banana-only edge) | Use unified result copy (Section 7), not two-part layout |
| Twitter/X intent URL too long | URL-encode `text` and `url`; tweet line uses short `SHARE_LABEL` strings |
| Mobile keyboard causing layout shift | All answer cards are divs, not inputs — no keyboard triggered |
| Clipboard API fails (e.g. non-HTTPS local test) | Fall back per §8 |

---

## 17. QUICK REFERENCE — LABELS & MICROCOPY

```
Landing title:        WHICH DYSTOPIA ARE YOU LIVING IN?
Landing subtitle:     10 questions. 5 literary nightmares. One uncomfortable mirror.
Start button:         [ BEGIN TRANSMISSION ]
Question label:       QUESTION [N] OF 10
Result label:         [ CLASSIFICATION REPORT ]
Resistance label:     YOUR RESISTANCE PROFILE: [COPING NAME]
CTA label:            YOUR MOVE:
Book label:           READ THIS NEXT:
Share hint (dim):     Copy your transmission link, then paste it wherever you broadcast the signal.
Share copy button:    [ COPY YOUR TRANSMISSION LINK ]
Share copy confirm:   [ COPIED — PASTE TO TRANSMIT ]
Tweet button:         [ POST TO THE BIRD SITE ]
Retake button:        [ RUN AGAIN ]
From-share link:      [ TAKE THE QUIZ YOURSELF → ]  (only control when `fromShare`)
Dots unfilled:        ·
Dots filled:          ▪
Dot active:           □  (neutral highlight — not archetype-colored)
Unified result open:  YOU ARE FULLY, COMPLETELY, UNAPOLOGETICALLY [ARCHETYPE].
Banana interstitial:  [ ARCHETYPE NOT FOUND ] → [ CLASSIFICATION ERROR ] → BANANASUTRA REALITY
```

---

*Spec aligned with shipped `index.html` in `bananasutra/which-dystopia` (share UI, URLs, tweet copy, `SHARE_LABEL`).*
*Build with Cursor. Deploy to GitHub Pages. Terrify the paper-pushers.*
