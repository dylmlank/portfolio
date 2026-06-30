# Design: Portfolio Redesign for LinkedIn

| Field        | Value                    |
|-------------|--------------------------|
| Status      | Draft                    |
| Created     | 2026-05-15               |
| Author      | Dylan                    |
| Spec        | docs/specs/rfc-portfolio-redesign.md |
| Phase       | Design                   |

## Overview

Complete rework of `~/portfolio/index.html` — stripping flashy effects, removing 4 sections, adding social links to nav, and refining the dark aesthetic. Still a single-file static site.

## Architecture

Single HTML file with embedded `<style>` and `<script>`. No build step, no framework.

```
index.html
├── <style> (CSS variables, nav, hero, marquee, about, projects, skills, experience, contact, footer)
├── <body> (HTML structure — 6 content sections + nav + footer)
└── <script> (typing animation, scroll reveals, skill bar animation, counter animation, theme toggle, nav scroll)
```

## CSS Changes

### Remove These CSS Blocks Entirely

- `.cursor-glow` — cursor follow effect
- `#particleCanvas` — canvas element
- `.hero-orb` + `@keyframes orbFloat` — animated gradient blurs
- `.bg-shapes` + `.bg-shape` + `@keyframes floatShape` — floating background circles
- `.philosophy-card`, `.philosophy-grid`, `.philosophy-icon` — removed section
- `.fact-card`, `.fact-emoji`, `.facts-grid` — removed section
- `.education-card`, `.edu-icon`, `.edu-info`, `.edu-subjects`, `@keyframes borderRotate` — removed section
- `.stats-row`, `.stat-item` — redundant stats

### Modify These CSS Blocks

#### Nav — Add Social Links

```css
.nav-social {
  display: flex; align-items: center; gap: 0.5rem;
}
.nav-social a {
  display: flex; align-items: center; justify-content: center;
  width: 34px; height: 34px; border-radius: 8px;
  color: var(--text-muted); text-decoration: none;
  border: 1px solid var(--border);
  font-size: 0.85rem; transition: all 0.2s ease;
}
.nav-social a:hover {
  color: var(--text); border-color: var(--purple);
  background: rgba(139,92,246,0.08);
}
```

#### Nav Layout Update

```css
nav {
  /* Keep fixed positioning, blur backdrop */
  display: grid; grid-template-columns: auto 1fr auto auto;
  align-items: center; gap: 1rem;
}
```

#### Hero — Simplify

- Remove orb references
- Remove `#particleCanvas` references
- Keep typing animation, keep buttons
- Reduce hero min-height to `90vh` (less dramatic)

#### Scroll Reveals — Tone Down

```css
.reveal { opacity: 0; transform: translateY(20px); transition: opacity 0.6s ease, transform 0.6s ease; }
/* Was translateY(40px) and 0.8s — more subtle now */
```

#### Project Cards — Remove 3D Tilt, Keep Featured

- Remove `perspective()` and `rotateX/Y` transforms
- Keep `.featured` class with gradient border
- Simpler hover: `transform: translateY(-4px)` + subtle shadow increase

#### Contact Cards — Remove Magnetic Effect

- Simple hover: `transform: translateY(-4px)` only

#### About Section — Condense

- Remove `.about-grid` two-column layout
- Single centered column with avatar, name, title, short bio, tags
- Remove `.stats-row` entirely

### Refined Color Adjustments

```css
:root {
  /* Slightly less saturated accents for professional feel */
  --purple: #7c5cbf;  /* was #8b5cf6 — a touch more muted */
  --pink: #d946a8;    /* was #ec4899 — slightly less neon */
  --cyan: #0891b2;    /* was #06b6d4 — slightly deeper */
}
```

Actually — keep the existing colors. They're fine. The issue is *how much* they're used, not the colors themselves. Reducing the gradients, glows, and animated borders will naturally tone things down.

## HTML Changes

### Remove These Elements

```html
<!-- DELETE -->
<div class="bg-shapes">...</div>
<div class="cursor-glow" id="cursorGlow"></div>
<canvas id="particleCanvas"></canvas>
<div class="hero-orb"></div> (x3)

<!-- DELETE sections -->
<section id="philosophy">...</section>
<section id="fun-facts">...</section>
<section id="education">...</section>

<!-- DELETE from about -->
<div class="stats-row reveal">...</div>
```

### Modify Nav HTML

