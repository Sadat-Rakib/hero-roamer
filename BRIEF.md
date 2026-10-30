# Roamer Header

A full-viewport travel-brand hero header for "ROAMER" featuring a fullscreen background video overlaid with an interactive WebGL fluid-smoke cursor simulation that liquify-warps the video pixels and injects iridescent lime/gold/cyan chromatic smoke behind the pointer.

## Tech stack
- React (client component, uses hooks: useEffect, useRef, useState).
- framer-motion — entrance animations for navbar, glass card, headline block, footer, and the mobile drawer (spring slide-in + AnimatePresence).
- lucide-react — icons: Tent, ArrowUpRight, Compass, Menu, X.
- Raw WebGL (canvas getContext('webgl')) — custom GLSL fluid solver: velocity sim + dye advection + composite render. No extra library.
- Tailwind utility classes are used only inside the mobile drawer; the rest is a scoped injected <style> block.

## Fonts & global styles
- Import: `@import url('https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Parkinsans:wght@300;400;500;600;700&display=swap');`
- Primary font throughout: 'Parkinsans', sans-serif. (Dancing Script is imported but reserve it only if a script accent is wanted.)
- Base text color: #FEFFFF. Root background: #030712.
- All styling is scoped under `.roamer-root` — never set global body/html rules.

## Section container
- `.roamer-root`: position relative, width 100%, height 100vh, min-height 100vh, overflow hidden, background #030712, font-family Parkinsans, color #FEFFFF.
- Stacking layers by z-index: video (z 0, position absolute inset 0, object-fit cover, pointer-events none) → WebGL canvas wrap (z 1, absolute inset 0, pointer-events none) → content overlay (z 10, relative, flex column, justify-content space-between, padding 28px 48px 24px 48px).

