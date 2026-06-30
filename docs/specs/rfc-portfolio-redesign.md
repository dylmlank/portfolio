# RFC: Portfolio Redesign for LinkedIn

| Field        | Value                    |
|-------------|--------------------------|
| Status      | Draft                    |
| Created     | 2026-05-15               |
| Author      | Dylan                    |
| Phase       | Specification            |

## Summary

Redesign the portfolio site to be LinkedIn-optimized — refined dark aesthetic, stripped of flashy effects, streamlined to 6 core sections, with social links always accessible in the nav. The goal is a site that says "I build serious things" to recruiters and connections clicking through from LinkedIn.

## Problem Statement

The current portfolio is heavy on visual effects (particle canvas, cursor glow, animated orbs, 3D tilt, gradient borders) that look impressive but read as "CSS demo" rather than "professional developer." It has 10 sections with redundant content (stats appear twice, philosophy is generic). For a LinkedIn audience — recruiters, hiring managers, potential collaborators — the site needs to load fast, communicate competence immediately, and make it easy to scan projects and experience.

## Goals

- **G1**: Communicate professionalism within 3 seconds of landing
- **G2**: Make projects and experience immediately scannable
- **G3**: Keep LinkedIn/GitHub/Email always one click away
- **G4**: Maintain dark aesthetic with intentional color accents (not neon overload)
- **G5**: Reduce page weight by removing particle canvas, cursor glow, animated orbs, and background shapes
- **G6**: Keep light/dark mode toggle functional

## Non-Goals

- No framework migration (stays single-file HTML/CSS/JS)
- No backend or CMS
- No blog or writing section
- Not redesigning the actual content/copy for each project (reuse existing descriptions)
- Not adding screenshots or images to project cards (keeping gradient previews)

## Requirements

### Functional Requirements

- **REQ-1**: Social links (LinkedIn, GitHub, Email) visible in nav bar at all times — Must have
- **REQ-2**: Streamlined to 6 sections: Hero, About (condensed), Projects, Experience, Skills, Contact — Must have
- **REQ-3**: Remove: Philosophy, Fun Facts, Education sections — Must have
- **REQ-4**: Remove: particle canvas, cursor glow effect, animated orbs, floating background shapes — Must have
- **REQ-5**: Keep featured project cards (Disconnect, Nebula, Pulse) with highlights — Must have
- **REQ-6**: Keep tech marquee (paused on hover) — Should have
- **REQ-7**: Keep typing animation in hero (toned down) — Should have
- **REQ-8**: Keep scroll reveal animations (subtle, not dramatic) — Should have
- **REQ-9**: Light/dark mode toggle preserved — Must have
- **REQ-10**: Mobile responsive — Must have

### Non-Functional Requirements

- **Performance**: No canvas rendering, no requestAnimationFrame loops. Page should feel instant.
- **Accessibility**: Proper contrast ratios in both themes, semantic HTML, ARIA labels on interactive elements
- **Load time**: Single file, no external JS dependencies, only Google Fonts

## Impact Analysis

### Affected Systems

| System/Component | Impact Type | Description |
|-----------------|-------------|-------------|
| `index.html` | Major rewrite | Remove 4 sections, strip JS effects, restyle nav |
| CSS variables | Modified | Tone down accent usage, refine color palette |
| JavaScript | Reduced | Remove particle canvas, cursor glow, orb parallax, 3D tilt, magnetic effect |
| Nav bar | Modified | Add social link pills/icons to nav |

### Dependencies

- Google Fonts (Inter + Space Grotesk) — keep as-is
- No other external dependencies

## Sections (Final Structure)

1. **Nav** — Brand + section links + social pills (LinkedIn, GitHub, Email) + theme toggle
2. **Hero** — Name, title, short tagline, typing animation, CTA buttons (View Work, Get In Touch)
3. **Marquee** — Scrolling tech stack (keep as-is, maybe slightly smaller)
4. **About** — Condensed single-column: avatar, name/title, 2-sentence bio, key tags. No stats row.
5. **Projects** — 3 featured (2-col span, bullet highlights) + 8 regular cards
6. **Skills** — Keep skill cards with bars, remove flashy hover gradients
7. **Experience** — Timeline (keep as-is, clean styling)
8. **Contact** — CTA + contact grid (email, GitHub, LinkedIn)
9. **Footer** — Minimal brand + copyright

## What Gets Removed

- Particle canvas + all canvas JS
- Cursor glow div + mousemove listener
- Hero animated orbs (3 gradient blurs)
- Floating background shapes
- 3D tilt effect on project cards
- 3D tilt on philosophy cards (section removed)
- Magnetic effect on contact cards
- Parallax on hero orbs
- Philosophy section entirely
- Fun Facts section entirely
- Education section entirely
- Duplicate stats row in About
- `#particleCanvas` element and all related JS

## What Gets Added

- Social link pills/icons in the nav bar (LinkedIn, GitHub, Email)
- Cleaner hover states (subtle border highlight, no dramatic transforms)
- Slightly toned-down scroll reveals (less translateY distance)

## Success Criteria

- [ ] Site loads with no canvas/animation jank
- [ ] LinkedIn, GitHub, Email accessible from nav on all viewport sizes
- [ ] Only 6 content sections remain (Hero, About, Projects, Skills, Experience, Contact)
- [ ] Dark mode looks professional, not flashy
- [ ] Light mode works correctly with updated components
- [ ] Mobile responsive — social links accessible, cards stack properly
- [ ] Featured projects still visually distinct from regular cards
- [ ] No JS errors in console

## Open Questions

- None — all resolved via discussion

## References

- Current site: `~/portfolio/index.html`
- LinkedIn: https://www.linkedin.com/in/dylan-lankford-411398332/
- GitHub: https://github.com/dylmlank
