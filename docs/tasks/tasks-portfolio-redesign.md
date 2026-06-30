# Tasks: Portfolio Redesign for LinkedIn

| Field        | Value                    |
|-------------|--------------------------|
| Status      | Not Started              |
| Created     | 2026-05-15               |
| Author      | Dylan                    |
| Spec        | docs/specs/rfc-portfolio-redesign.md |
| Design      | docs/designs/design-portfolio-redesign.md |
| Phase       | Tasks                    |

## Task Execution Order

### Phase 1: Strip Flashy Effects (JS + HTML)

#### Task 1: Remove particle canvas, cursor glow, orbs, and background shapes

| Field | Value |
|-------|-------|
| ID | T-001 |
| Type | Modify |
| Estimated Effort | Small |
| Dependencies | None |

**Description**: Delete all the flashy visual elements from the HTML and their associated JavaScript.

**HTML to delete**:
- `<div class="bg-shapes">...</div>` (3 bg-shape children)
- `<div class="cursor-glow" id="cursorGlow"></div>`
- `<canvas id="particleCanvas"></canvas>`
- All 3 `<div class="hero-orb"></div>` elements

**JS to delete**:
- Entire particle canvas section (class Particle, initParticles, connectParticles, animateParticles, canvas resize, canvas mousemove/mouseleave)
- Cursor glow listener (`document.addEventListener('mousemove', ...)` that sets glow position)
- Parallax on hero orbs (`window.addEventListener('scroll', ...)` that transforms `.hero-orb`)
- 3D tilt on project cards (`document.querySelectorAll('.project-card').forEach(...)`)
- 3D tilt on philosophy cards (`document.querySelectorAll('.philosophy-card').forEach(...)`)
- Magnetic effect on contact cards (`document.querySelectorAll('.contact-card').forEach(...)`)

**Acceptance Criteria**:
- [ ] No `<canvas>` element in DOM
- [ ] No `requestAnimationFrame` calls in JS
- [ ] No mousemove-based transform listeners
- [ ] Page loads cleanly with no JS errors

---

#### Task 2: Remove sections — Philosophy, Fun Facts, Education

| Field | Value |
|-------|-------|
| ID | T-002 |
| Type | Modify |
| Estimated Effort | Small |
| Dependencies | None |
| Parallel With | T-001 |

**Description**: Delete entire HTML sections and their nav links.

**HTML to delete**:
- `<section id="philosophy">...</section>` (entire block)
- `<section id="fun-facts">...</section>` (entire block)
- `<section id="education">...</section>` (entire block)
- The `<svg class="footer-wave">` element (between contact and footer)
- Remove nav links to these sections: `<li><a href="#education">Education</a></li>`

**Also delete from About section**:
- `<div class="stats-row reveal">...</div>` (the entire stats row with counters)

**JS to delete**:
- Counter animation (IntersectionObserver for `[data-count]` elements) — no data-count elements will remain

**Acceptance Criteria**:
- [ ] Only 5 nav links remain: About, Projects, Skills, Experience, Contact
- [ ] No `#philosophy`, `#fun-facts`, `#education` sections in DOM
- [ ] No counter animation JS
- [ ] No stats-row in about section

---

### Phase 2: Restructure Nav & About

#### Task 3: Add social links to nav bar

| Field | Value |
|-------|-------|
| ID | T-003 |
| Type | Modify |
| Estimated Effort | Small |
| Dependencies | T-001, T-002 |

**Description**: Add LinkedIn, GitHub, and Email as small icon pills in the nav, always visible.

**CSS to add**:
```css
.nav-social {
  display: flex; align-items: center; gap: 0.5rem;
}
.nav-social a {
  display: flex; align-items: center; justify-content: center;
  width: 34px; height: 34px; border-radius: 8px;
  color: var(--text-muted); text-decoration: none;
  border: 1px solid var(--border);
  font-size: 0.8rem; font-weight: 600;
  transition: all 0.2s ease;
}
.nav-social a:hover {
  color: var(--text); border-color: var(--purple);
  background: rgba(139,92,246,0.08);
}
```

**HTML** — restructure nav:
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

