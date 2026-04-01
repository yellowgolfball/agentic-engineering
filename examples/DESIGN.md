# NodeProxy — Design System

> **For AI agents:** This document is a complete design specification for the NodeProxy marketing site. It is precise enough that an agent can reconstruct any UI component from scratch without seeing the original source. Read this before generating any HTML, CSS, or components for this project.

---

## 1. Visual Theme & Atmosphere

**Concept:** Dark sovereign technology. The site communicates hardware ownership, privacy, and infrastructure independence. The aesthetic is Ubuntu Linux meets deep-space—warm aubergine darkness punctuated by hot orange energy.

**Mood:** Confident, technical, warm. Not cold/corporate. Not playful/startup. Somewhere between a premium developer tool and a privacy manifesto.

**Personality:**
- The darkness is *deep aubergine*, not black. It carries warmth and history (Ubuntu's purple brand).
- The orange is Ubuntu's signature flame—used sparingly as energy and activation.
- Purple-lavender tones (aubergine.300, .400) appear as secondary accents—whispers of the Ubuntu palette.
- Warm off-white text (not pure white) keeps the reading experience comfortable.
- Subtle noise texture on `<body>` adds organic depth—renders the flat color as slightly physical.

**Key visual motifs:**
- **Radial node logo:** A central node with spokes radiating to outer nodes at cardinal and diagonal directions—represents connectivity, mesh networks, and decentralized architecture.
- **Animated morphing blobs:** Large, blurred background glows that slowly animate in the hero section.
- **Organic mesh/constellation backgrounds:** SVG diagrams use a neural-net particle aesthetic.
- **Terminal windows:** Black-background code blocks with macOS-style traffic light dots.
- **Architecture diagrams:** SVG-based flow diagrams with animated tunnel lines, glowing nodes, and ACL policy code.

---

## 2. Color Palette & Roles

### Core Background Scale (Aubergine)

| Token | Hex | Usage |
|---|---|---|
| `aubergine-950` | `#1a0a1e` | Terminal window background, deepest surfaces |
| `aubergine-900` | `#2C001E` | **Page body background** (also `bg-aubergine-900`) |
| `aubergine-850` | `#3B0A2E` | Card backgrounds, elevated surfaces (`bg-aubergine-850/50`) |
| `aubergine-800` | `#4A1442` | Hover state backgrounds, card headers (`bg-aubergine-800/60`) |
| `aubergine-700` | `#5E2750` | **Border color** (used as `border-aubergine-700/30` — the standard card border) |
| `aubergine-600` | `#77216F` | Decorative blob color, divider gradients, secondary accent |
| `aubergine-500` | `#8C3382` | Middle terminal dot (`bg-aubergine-500/40`) |
| `aubergine-400` | `#AE5CA5` | SVG secondary node dots, mid-depth accents |
| `aubergine-300` | `#D08EC8` | SVG label text, code block keywords, secondary text accent |

### Ubuntu Brand (Orange)

| Token | Hex | Usage |
|---|---|---|
| `ubuntu-orange` | `#E95420` | **Primary CTA buttons**, logo spokes, active indicators, accent lines |
| `ubuntu-orange-light` | `#F47B53` | Button gradient overlay on hover, logo center inner circle |
| `ubuntu-orange-dark` | `#C7420F` | Darker orange variant (available, used sparingly) |
| `ubuntu-orange-glow` | `rgba(233,84,32,0.15)` | Glow backgrounds behind key elements |

### Warm Neutral (Text Scale)

| Token | Hex | Usage |
|---|---|---|
| `warm-50` | `#FDF6F0` | Rarely used (near-white) |
| `warm-100` | `#FAF0E8` | Primary body text, SVG node labels |
| `warm-200` | `#F2E2D4` | High-emphasis body text, nav brand |
| `warm-300` | `#E8D0BB` | Standard body text (at `/60` opacity) |
| `warm-400` | `#D4B49A` | Muted/secondary text |
| `warm-500` | `#B8956F` | Dimmed secondary elements |
| `warm-600` | `#9A7752` | Least prominent text |

### Semantic Usage Patterns

```
Page background:      #2C001E  (bg-aubergine-900)
Terminal/deep bg:     #1a0a1e  (bg-aubergine-950)
Card surface:         rgba(59,10,46,0.5)  (bg-aubergine-850/50)
Card border:          rgba(94,39,80,0.3)  (border-aubergine-700/30)
Primary accent:       #E95420  (ubuntu-orange)
Heading text:         #FFFFFF  (white)
Body text high:       rgba(242,226,212,0.7)  (text-warm-200/70)
Body text medium:     rgba(232,208,187,0.6)  (text-warm-300/60)
Body text low:        rgba(232,208,187,0.5)  (text-warm-300/50)
Muted/label text:     rgba(212,180,154,0.4)  (text-warm-400/40) or /30
Eyebrow/tag text:     #E95420  (text-ubuntu-orange)
Code text:            rgba(242,226,212,0.8)  (text-warm-200/80)
Code prompt:          rgba(212,180,154,0.4)  (text-warm-400/40)
```

### Transparency Usage

Opacity modifiers are used heavily. Common values:
- `/90` — near-solid (nav background)
- `/70` — strong but slightly transparent (key labels)
- `/60` — standard body text
- `/50` — secondary body text
- `/40` — muted/disabled-feeling text
- `/30` — subtle borders and dividers
- `/20` — very subtle decorative borders
- `/10` — tint backgrounds (badge, accent washes)
- `/04`–`/06` — nearly invisible glow effects

---

## 3. Typography Rules

### Font Stack

```css
font-family: 'Ubuntu', system-ui, sans-serif;    /* --font-sans */
font-family: 'Ubuntu Mono', monospace;            /* --font-mono */
-webkit-font-smoothing: antialiased;
```

Load from Google Fonts:
```html
<link href="https://fonts.googleapis.com/css2?family=Ubuntu:ital,wght@0,300;0,400;0,500;0,700&family=Ubuntu+Mono:wght@400;700&display=swap" rel="stylesheet">
```

### Type Scale

| Role | Size | Weight | Tracking | Line-height | Color |
|---|---|---|---|---|---|
| Hero H1 | `clamp(2.4rem, 5vw, 4.2rem)` | 700 | `-0.035em` | `1.08` | `white` |
| Section H2 | `clamp(2rem, 4vw, 2.8rem)` | 700 | `-0.025em` | `1.12` | `white` |
| Section H2 light | Same as H2 | 300 | Same | Same | `text-warm-300/50` |
| Mid-section statement | `clamp(1.8rem, 4vw, 3rem)` | 700 | `-0.03em` | default | `white` |
| Nav brand | `17px` | 700 | `-0.01em` | default | `text-warm-100` |
| Nav links | `14px` | 500 | default | default | `text-warm-300/60` → `text-warm-100` |
| Eyebrow label | `12px` | 700 | `0.15em` | default | `text-ubuntu-orange` |
| Body large | `17px` | 400/300 | default | `1.75` | `text-warm-200/70` or `text-warm-300/60` |
| Body standard | `16px` | 400 | default | `1.8` | `text-warm-300/60` |
| Body small | `15px` | 400 | default | `relaxed` | `text-warm-300/50` |
| Card label | `13px` | 500 | default | default | `text-warm-200/70` |
| Card sublabel | `13px` | 400 | default | `1.7` | `text-warm-400/40` or `text-warm-300/50` |
| Feature list item | `14px` | 400 | default | default | `text-warm-300/50` (value: `text-warm-200/70`) |
| Terminal / code | `14px` | 400 | default | default | `text-warm-200/80` (Ubuntu Mono) |
| Code filename label | `12px` | 400 | default | default | `text-aubergine-400/50` (Ubuntu Mono) |
| Small muted | `13px` | 400 | default | default | `text-warm-400/30` or `text-warm-300/40` |
| Diagram zone label | `11px` | 700 | `2px` | default | `#FAF0E8` at `0.6` opacity |
| Pricing plan name | `13px` | 700 | `wider` | default | `text-warm-300/50` (or `text-ubuntu-orange` for Pro) |
| Price numeral | `text-3xl` (1.875rem) | 700 | default | default | `white` |
| Price unit `/mo` | `15px` | 400 | default | default | `text-warm-400/40` |

### Typography Patterns

**Section eyebrow pattern:**
```html
<p class="text-[12px] font-bold uppercase tracking-[0.15em] text-ubuntu-orange mb-3 inline-flex items-center gap-2">
  Label Text <svg ...icon.../>
</p>
```

**H1 with orange accent word:**
```html
<h1 class="font-bold text-[clamp(2.4rem,5vw,4.2rem)] leading-[1.08] tracking-[-0.035em] mb-6 text-white">
  Own your AI.<br>
  <span class="text-ubuntu-orange">Reach the world.</span>
</h1>
```

**H2 with light secondary line:**
```html
<h2 class="font-bold text-[clamp(2rem,4vw,2.8rem)] tracking-[-0.025em] leading-[1.12] mb-6 text-white">
  Your email. Your machine.<br>
  <span class="font-light text-warm-300/50">Nobody else reads it.</span>
</h2>
```

**Feature checklist:**
```html
<p class="text-warm-300/50 text-[14px]">
  <span class="text-ubuntu-orange/70">✓</span> 
  <span class="text-warm-200/70">Feature description here</span>
</p>
```

**Inline code mention:**
```html
<code class="text-ubuntu-orange/70 font-mono text-[14px]">/mcp-agent-api</code>
```

---

## 4. Component Stylings

### Navigation Bar

```css
/* Fixed top nav */
position: fixed; top: 0; left: 0; right: 0; z-index: 50;
padding: 1rem 1.5rem (lg: 2.5rem);
background: rgba(44,0,30,0.9); /* bg-aubergine-900/90 */
backdrop-filter: blur(24px);
border-bottom: 1px solid rgba(94,39,80,0.3);
box-shadow: 0 1px 0 rgba(233,84,32,0.06);
```

- Logo: `w-8 h-8` SVG + `font-bold text-[17px] tracking-[-0.01em]`, flex gap-3
- Nav links: `text-[14px] font-medium text-warm-300/60 hover:text-warm-100`, no-underline, `transition-colors`
- Vertical divider between links: `w-px h-4 bg-aubergine-700/40`
- CTA button: `text-[14px] font-medium px-5 py-2.5 bg-ubuntu-orange text-white rounded-full hover:bg-ubuntu-orange-light transition-colors`

### Primary CTA Button

```css
/* Orange pill button — hero/CTA */
display: inline-block; position: relative; overflow: hidden;
padding: 0.875rem 2rem; /* py-3.5 px-8 */
background: #E95420;
color: white;
border-radius: 9999px; /* rounded-full */
font-size: 15px; font-weight: 500;
transition: box-shadow 0.2s, transform 0.2s;

/* Hover */
transform: translateY(-2px); /* hover:-translate-y-0.5 */
box-shadow: 0 20px 60px rgba(233,84,32,0.2);

/* Gradient overlay (inside, on hover) */
::before {
  content: ''; position: absolute; inset: 0;
  background: linear-gradient(90deg, #F47B53, #E95420);
  opacity: 0; transition: opacity 0.2s;
}
:hover::before { opacity: 1; }
```

### Secondary Button (Ghost)

```css
padding: 0.75rem 1.5rem; /* py-3 px-6 */
border-radius: 9999px;
font-size: 14px; font-weight: 500;
color: rgba(232,208,187,0.7); /* text-warm-200/70 */
background: rgba(74,20,66,0.6); /* bg-aubergine-800/60 */
box-shadow: inset 0 0 0 1px rgba(94,39,80,0.3); /* ring-1 ring-aubergine-700/30 */
transition: background 0.2s, transform 0.2s;

:hover {
  background: rgba(94,39,80,0.6); /* bg-aubergine-700/60 */
  transform: translateY(-2px);
}
```

### Terminal Window

```html
<!-- Container -->
<div class="relative bg-aubergine-950 rounded-[16px] overflow-hidden ring-1 ring-aubergine-700/30">
  <!-- Glow halo (behind, slightly offset) -->
  <!-- <div class="absolute inset-0 translate-y-2 bg-ubuntu-orange/[0.04] rounded-[16px] blur-xl"></div> -->
  
  <!-- Title bar -->
  <div class="px-5 py-3 bg-aubergine-800/30 border-b border-aubergine-700/20 flex items-center justify-between">
    <!-- Traffic lights -->
    <div class="flex items-center gap-2">
      <span class="w-3 h-3 rounded-full bg-ubuntu-orange/50"></span>
      <span class="w-3 h-3 rounded-full bg-aubergine-500/40"></span>
      <span class="w-3 h-3 rounded-full bg-warm-400/30"></span>
    </div>
    <span class="font-mono text-[12px] text-aubergine-400/50">bash</span>
  </div>
  
  <!-- Code body -->
  <div class="px-6 py-5 font-mono text-[14px] flex items-center justify-between gap-4">
    <div>
      <span class="text-warm-400/40 select-none">$ </span>
      <span class="text-warm-200/80">curl -fsSL https://nodeproxy.ai/install.sh | sh</span>
    </div>
    <!-- Copy button -->
    <button class="shrink-0 flex items-center gap-1.5 px-3 py-1.5 text-[12px] font-medium 
                   text-warm-300/50 hover:text-warm-100 bg-aubergine-800/50 hover:bg-aubergine-700/50 
                   rounded-lg ring-1 ring-aubergine-700/30 transition-all cursor-pointer">
      [copy icon] Copy
    </button>
  </div>
</div>
```

### Content Cards

**Standard card:**
```css
background: rgba(59,10,46,0.5);       /* bg-aubergine-850/50 */
border-radius: 1rem;                   /* rounded-2xl */
border: 1px solid rgba(94,39,80,0.3); /* border-aubergine-700/30 */
overflow: hidden;
transition: box-shadow 0.3s;

:hover {
  box-shadow: 0 0 30px rgba(233,84,32,0.04), 0 8px 32px rgba(0,0,0,0.15);
}
```

**Card with animated top-line reveal:**
```css
/* .card-line */
position: relative;
::before {
  content: ''; position: absolute;
  top: 0; left: 0; right: 0; height: 2px;
  background: linear-gradient(90deg, #E95420, #77216F);
  opacity: 0; transition: opacity 0.3s;
}
:hover::before { opacity: 1; }
```

**Card header area:**
```css
padding: 1rem 1.5rem;                         /* px-6 py-4 */
border-bottom: 1px solid rgba(94,39,80,0.2); /* border-aubergine-700/20 */
```

**Card body area:**
```css
padding: 1.25rem 1.5rem; /* px-6 py-5 */
```

**Highlighted pricing card (Pro):**
```css
border: 2px solid rgba(233,84,32,0.4);
box-shadow: 0 10px 40px rgba(233,84,32,0.06);
```

### Section Accent Left Border

```css
/* .accent-left — used on deep-dive section copy columns */
border-left: 2px solid rgba(233,84,32,0.2);
padding-left: 2rem;
```

### Section Line Decorator

```css
/* .section-line::before — orange gradient line above section content */
::before {
  content: ''; display: block;
  width: 40px; height: 2px;
  background: linear-gradient(90deg, #E95420, transparent);
  margin-bottom: 1.5rem;
}
```

### Divider

```html
<!-- Center dot divider between major sections -->
<div class="flex items-center justify-center gap-5 py-4">
  <div class="w-24 h-px bg-gradient-to-r from-transparent to-aubergine-700/40"></div>
  <div class="w-2 h-2 rounded-full bg-ubuntu-orange/30"></div>
  <div class="w-24 h-px bg-gradient-to-l from-transparent to-aubergine-700/40"></div>
</div>
```

```html
<!-- Thin centered orange line divider -->
<div class="w-12 h-px bg-gradient-to-r from-transparent via-ubuntu-orange/40 to-transparent mx-auto mb-10"></div>
```

### Flow Diagram (Horizontal Pipeline)

```html
<!-- How It Works flow, inside rounded-3xl container -->
<div class="bg-aubergine-850/50 border border-aubergine-700/30 rounded-3xl p-10 md:p-14">
  <div class="flex flex-col md:flex-row items-center justify-center gap-5">
    
    <!-- Normal node -->
    <div class="px-8 py-6 bg-aubergine-800/60 rounded-2xl border border-aubergine-700/30 
                text-center min-w-[190px] hover:-translate-y-1 transition-transform">
      <div class="text-[11px] uppercase tracking-[0.12em] text-warm-400/40 font-medium mb-2">Label</div>
      <div class="font-medium text-[17px] text-white">Node Name</div>
      <div class="text-[13px] text-warm-300/40 mt-1">Subtitle</div>
    </div>
    
    <!-- Arrow connector -->
    <svg ...chevron in text-ubuntu-orange/50.../>
    
    <!-- Active/highlighted node (Edge Proxy) -->
    <div class="px-8 py-6 bg-gradient-to-b from-ubuntu-orange/[0.12] to-aubergine-800/60 
                rounded-2xl border border-ubuntu-orange/20 text-center min-w-[200px]
                shadow-lg shadow-ubuntu-orange/[0.06] hover:-translate-y-1 
                hover:shadow-xl hover:shadow-ubuntu-orange/10 transition-all">
      <div class="text-[11px] uppercase tracking-[0.12em] text-ubuntu-orange/60 font-medium mb-2">Edge Proxy</div>
      <div class="font-medium text-[17px] text-white">Node Proxy</div>
      <!-- Active status dot -->
      <div class="flex items-center justify-center gap-1.5 mt-2">
        <span class="w-1.5 h-1.5 rounded-full bg-ubuntu-orange ping-dot"></span>
        <span class="text-[12px] text-ubuntu-orange/70 font-mono">active</span>
      </div>
    </div>
  </div>
</div>
```

### Numbered Step List

```html
<div class="flex items-center gap-2.5">
  <span class="w-5 h-5 rounded-full bg-ubuntu-orange/10 text-ubuntu-orange 
               text-[11px] font-bold flex items-center justify-center shrink-0">1</span>
  <span class="text-warm-300/60 text-[13px]">Step description here</span>
</div>
```

### Info Panel (inside a card)

```html
<div class="p-5 rounded-xl bg-aubergine-850/50 border border-aubergine-700/30 mb-6">
  <div class="flex items-start gap-3">
    <span class="text-ubuntu-orange mt-0.5">[icon 20x20]</span>
    <div>
      <p class="text-[14px] font-medium text-white mb-1">Panel Title</p>
      <p class="text-warm-300/50 text-[14px] leading-[1.7]">Panel description text.</p>
    </div>
  </div>
</div>
```

### Status Badge Pills

```html
<!-- Orange badge -->
<span class="text-[12px] text-ubuntu-orange/70 bg-ubuntu-orange/10 px-2.5 py-1 rounded-full">$0/mo</span>

<!-- Green badge -->
<span class="text-[12px] text-green-400/70 bg-green-400/10 px-2.5 py-1 rounded-full">encrypted</span>

<!-- Neutral badge -->
<span class="text-[12px] text-warm-300/40 bg-aubergine-800/50 px-2.5 py-1 rounded-full">automatic</span>
```

### Feature Icon Wrap (Card Hover Bounce)

```css
.feature-icon-wrap {
  transition: transform 0.35s cubic-bezier(0.34, 1.56, 0.64, 1);
}
.group:hover .feature-icon-wrap {
  transform: scale(1.1) rotate(-5deg);
}
```

### Ubuntu Circle of Friends

```css
.cof {
  width: 32px; height: 32px; /* or 48px for CTA section */
  border-radius: 50%;
  background: conic-gradient(from 0deg,
    #E95420 0deg 120deg,
    #77216F 120deg 240deg,
    #F47B53 240deg 360deg
  );
  position: relative;
}
.cof::after {
  content: ''; position: absolute; inset: 8px;
  border-radius: 50%;
  background: var(--cof-bg, #2C001E); /* set --cof-bg to match surface */
}
```

### Active Status Ping Dot

```css
.ping-dot { position: relative; }
.ping-dot::after {
  content: ''; position: absolute; inset: -4px;
  border-radius: 50%;
  border: 1px solid #E95420;
  animation: dotPing 2.5s ease-out infinite;
}
@keyframes dotPing {
  0%   { transform: scale(1); opacity: 0.5; }
  100% { transform: scale(2.5); opacity: 0; }
}
```

### Agent Prompt Block

```html
<!-- For displaying a copyable agent instruction -->
<div class="bg-aubergine-950/80 rounded-xl px-5 py-4 ring-1 ring-aubergine-700/25 mb-5 
            flex items-center justify-between gap-4">
  <p class="text-[15px] text-warm-200/80 leading-[1.7]">
    Install Node Proxy from:<br>
    <span class="text-ubuntu-orange/80">https://nodeproxy.ai/mcp-agent-api</span>
  </p>
  <button class="shrink-0 flex items-center gap-1.5 px-3 py-1.5 text-[12px] font-medium 
                 text-warm-300/50 hover:text-warm-100 bg-aubergine-800/50 hover:bg-aubergine-700/50 
                 rounded-lg ring-1 ring-aubergine-700/30 transition-all">
    [copy icon] Copy
  </button>
</div>
```

### Footer

```css
border-top: 1px solid rgba(94,39,80,0.3); /* border-aubergine-700/30 */
padding: 2rem 1.5rem; /* py-8 px-6 */
max-width: 64rem; margin: 0 auto; /* max-w-5xl */
```

- Brand: `w-5 h-5` logo + `text-[13px] text-warm-300/40`
- Social icons: `text-warm-400/30 hover:text-warm-200/70`
- Footer links: `text-[12px] text-warm-400/30 hover:text-warm-200/70`
- Copyright: `text-[12px] text-warm-400/20`

---

## 5. Layout Principles

### Content Width

```
max-w-5xl (64rem / 1024px) — most sections
max-w-6xl (72rem / 1152px) — hero section
max-w-2xl (42rem / 672px)  — centered text blocks, subheadings, footers
max-w-xl  (36rem / 576px)  — terminal blocks, contained forms
max-w-md  (28rem / 448px)  — small terminal in hero
max-w-[480px]              — hero body text max-width
```

### Section Rhythm

```
Hero:          min-h-[85vh] — flex items-center, pt-28 pb-16
Main sections: py-16 or py-24 — most content sections
Dense sections: py-16
Spacious sections: py-24 (features, security, pricing)
```

### Two-Column Patterns

```html
<!-- Standard 50/50 (agent-first, how-it-works) -->
<div class="grid grid-cols-1 lg:grid-cols-2 gap-12 lg:gap-16 items-center">

<!-- Email section: copy | wider visual -->
<div class="grid grid-cols-1 lg:grid-cols-[1fr_1.2fr] gap-12 lg:gap-16 items-center">

<!-- Photos section: wider visual | copy -->
<div class="grid grid-cols-1 lg:grid-cols-[1.2fr_1fr] gap-12 lg:gap-16 items-center">
```

### Grid Patterns

```html
<!-- Pricing: 3 column -->
<div class="grid grid-cols-1 md:grid-cols-3 gap-5">

<!-- Security cards: 2 column -->
<div class="grid grid-cols-1 md:grid-cols-2 gap-5 mt-10">

<!-- Feature rows inside cards: flex column -->
<div class="flex flex-col gap-4">

<!-- Check list items: flex column -->
<div class="flex flex-col gap-2">
```

### Horizontal Padding

```
Sections:   px-6           (mobile)  → px-6  (desktop, constrained by max-w)
Nav:        px-6           (mobile)  → px-10 (lg:px-10)
Cards:      px-6 (header)  py-4 / px-6 py-5 (body)
Flow nodes: px-8 py-6
Pricing:    p-8
Feature:    p-8
```

### Z-Index Layer Stack

```
z-50   — Fixed navigation
z-10   — Hero content (above blobs)
blobs  — position: absolute, z-0 (below content, within section overflow:hidden)
```

---

## 6. Depth & Elevation

### Shadow System

NodeProxy uses restrained shadows that feel more like ambient glow than hard drop shadows.

| Level | CSS | Usage |
|---|---|---|
| Flat | none | Standard cards at rest |
| Card hover | `0 0 30px rgba(233,84,32,0.04), 0 8px 32px rgba(0,0,0,0.15)` | `.card-glow:hover` |
| Orange glow (Pro card) | `box-shadow: 0 10px 40px rgba(233,84,32,0.06)` | Highlighted pricing card |
| Orange hover (Pro card) | `0 20px 60px rgba(233,84,32,0.1)` | Pricing card hover state |
| CTA button hover | `0 20px 60px rgba(233,84,32,0.2)` | Primary orange button hover |
| Terminal glow hover | `0 0 40px rgba(233,84,32,0.08)` | Terminal window hover |
| Terminal glow halo | `bg-ubuntu-orange/[0.04] blur-xl translate-y-2` | Pseudo-glow behind terminal (div, not CSS shadow) |
| SVG nodes | `feDropShadow dy=3 stdDeviation=6 #1a0a1e 22%` | Architecture diagram nodes |
| SVG node glow | `feGaussianBlur stdDeviation=3` | Glowing mesh coordinator node |

### Background Layering

```
1. Body background:   #2C001E with SVG noise texture (opacity 0.015)
2. Hero blobs:        absolute, blur(100px), partially transparent
   - Blob 1 (orange): 650×650px, top-right, opacity 0.1, #E95420
   - Blob 2 (purple): 500×500px, bottom-left, opacity 0.2, #77216F  
   - Blob 3 (orange): 280×280px, mid-left, opacity 0.05, #E95420
3. Section surfaces:  bg-aubergine-850/50 (semi-transparent cards)
4. Card headers:      bg-aubergine-800/30 (slight elevation)
5. Terminal/code:     bg-aubergine-950 (deepest, most contrast)
```

### Surface Elevation by Hex Value

```
Lowest (body):    #2C001E
↑ Card surface:   rgba(59,10,46,0.5)   — bg-aubergine-850/50
↑ Card header:    rgba(74,20,66,0.3)   — bg-aubergine-800/30
↑ Input/code:     rgba(74,20,66,0.5)   — bg-aubergine-800/50
↑ Terminal body:  #1a0a1e              — bg-aubergine-950
```

### Border Elevation

```
Standard card:    border: 1px solid rgba(94,39,80,0.3)
Card hover:       border: 1px solid rgba(94,39,80,0.4)   (border-aubergine-600/40)
Active/featured:  border: 2px solid rgba(233,84,32,0.4)
Nav:              border-bottom: 1px solid rgba(94,39,80,0.3)
Inner dividers:   border: 1px solid rgba(94,39,80,0.2)   (lighter, interior)
Ring (terminal):  box-shadow: inset 0 0 0 1px rgba(94,39,80,0.3)
```

---

## 7. Do's and Don'ts

### ✅ Do

- **Use `text-balance`** on multi-line headings for balanced line breaks (`text-wrap: balance`)
- **Use `font-light` (300)** for the secondary/contrast line within H2 headings
- **Use opacity modifiers** rather than separate color values for muted text — it harmonizes with backgrounds
- **Use `rounded-full`** for all pill buttons (CTAs, nav CTA, pricing CTAs)
- **Use `rounded-2xl`** for cards and feature containers
- **Use `rounded-[16px]`** for terminal windows (slightly less than 2xl)
- **Use `rounded-3xl`** for large section containers (How It Works, pricing grid)
- **Layer orange accents sparingly** — it's energy, not wallpaper. Keep it to CTAs, icons, active states, eyebrows
- **Use transition classes on interactive elements** — `transition-colors`, `transition-all`, `transition-transform`
- **Use `hover:-translate-y-0.5`** on primary buttons (lift effect)
- **Use `hover:-translate-y-1`** on flow diagram nodes and feature cards
- **Pair eyebrow labels with inline SVG icons** — they appear at the same size as the text
- **Use `font-mono` for all code** — Ubuntu Mono at 14px for body code, 12px for labels
- **Stagger reveal animations** — use `reveal-d1`, `reveal-d2`, `reveal-d3` for grid siblings
- **Use `backdrop-blur-xl`** on the nav — it's a key part of the frosted glass effect
- **Keep section max-width at `max-w-5xl`** — don't let content sprawl to full viewport width
- **Use `clamp()` for heading font sizes** — they scale fluidly between mobile and desktop
- **Use the noise texture** on the body — it adds subtle organic warmth

### ❌ Don't

- **Don't use pure black backgrounds** — aubergine-950 (`#1a0a1e`) is the darkest allowed, and only for terminal windows
- **Don't use pure white text everywhere** — headings use `text-white`, body text uses the warm scale
- **Don't use hard box shadows** (e.g., `box-shadow: 0 4px 6px black`) — all shadows use very low opacity or orange tints
- **Don't use solid borders** without the transparency modifier — borders should feel light, not heavy
- **Don't use the orange as a background fill for large areas** — only for buttons, icons, small badges
- **Don't use sans-serif for code blocks** — always Ubuntu Mono
- **Don't left-align section headings on mobile** — headings centered on mobile, left-aligned on `lg:`
- **Don't use default Tailwind blue for focus states** — override to `outline: 2px solid #E95420`
- **Don't use border-radius on the traffic light dots** — they use `rounded-full` (already correct, but don't use square)
- **Don't use bright green for success states** — use `text-green-400/70 bg-green-400/10` for encrypted/active badges (muted)
- **Don't use uppercase for body text** — uppercase is reserved for eyebrow labels and diagram zone labels only
- **Don't put more than one orange CTA in the same visual area** — they lose impact when repeated
- **Don't skip the `no-underline` class on `<a>` elements** — the default link underline conflicts with the design
- **Don't use `font-bold` on body text** — body weight is 400 (regular) or 300 (light), with 500 (medium) for labels
- **Don't omit the `transition` class on hover states** — all interactive elements should transition smoothly