## Structure, section by section
1. **Background video** — <video> autoPlay loop muted playsInline crossOrigin=anonymous, src `https://cdn.jiro.build/Jahid/Test/Panda%20with%20nature.mp4`, class covers the whole root.
2. **WebGL canvas** — full-cover canvas that reads the video as a texture and displaces its pixels with the fluid velocity field around the cursor, adding chromatic-fringed smoke.
3. **Top navbar** (`.roamer-nav`, flex justify-between): left brand `ROAMER` (Parkinsans 500, 26px, letter-spacing 0.32em, uppercase, hover opacity 0.85). Center nav (gap 20px) three text buttons — Destinations, Experiences, Stories (Parkinsans 400, 15px, color rgba(254,255,255,0.85)) separated by 36px×1px dividers rgba(254,255,255,0.35); each label uses a dual-layer top-to-bottom rolling text effect on hover. Right: desktop CTA "Plan Your Trip" pill (background #FEFFFF, color #0A0D14, radius 8px, padding 6px 6px 6px 18px, box-shadow 0 4px 18px rgba(0,0,0,0.25)) containing a 26×26 dark icon box (#0A0D14, radius 5px) holding an ArrowUpRight; hover lifts translateY(-2px), background #E2E8F0. A mobile Menu button (42×42, rgba(9,17,32,0.7), 1px border rgba(255,255,255,0.15), radius 8px, backdrop-blur 12px) is hidden ≥1025px, shown ≤1024px.
4. **Floating glass card** (desktop only, hidden ≤1024px) — `.roamer-glass-card` liquid-glass: max-width 486px, radius 14px, padding 16px, backdrop-blur 4px, background rgba(255,255,255,0.01), with a masked gradient border via ::before (linear-gradient 180deg white 0.28→0.08→0→0.08→0.28). Inside: 2-column grid (gap 13px) of two subcards — thumbnail (height 116px, radius 8px, hover image scale 1.05) + a label button (rgba(26,40,66,0.88), radius 6px, 13px). Card 1 = "Stargazing Camps" (img `https://images.unsplash.com/photo-1510312305653-8ed496efae75?auto=format&fit=crop&w=600&q=80`); Card 2 = "Mountain Getaways" (img `https://images.unsplash.com/photo-1464822759023-fed622ff2c3b?auto=format&fit=crop&w=600&q=80`). Below the grid: description "Explore breathtaking places, from starry skies to serene horizons. Journeys made for dreamers." (Parkinsans 300, 16px, line-height 1.55, rgba(254,255,255,0.9)).
5. **Bottom hero block** (`.roamer-bottom-section`, margin-top auto): two-line headline `.roamer-main-headline` (Parkinsans 400, 68px, line-height 1.08, letter-spacing -0.02em, text-shadow 0 4px 28px rgba(0,0,0,0.6)) — line 1 "Explore —", line 2 "Discover — Repeat" — animated with a word-safe letter-by-letter blur-in. To the right, description `.roamer-bottom-desc` (max-width 580px, Parkinsans 300, 20px, line-height 1.55): "Discover stunning destinations, plan unforgettable road trips, and create stories that last a lifetime." plus a sub-line (hidden ≤1024px) "From starry nights to mountain escapes, travel made for dreamers like you."
6. **Footer metric bar** (`.roamer-footer-bar`, flex justify-between, padding-top 14px, border-top 1px rgba(255,255,255,0.18)): left Compass 17px + "1000+ Destinations Mapped"; right Tent 17px + "500+ Happy Travelers" (Parkinsans 300, 16px, rgba(254,255,255,0.88), letter-spacing 0.02em).
7. **Mobile drawer** (AnimatePresence, ≤1024px): black/65 backdrop-blur backdrop + right slide-in sheet (w 90vw, max 380px, bg #091120/95, backdrop-blur-2xl, left border white/15). Contains ROAMER header + X close, the three nav links, a "Featured Escapes" 2-card grid, a description, and a full-width "Plan Your Trip" CTA plus a mini metric row.

## Assets
- Video: `https://cdn.jiro.build/Jahid/Test/Panda%20with%20nature.mp4`
- Stargazing thumb: `https://images.unsplash.com/photo-1510312305653-8ed496efae75?auto=format&fit=crop&w=600&q=80`
- Mountain thumb: `https://images.unsplash.com/photo-1464822759023-fed622ff2c3b?auto=format&fit=crop&w=600&q=80`

## Animations
- Navbar: initial y -45 opacity 0 → y 0 opacity 1, duration 0.85, ease cubic-bezier(0.16,1,0.3,1).
- Glass card: initial x -70 opacity 0 → 0, duration 0.9, delay 0.22, same ease.
- Bottom block: initial y 55 opacity 0 → 0, duration 0.95, delay 0.35, same ease.
- Footer: initial y 15 opacity 0 → 0, duration 0.7, delay 0.85, same ease.
- Mobile drawer: backdrop fade 0.25s; sheet spring x '100%'→0, damping 28, stiffness 260.
- **Word-safe blur-in headline/paragraph:** each character animates initial {opacity:0, filter:'blur(10px)', x:-14} → {opacity:1, filter:'blur(0px)', x:0}, duration 0.65, ease cubic-bezier(0.16,1,0.3,1), delayed per-char by baseDelay + charIndex*stagger. Words are kept in inline-block spans (mr 0.28em) so words never break mid-word while chars stagger.
- **Dual-layer rolling text on hover:** two stacked copies of the label; main starts translateY(0), top copy starts translateY(-100%). On hover the container slides both down: main → translateY(100%), top → translateY(0). Transition transform 0.42s cubic-bezier(0.22,1,0.36,1):
```css
.roamer-roll-box { display:inline-flex; position:relative; overflow:hidden; height:1.3em; line-height:1.3em; }
.roamer-roll-item { display:inline-block; white-space:nowrap; transition: transform 0.42s cubic-bezier(0.22,1,0.36,1); }
.roamer-roll-top { position:absolute; top:0; left:0; right:0; transform:translateY(-100%); }
.parent:hover .roamer-roll-main { transform:translateY(100%); }
.parent:hover .roamer-roll-top { transform:translateY(0); }
```
- **WebGL fluid smoke (signature effect):** a 256×256 ping-pong simulation. Pass 1 velocity: advect + 4-neighbor diffusion, ×0.94 decay, inject cursor velocity into a soft smoothstep(0.09,0) radius. Pass 2 dye: advect prev dye ×0.978 decay, inject fbm-noised iridescent color in a pow-falloff radius with sin swirl. Pass 3 render: warp the cover-fit video sample by the velocity field + ripple vector with per-channel chromatic offsets, blend fbm smoke wisps in lime/gold/cyan and add fold highlights. Injected color cycles over time between lime [0.42,0.86,0.52], gold [0.94,0.78,0.35], cyan [0.38,0.82,0.88]. Cursor position tracked in normalized coords with velocity decay ×0.45/frame and active decay ×0.88/frame.

## Responsive behavior
- ≤1024px: hide floating glass card, hide desktop CTA, hide center nav, show Menu button; overlay padding 22px 24px 18px; bottom section becomes flex:1 space-between; sub-description hidden.
- 641–1024px (tablet): bottom row stacks column, max-width 500px; headline 48px; description 440px / 16px.
- ≤640px (mobile): overlay padding 18px 16px 14px; brand 20px / 0.22em; headline 36px, headline lines allowed to wrap; footer bar stacks column; description 100% / 16px.

## Key design principles
- Dark cinematic travel aesthetic: white type on video, thin dividers, generous letter-spacing.
- Signature interaction is the cursor fluid-smoke that liquifies the background video with chromatic fringing.
- Micro-interactions everywhere: rolling text on hover, lift on CTA, image zoom on subcards.
- Word-safe staggered blur-in for editorial headline reveal.
- Glassmorphism with a masked gradient hairline border, no hard shadows.

## Common mistakes to avoid
- Do NOT use styled-jsx or backtick template literals; keep the CSS in a joined string array inside a <style> tag.
- Ensure the WebGL cleanup deletes textures, framebuffers, buffers, and programs on unmount.
- Cover-fit the video texture inside the shader (aspect correction) so it never stretches.
- Keep the canvas and video pointer-events none so overlay buttons stay clickable.
- Fall back gracefully (console warn) if WebGL is unavailable.

## Page title
ROAMER — Explore, Discover, Repeat

## Integration (build-safety — do not skip)
- Add this section as a **new** component file with a unique name. Don't edit or overwrite any existing file except to add its import and render it.
- Render it **after** all existing sections; keep every previously built section exactly as-is — never replace or remove them.
- If no project exists, create a minimal React + Tailwind app; if one exists, use it as-is — don't re-scaffold or change the Tailwind/build config or version.
- Keep it self-contained: scope its fonts and any resets to this section; never set global `body`/`html`/`*` styles or a global font.
- Install only the libraries this section names.