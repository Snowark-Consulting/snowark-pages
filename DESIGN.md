---
version: 1.0
name: Snowark
description: Calm, refined, premium. An AI business partner for founders, not an automation vendor. British English. No em dashes.
shipped: 2026-07-07 (snowark.com.au full rebuild)
supersedes: alpha
colors:
  primary: "#174445"
  secondary: "#0d8f64"
  neutral: "#e9e7e3"
  on-primary: "#FFFFFF"
  on-neutral: "#000000"
  # v1.0 additions: the dark-section family
  ink: "#0b2021"
  ink-2: "#0e2628"
  teal-lift: "#2a7f80"
  teal-soft: "#7fb8b9"
  green-lift: "#4cc296"
  heading-ink: "#0e2929"
typography:
  h1: { fontFamily: Cambo, fontSize: clamp(2.6rem, 6.5vw, 4.6rem), fontWeight: 400, lineHeight: 1.08 }
  h2: { fontFamily: Cambo, fontSize: clamp(1.9rem, 4vw, 2.9rem), fontWeight: 400, lineHeight: 1.15 }
  h3: { fontFamily: Lato, fontSize: 1.15rem, fontWeight: 700, lineHeight: 1.4 }
  body: { fontFamily: Lato, fontSize: 1rem, fontWeight: 400, lineHeight: 1.7 }
  eyebrow: { fontFamily: Lato, fontSize: 0.72rem, fontWeight: 700, letterSpacing: 0.16em, textTransform: uppercase }
  stat: { fontFamily: Cambo, fontWeight: 400, note: "All display numerals are Cambo. JetBrains Mono retired from the site; reserve for dashboards only." }
---

# Snowark Design System v1.0

The alpha doc described intent. This version records what shipped at
snowark.com.au on 2026-07-07 and the rules learned building it.

## 1. Theme architecture: light site, teal feature moments

The site is light (white + warm gray `#edebe6` alternation, near-teal-ink
text `#0e2929` headings). Three **dark feature sections** carry the drama:
hero, proof ("placements"), and footer. These use the `.dark-sec` pattern:

- Background: `linear-gradient(180deg, #0d2d2e 0%, #123a3b 50%, #0d2d2e 100%)`
- Text: mist `rgba(233,231,227,0.78)`, headings white, eyebrows `green-lift`
- Dark glass cards: `linear-gradient(180deg, rgba(38,68,69,0.42), rgba(17,38,39,0.42))` + 1px `rgba(233,231,227,0.12)` border

Dark feature **cards** may also appear inside light sections (the problem
journey tiles): `linear-gradient(180deg, #12393a, #0c2829)`, deep shadow
`0 14px 40px rgba(14,41,41,0.24)`.

Green remains the sole accent, used softened on dark (`#4cc296`) and full
(`#0d8f64`) on light. One accent moment per section still holds.

## 2. Motion system (new)

**Stack:** GSAP 3.12 + ScrollTrigger + MotionPathPlugin (cdnjs), SMIL for
self-contained SVG loops, CSS keyframes for ambient motion (aurora drift,
marquees, shimmer, orbits). No video, no canvas, no Lottie.

**Principles (hard rules):**
- Animate transform and opacity only. Never animate `filter` or
  `backdrop-filter` on large surfaces; dim with an opacity overlay div.
- True `backdrop-filter` blur only on small overlay elements (nav, badges,
  captions, buttons). Large cards fake glass with tinted gradient fills.
- All scrubbed ScrollTriggers use `scrub: 0.5-0.7`, never `true`. Pins get
  `anticipatePin: 1`.
- Scroll reveals: `gsap.set` hidden once up-front, then animate TO visible.
  Never `gsap.from` on already-painted content (causes flicker).
- Create ScrollTriggers in page order. Never measure `position: sticky`
  elements as triggers; use their non-sticky container.
- Content must be fully visible with JS disabled. `prefers-reduced-motion`
  disables all animation including SMIL layers (hide the SVG group).
- Paths that dots travel on are anchored INSIDE their start/end cards so
  travellers emerge from behind surfaces, never pop in at a line's tip.

**Signature moves (catalogue of shipped patterns):**
1. **Living network hero.** SVG grid lines + curved paths + nodes. Data
   pulses travel the paths (SMIL animateMotion, staggered, 6-10s), nodes
   emit expanding ping rings, per-node sine drift (GSAP), and the two node
   layers parallax toward the cursor at different depths (quickTo, 27/50px).
2. **Aurora.** 3 radial-gradient blobs (soft green 0.25 / teal 0.5 /
   warm mist 0.13), `blur(80px)`, 26-38s alternate drift keyframes.
3. **Pinned word reveal.** Statement pinned 140vh; words fade 0.1 to 1 on
   scrub; closing line in green.
4. **Problem journey.** Animation-only dark tiles zigzag with text in the
   whitespace; one dashed SVG path connects tile centres (rebuilt from DOM
   on resize); three dots ride it via MotionPathPlugin, scrubbed to scroll.
5. **Scene tiles.** Each journey tile holds a bespoke SMIL scene matched to
   its message (swaying disconnected nodes + pulsing hub, racing clock,
   dots falling through a cracked ledge, diverging trajectories).
6. **Plan stack.** Sticky stacking cards; one container-scrubbed timeline
   drives segment fills (01/02/03 strip) + previous-card recede (scale +
   opacity overlay); strip fades out when complete.