```html
<nav id="navbar">
  <a href="#" class="nav-brand">D</a>
  <ul class="nav-links">
    <li><a href="#about">About</a></li>
    <li><a href="#projects">Projects</a></li>
    <li><a href="#skills">Skills</a></li>
    <li><a href="#experience">Experience</a></li>
    <li><a href="#contact">Contact</a></li>
  </ul>
  <div class="nav-social">
    <a href="https://www.linkedin.com/in/dylan-lankford-411398332/" target="_blank" rel="noopener" aria-label="LinkedIn" title="LinkedIn">in</a>
    <a href="https://github.com/dylmlank" target="_blank" rel="noopener" aria-label="GitHub" title="GitHub">GH</a>
    <a href="mailto:dylmlank@gmail.com" aria-label="Email" title="Email">@</a>
  </div>
  <div style="display:flex;align-items:center;gap:0.75rem;">
    <button class="theme-toggle" id="themeToggle" aria-label="Toggle theme">☾</button>
    <button class="menu-toggle" onclick="document.querySelector('.nav-links').classList.toggle('open')" aria-label="Toggle menu">☰</button>
  </div>
</nav>
```

### Simplify About Section

```html
<section id="about">
  <div class="section-header reveal">
    <h2>About Me</h2>
    <div class="accent-bar"></div>
  </div>
  <div class="about-centered reveal">
    <div class="about-avatar">D</div>
    <h3>Dylan</h3>
    <div class="title">Full-Stack Developer & Entrepreneur</div>
    <p>High school junior building full-stack apps, AI tools, and real businesses across JavaScript, TypeScript, Python, and Rust. Co-founded an auto detailing business, shipped eleven projects, and still just getting started.</p>
    <div class="about-tags">
      <span class="about-tag">Full-Stack Development</span>
      <span class="about-tag">Mobile Apps</span>
      <span class="about-tag">Rust / Tauri</span>
      <span class="about-tag">AI / LLM Tooling</span>
      <span class="about-tag">Entrepreneurship</span>
    </div>
  </div>
</section>
```

### About Centered CSS

```css
.about-centered {
  max-width: 600px; margin: 0 auto; text-align: center;
}
.about-centered .about-avatar { margin: 0 auto 1.5rem; }
.about-centered h3 { font-family: 'Space Grotesk', sans-serif; font-size: 1.6rem; font-weight: 700; margin-bottom: 0.25rem; }
.about-centered .title { color: var(--purple); font-weight: 600; font-size: 0.95rem; margin-bottom: 1.25rem; }
.about-centered p { color: var(--text-muted); font-size: 1.05rem; line-height: 1.8; margin-bottom: 1.5rem; }
.about-centered .about-tags { justify-content: center; }
```

## JavaScript Changes

### Remove Entirely

- Particle canvas (class `Particle`, `initParticles`, `connectParticles`, `animateParticles`, canvas event listeners)
- Cursor glow mousemove listener
- 3D tilt on project cards
- 3D tilt on philosophy cards (section removed)
- Magnetic effect on contact cards
- Parallax on hero orbs

### Keep (unchanged or minor tweaks)

- Typing animation
- Nav scroll class toggle
- Scroll reveal (IntersectionObserver)
- Skill bar animation
- Counter animation (only used if any `data-count` elements remain — remove if none)
- Theme toggle
- Active nav link highlighting
- Mobile menu toggle

### Counter Animation

Remove counter animation entirely — no more `data-count` elements after removing stats row and fun facts.

## Mobile Responsive Changes

- Nav social links: hide text labels, show icons only, or collapse into mobile menu
- On mobile (≤768px): `.nav-social` should be inside the mobile menu dropdown or shown as a row

```css
@media (max-width: 768px) {
  .nav-social { display: none; }
  .nav-links.open .nav-social-mobile { display: flex; gap: 1rem; padding: 1rem 0; }
}
```

Alternative (simpler): keep `.nav-social` always visible since the icons are small (34px each = 102px + gaps ≈ 120px total). That fits fine on most mobile screens alongside the brand and menu toggle.

## Testing Strategy

- Open in browser, verify no console errors
- Check dark mode appearance — professional, not flashy
- Toggle to light mode — verify all elements adapt
- Resize to mobile — nav, projects, about all stack correctly
- Click LinkedIn/GitHub/Email in nav — verify correct targets
- Scroll through — reveals fire subtly, no jank
- Verify featured projects still have gradient border + highlights
- Verify no leftover CSS for removed sections (dead code)

## Decisions Log

| Decision | Rationale | Alternatives Considered |
|----------|-----------|----------------------|
| Keep existing colors, just reduce usage | Colors aren't the problem — the animated gradients and glows are | Muting all accent colors |
| Social links as small icon pills in nav | Always accessible, doesn't compete with CTAs | Full text links, dropdown menu |
| Remove counter animations entirely | No data-count elements remain after removing stats/fun-facts | Keep stats somewhere else |
| Single-column about | Simpler, faster to scan, no avatar-beside-text layout to maintain | Keep two-column grid |
| Keep marquee | Shows breadth of tech stack at a glance, low-effort section | Remove it for more minimal feel |

## References

- Specification: `docs/specs/rfc-portfolio-redesign.md`
- Current site: `~/portfolio/index.html`