---

## 8. Responsive Behavior

### Breakpoints (Tailwind defaults)

```
sm:  640px  — tablets, small breakpoints (flow diagram stacking)
md:  768px  — mid-layout switches (testimonial stagger, security grid)
lg:  1024px — main desktop layout (two-column grids, nav link visibility)
```

### Nav

- Mobile: logo + CTA button only (all nav links hidden with `hidden md:block`)
- Desktop (md+): full nav with links, separator, CTA

### Hero

- Mobile: single column, text centered (`text-center lg:text-left`)
- Desktop (lg): two-column, 50/50, text left-aligned
- CTA buttons: centered on both (inside right column at desktop)

### Two-Column Sections

- Mobile: `grid-cols-1`, vertical stacking
- Desktop (lg): two columns
- Some sections swap visual order on mobile via `order-2 lg:order-1`

### Flow Diagram (How It Works)

- Mobile: `flex-col` (vertical flow with `↓` arrows)
- Desktop (md): `flex-row` (horizontal flow with `→` arrows)
- Arrow SVGs: `md:rotate-0 rotate-90` (90° rotation on mobile)

### Pricing Grid

- Mobile: `grid-cols-1`
- Tablet/Desktop (md): `grid-cols-3`

### Security Cards Grid

- Mobile: `grid-cols-1`
- Desktop (md): `grid-cols-2`

