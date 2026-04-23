# Portfolio Enhancements — Design Spec
**Date:** 2026-04-23

## Scope
Ten targeted improvements to `index.html` (single-file portfolio). No new files beyond texture assets.

## Features

### 1. Remove floating 3D cubes
Delete `_cubeData`, `_floatCubes` array, the forEach that creates them, and the animation block inside the render loop. No replacement.

### 2. Random music playlist from `musics/`
- Playlist: `['musics/music.mp3','musics/music2.mp3','musics/music3.mp3','musics/music4.mp3','musics/music5.mp3']`
- Fisher-Yates shuffle on page load
- `audio.src` set to current track; `audio.addEventListener('ended', playNext)`
- `playNext()` advances index, wraps around, reshuffles on full loop
- Add `⏭` skip button (`.music-btn`) next to play/pause in existing player UI
- Remove `<audio src="music.mp3" loop>` loop attribute

### 3. i18n PT-BR / EN
- `TRANSLATIONS` object with keys for every UI string in both `pt` and `en`
- `data-i18n="key"` attributes on all text elements in HTML
- `applyLocale(lang)` iterates all `[data-i18n]` elements and sets `textContent`/`innerHTML`
- Toggle button `[PT|EN]` in nav HUD between logo and nav-buttons
- Default: `pt`; persisted in `localStorage`

### 4. Tech icons via Simple Icons
- Load Simple Icons SVG via CDN url pattern: `https://cdn.simpleicons.org/{slug}/{color}`
- Each `tech-badge` gets `<img class="tech-icon">` prepended (16x16, filter for color tinting)
- Map of tech name → Simple Icons slug defined in JS
- Techs without Simple Icons icon (REST API, Pentest, SQL Injection) use a generic terminal icon SVG inline

### 5. Contact info correction
- Email: `gabrielinaciodev47@gmail.com`
- GitHub: `https://github.com/GabrielInacior`
- LinkedIn: `https://www.linkedin.com/in/gabriel-inácio-rodrigues-silva-072029220/`

### 6. Ground texture — procedural CanvasTexture
- Create 512×512 canvas with dark purple base (`#0f0818`)
- Draw grid lines in neon pink/purple
- Add subtle noise dots for grunge
- Apply as `map` on `MeshStandardMaterial` with `repeat(20,20)` for tiling
- Keep existing `gridHelper` on top

### 7. Dynamic directional moonlight
- In animation loop: `moonLight.position.x = Math.cos(elapsed * 0.05) * 40`; `moonLight.position.z = Math.sin(elapsed * 0.05) * 40`
- `moonLight.intensity` oscillates between 0.8 and 1.6 with `sin`
- Color shifts subtly between blue-purple and warm amber over a long cycle (~120s)

### 8. Projects panel improvements
- Each project entry gets: status badge (`PRODUÇÃO`/`EM DEV`/`ARQUIVADO`), optional GitHub link, optional Live link
- Tech tags get Simple Icons icons matching tech
- Entry layout: number | body (title + desc + tags + links) | status badge
- Links styled as small `[↗ GITHUB]` / `[▶ LIVE]` buttons, neon bordered
- Animate: each entry slides in from left with stagger (same as current), but title also gets scramble effect

### 9. Typewriter sequential fix (About)
- Calculate duration each paragraph will take: `txt.length * speed` ms
- Call `typewriter(p[i+1], delay + p[i].length * speed + buffer)` sequentially
- Each paragraph starts only after the previous has fully typed

### 10. Stack: links + holographic 3D logo on hover
- Each `tech-badge` wrapped in `<a href="{docs_url}" target="_blank">`
- URLs map defined in JS (same map as icons)
- `.hologram-panel` fixed-position div (240×180px), hidden by default
- Contains a `<canvas class="hologram-canvas">` (Three.js sub-scene)
- Sub-scene: `TextGeometry` with the tech name, `MeshBasicMaterial` emissive cyan, rotating on Y axis
- Scanline overlay div inside the panel for VHS effect
- On `mouseenter` of badge: position panel near cursor, init or show canvas, start rotation
- On `mouseleave`: hide panel, stop render loop
- Fallback for mobile: no hologram (touch devices skip)