7. **Card fan.** Portrait cards fanned by slot params
   (`rot ±7.5k, x ±96k, y 9k², scale 1-0.05k`, origin `50% 150%`); front
   card auto-deals to the back every 3.4s; click any card to bring it to
   front; hover pauses. Each card carries a line-art motif drawn on loop
   (`pathLength=100`, dasharray 24/76, 4.2s) MATCHED to its content
   (waveform, envelope, wireframe, check, chart, redaction bars).
8. **Marquee.** Duplicate-track translateX loop, edge mask fades, two rows
   opposing directions. Pills = white, real brand logos.
9. **Orb + orbits.** CSS-shaded sphere (radial gradients + inset shadows,
   masked icon), orbiting agent chips counter-rotated to stay upright,
   wider slow orbit for anonymous "shadow" chips.
10. **Workday feed.** Vertical duplicate-track ticker of timestamped agent
    actions, edge-masked.
11. **Try-and-fail dots.** Ledger rows: a dot rides the divider line ~60%
    of the way, then drops and fades. Staggered per row.
12. **Footer shimmer.** Brand lockup as CSS `mask-image`, animated gradient
    (teal-soft, mist, green-lift) sweeping through it.
13. **Buttons.** Primary = dark glass fill + gradient light border
    (padding-box/border-box trick) + periodic shine sweep + hover lift.
    Green is never a fill.

## 3. Components added in v1.0

- **Glass caption** (on dark imagery): blur-only glass. No dark fill.
  `rgba(255,255,255,0.08→0.03)` padding-box + gradient light border
  brightest at top (`rgba(255,255,255,0.5)→0.08`) border-box, blur(18px).
- **Verdict chip:** Lato 700 caps 0.66rem, green text/border tint, pill.
- **Agent chip:** avatar (42px, teal ring + glow) + name caps + brand small.
  Shadow variant: silhouette SVG, 0.55 opacity, labels "Classified",
  "Under NDA", "Confidential".
- **Tool pill:** white pill, hairline border, soft shadow, 18px brand logo.
- **Stat card (cred):** Cambo numeral + caps label, white card. Grid MUST
  use `minmax(0, 1fr)` tracks (nowrap numerals otherwise burst the grid on
  mobile).
- **Feed item:** white row card, Lato-bold green timestamp + action.
- **Plan progress strip:** sticky caps labels over hairlines, green fill
  bars, solid section-colour backdrop, fades when the stack completes.

## 4. Iconography and asset sourcing

- Favicon = circular icon mark only (16/32/180/ico generated from
  `assets/logos/Snowark-Icon.png`).
- Tool logos: prefer Simple Icons CDN (SVG), then Iconify `logos/`
  collection, then Google favicon service at 64px (last resort). Commit
  files into `assets/tools/`; never hotlink at runtime.
- Agent avatars live in `assets/profile/` (gary, winston, atlas).

## 5. Voice additions

- **The placement metaphor.** Agents are team members we "place", not
  software we install. Section language: "Recent placements", "performance
  reviews", "Place my agent" (primary CTA). Confidential engagements are
  the running wink; keep it dry and brief.
- **Claims accuracy.** Every client gets their own agent. Never claim the
  AFS agent itself deploys for clients; what transfers is the
  infrastructure and playbook. No outbound links to personal portfolios.
- **Stat presentation.** Growth stats as `$X → $Y` arrows in Cambo with a
  caps label. The playbook narrative (portfolio exposure, then brand one,
  two, three) beats raw platform counts. Metrics never appear without
  context that explains why they matter.

## 6. Engineering guardrails (learned the hard way)

- `grep -c '—' index.html` must return 0 before every deploy.
- The `header` element selector styles the site nav; any component using a
  `<header>` tag must reset position/height/padding or it inherits fixed
  positioning.
- The Supabase contact form posts to `client_responses` with legacy field
  mapping (email lands in `custom_personality`, company in `customers`).
  Preserve exactly unless the table is migrated.
- Microsoft Clarity `xg3xx2k35l` ships on every page.
- Preserve on rebuilds: `dash/`, `notforgotten-onboarding/`, `proposals/`,
  `cao-reference.html`, `josh-resume.html`.
- Deploy: push to `main`, Vercel serves in ~7s. Verify live with a grep
  for a new-build marker, then a browser pass.

## 7. Content depth pass (shipped 2026-07-08)

- **Voice update: contractions are now standard in body copy** (don't,
  you're, it's). Headings, eyebrows, and stat labels keep the formal
  register. This was a deliberate approachability decision (Quirk Digital
  reference); do not revert to uncontracted forms on rebuilds.
- **Founder-forward framing:** first person throughout ("Hi, I'm Josh"),
  CMO in past tense, "six months ago", full-time Snowark. Positioning:
  one operator plus his agents. Never "one client at a time" (false).
- **Three returns** (Time / Cost / Growth) is the value-prop spine; admin
  relief is one third of the story, never the whole story.
- New sections: #returns, #fit, #faq, #note (letter). All use .reveal,
  .glass, .spot (cursor spotlight). Returns cards carry .ret-art line art
  in the fan-card DNA (art-bg + art-draw dash).
- Fan ARTS array in the JS is index-matched to fan-card DOM order: when
  adding/removing a card, add/remove its art path at the same index.
- FAQ is a GSAP accordion (one open at a time), numbered via CSS counter.
- Preview gotcha: footer-mark's SVG mask will NOT render from file://
  (Chrome blocks it). Preview with `python3 -m http.server` instead.