**Mobile**: Social links stay visible (they're only ~120px wide total), menu toggle for nav links.

**Acceptance Criteria**:
- [ ] LinkedIn, GitHub, Email pills visible in nav on desktop
- [ ] Links open correct URLs
- [ ] Hover state works (border highlight)
- [ ] Still looks good on mobile (doesn't overflow)

---

#### Task 4: Simplify About section to single centered column

| Field | Value |
|-------|-------|
| ID | T-004 |
| Type | Modify |
| Estimated Effort | Small |
| Dependencies | T-002 |
| Parallel With | T-003 |

**Description**: Replace the two-column about grid with a centered single-column layout.

**Replace about section HTML** with:
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

**CSS to add**:
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

**Acceptance Criteria**:
- [ ] About section is single centered column
- [ ] Avatar, name, title, bio, and tags all render correctly
- [ ] No two-column grid on any viewport
- [ ] Section header has no subtitle (just "About Me" + accent bar)

---

### Phase 3: CSS Cleanup & Refinement

#### Task 5: Remove dead CSS and tone down animations

| Field | Value |
|-------|-------|
| ID | T-005 |
| Type | Modify |
| Estimated Effort | Medium |
| Dependencies | T-001, T-002, T-003, T-004 |

**Description**: Clean up all CSS that references removed elements and refine remaining animations.

**CSS blocks to delete**:
- `.cursor-glow` styles
- `#particleCanvas` styles
- `.hero-orb` styles + `@keyframes orbFloat`
- `.bg-shapes` + `.bg-shape` styles + `@keyframes floatShape`
- `.philosophy-card`, `.philosophy-grid`, `.philosophy-icon` — all philosophy styles
- `.fact-card`, `.fact-emoji`, `.facts-grid` — all fun-facts styles
- `.education-card`, `.edu-icon`, `.edu-info`, `.edu-subjects` — all education styles
- `@keyframes borderRotate` (only used by education card — keep if featured cards use it... check: yes, featured cards reference `animation: borderRotate`. KEEP this keyframe.)
- `.stats-row`, `.stat-item` styles
- `.about-grid`, `.about-visual`, `.about-text`, `.about-card` styles (replaced by `.about-centered`)
- `.footer-wave` styles

**CSS to modify**:
- `.reveal` — change `translateY(40px)` to `translateY(20px)`, `0.8s` to `0.6s`
- `.reveal-left` — change `translateX(-60px)` to `translateX(-30px)`
- `.reveal-right` — change `translateX(60px)` to `translateX(30px)`
- `.reveal-scale` — change `scale(0.85)` to `scale(0.92)`
- `.project-card:hover` — reduce shadow, change to `translateY(-4px)`
- `.skill-card:hover` — reduce to `translateY(-4px)`, tone down shadow
- `.contact-card:hover` — reduce to `translateY(-4px)`
- Hero `.hero` — reduce `min-height` to `90vh`
- Remove `[data-theme="light"] .hero-orb` and `[data-theme="light"] .bg-shape` rules
- Remove print styles for removed elements (`#particleCanvas`, `#fun-facts`)

**Acceptance Criteria**:
- [ ] No CSS references orphaned elements
- [ ] Animations are subtle (20px translate, 0.6s timing)
- [ ] Hover effects are restrained (4px lift max)
- [ ] No unused keyframe animations (except borderRotate which featured cards need)
- [ ] File size reduced noticeably

---

### Phase 4: Final Polish

#### Task 6: Verify and fix light mode, mobile, and overall consistency

| Field | Value |
|-------|-------|
| ID | T-006 |
| Type | Test/Fix |
| Estimated Effort | Small |
| Dependencies | T-005 |

**Description**: Final pass to ensure everything works together.

**Checks**:
- Light mode: all sections render correctly, no broken references
- Dark mode: professional, clean look
- Mobile (≤768px): nav collapses, social links visible, projects stack, about centered
- Active nav highlighting still works with reduced section list
- Theme toggle persists via localStorage
- All project card links work
- No dead `id` references in JS (active nav link highlighting uses `section[id]`)

**Fixes if needed**:
- Update active nav link JS to only target existing sections
- Ensure `.nav-social` doesn't break mobile layout
- Test featured cards render correctly after CSS changes

**Acceptance Criteria**:
- [ ] Zero console errors
- [ ] Both themes look correct
- [ ] Mobile layout clean and usable
- [ ] All links functional
- [ ] Page feels fast and professional

---

## Progress Tracking

| Task ID | Status | Notes |
|---------|--------|-------|
| T-001 | Not Started | Remove flashy JS + HTML |
| T-002 | Not Started | Remove sections |
| T-003 | Not Started | Add social nav links |
| T-004 | Not Started | Simplify about section |
| T-005 | Not Started | CSS cleanup |
| T-006 | Not Started | Final QA |