### SVG Architecture Diagram

- Full-width, aspect ratio preserved via `viewBox="0 0 1060 900"` + `class="w-full h-auto"`
- On small screens, text labels may become small — this is accepted; the diagram is decorative/supplementary

### Testimonials

- Desktop (md+): second card offset with `transform: translateY(24px)` for a staggered look
- Mobile: no offset

### Typography Scaling

- `clamp(2.4rem, 5vw, 4.2rem)` — H1 scales from ~38px mobile to ~67px wide desktop
- `clamp(2rem, 4vw, 2.8rem)` — H2 scales from 32px to ~45px
- `clamp(1.8rem, 4vw, 3rem)` — Statement scales from 29px to ~48px

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
  .kinetic-word { opacity: 1 !important; transform: none !important; }
  .reveal { opacity: 1 !important; transform: none !important; }
  .marquee-track { animation: none !important; }
  .hero-blob { animation: none !important; }
}
```

---

## 9. Agent Prompt Guide

Use this section as a direct reference when generating UI for NodeProxy.

### Generating a New Section

```
Create a NodeProxy section with:
- Background: transparent (inherits #2C001E body)
- Max width: max-w-5xl mx-auto with px-6 horizontal padding
- Vertical spacing: py-24
- Section eyebrow: 12px bold uppercase tracking-[0.15em] in text-ubuntu-orange
- H2: clamp(2rem,4vw,2.8rem) bold tracking-[-0.025em] leading-[1.12] text-white
- H2 secondary line: font-light text-warm-300/50
- Body text: 16px text-warm-300/60 leading-[1.8]
- Add .reveal class for scroll-triggered entrance animation
```

### Generating a Card

```
NodeProxy card:
- bg-aubergine-850/50 (rgba(59,10,46,0.5))
- rounded-2xl
- border border-aubergine-700/30 (1px solid rgba(94,39,80,0.3))
- overflow-hidden
- card-glow class (hover shadow)
- card-line class (top orange-to-purple line on hover)
Header: px-6 py-4 with border-b border-aubergine-700/20, text-[13px] font-medium text-warm-200/70
Body: px-6 py-5
```

### Generating a CTA Button

```
NodeProxy primary CTA:
- group relative px-8 py-3.5
- bg-ubuntu-orange (#E95420) text-white
- rounded-full
- font-size 15px font-weight 500
- overflow-hidden no-underline
- transition-all hover:shadow-xl hover:shadow-ubuntu-orange/20 hover:-translate-y-0.5
- Inside: span.absolute.inset-0 with gradient from-ubuntu-orange-light to-ubuntu-orange opacity-0 group-hover:opacity-100 transition-opacity
- Inside: span.relative with button text
```

### Generating a Terminal Block

```
NodeProxy terminal:
- Outer wrapper: relative (for glow halo)
- Halo div: absolute inset-0 translate-y-2 bg-ubuntu-orange/[0.04] rounded-[16px] blur-xl
- Container: relative bg-aubergine-950 (#1a0a1e) rounded-[16px] overflow-hidden ring-1 ring-aubergine-700/30
- Header: px-5 py-3 bg-aubergine-800/30 border-b border-aubergine-700/20
  - Traffic lights: w-3 h-3 rounded-full in colors [bg-ubuntu-orange/50, bg-aubergine-500/40, bg-warm-400/30]
  - Label: font-mono text-[12px] text-aubergine-400/50
- Body: px-6 py-5 font-mono text-[14px]
  - Prompt: span text-warm-400/40 non-selectable "$ "
  - Command: span text-warm-200/80
```

### NodeProxy Logo SVG

The radial node logo is an SVG with `viewBox="0 0 100 100"`:

```
Center node: circle cx=50 cy=50 r=10 fill=#E95420
Center inner: circle cx=50 cy=50 r=6 fill=#F47B53

Primary spokes (8): lines from center to r=34 and beyond, stroke=#E95420 stroke-width=1.5 opacity=0.7
  Directions: N, E, S, W + 4 diagonals (NE, SE, SW, NW)

Outer primary nodes (8): circle r=4 at cardinal points (cx=50 cy=12, cx=88 cy=50, etc.)
  + circle r=3.5 at diagonal points (cx=85 cy=18, etc.)
  All: fill=#E95420 opacity=0.9

Secondary spokes (8): lines at intermediate angles, stroke=#D08EC8 stroke-width=1 opacity=0.3
Secondary outer nodes (8): circle r=2.5, fill=#AE5CA5 opacity=0.5 at ~70°, ~20°, etc.
```

### Typography Cheat Sheet

```
Page H1 (hero):       font-bold text-[clamp(2.4rem,5vw,4.2rem)] leading-[1.08] tracking-[-0.035em]
Section H2:           font-bold text-[clamp(2rem,4vw,2.8rem)] tracking-[-0.025em] leading-[1.12]
Eyebrow label:        text-[12px] font-bold uppercase tracking-[0.15em] text-ubuntu-orange
Body high:            text-[17px] text-warm-200/70 leading-[1.75]
Body standard:        text-[16px] text-warm-300/60 leading-[1.8]
Card label:           text-[13px] font-medium text-warm-200/70
Card sublabel:        text-[13px] text-warm-400/40 leading-[1.7]
Check list value:     text-[14px] text-warm-200/70
Check list label:     text-[14px] text-warm-300/50
Code / terminal:      font-mono text-[14px] text-warm-200/80
Code prompt:          font-mono text-warm-400/40
Muted hint:           text-[13px] text-warm-400/30
```

### Scroll Reveal Pattern

```html
<!-- Add to any element that should animate in on scroll -->
<div class="reveal">...</div>               <!-- no delay -->
<div class="reveal reveal-d1">...</div>     <!-- 0.1s delay -->
<div class="reveal reveal-d2">...</div>     <!-- 0.2s delay -->
<div class="reveal reveal-d3">...</div>     <!-- 0.3s delay -->
```

```css
.reveal {
  opacity: 0;
  transform: translateY(28px);
  transition: opacity 0.7s cubic-bezier(0.16,1,0.3,1), transform 0.7s cubic-bezier(0.16,1,0.3,1);
}
.reveal.visible { opacity: 1; transform: translateY(0); }
.reveal-d1 { transition-delay: 0.1s; }
.reveal-d2 { transition-delay: 0.2s; }
.reveal-d3 { transition-delay: 0.3s; }
```

```js
const obs = new IntersectionObserver(
  (entries) => entries.forEach(e => {
    if (e.isIntersecting) { e.target.classList.add('visible'); obs.unobserve(e.target); }
  }),
  { threshold: 0.12, rootMargin: '0px 0px -30px 0px' }
);
document.querySelectorAll('.reveal').forEach(el => obs.observe(el));
```

### Color Quick Reference

```
Body bg:        #2C001E
Deep surface:   #1a0a1e
Card surface:   #3B0A2E at 50% opacity
Orange:         #E95420
Orange light:   #F47B53
Purple 600:     #77216F
Purple 300:     #D08EC8
Text white:     #FFFFFF
Text high:      #F2E2D4 at 70%
Text medium:    #E8D0BB at 60%
Text muted:     #E8D0BB at 50%
Text dim:       #D4B49A at 40%
Text ghost:     #D4B49A at 30%
Border:         #5E2750 at 30%
```
