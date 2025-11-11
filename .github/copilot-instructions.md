# AI Agent Instructions for perezjmp3.github.io

## Project Overview
This is a personal portfolio website built with vanilla HTML/CSS/JavaScript and Three.js for 3D visualization. The site focuses on performance while providing interactive 3D capabilities. The site is hosted via GitHub Pages.

## Architecture
- Static website with modular components
- Three.js integration for 3D graphics
- ES6 modules for JavaScript organization
- Inline CSS for fast loading and reduced HTTP requests
- Responsive design using system font stack and flexbox layouts

## Key Files and Their Purpose
- `index.html`: Main entry point containing site content and navigation
- `3d-environment.html`: Interactive 3D environment using Three.js
- `lib/`: External libraries and dependencies
  - `three/`: Three.js modules (three.module.js, OrbitControls.js)
  - `lil-gui/`: GUI controls for 3D environment
- `CNAME`: GitHub Pages custom domain configuration
- `README.md`: Basic project documentation

## 3D Environment – Current Capabilities and Defaults
- Physics: Cannon.js with SAP broadphase, tuned solver, and shared contact materials
- Lighting: Directional light plus a hanging lamp (chain, shade, emissive bulb, SpotLight with shadows)
- Ground: 30×30 wireframe plane; axes and labels on floor
- Transparency: MeshPhysicalMaterial with transmission, IOR, thickness, attenuationDistance, clearcoat, and clearcoatRoughness; double-sided rendering for proper see-through behavior
- D6 pips: Real geometric dimples created by vertex displacement on a highly subdivided BoxGeometry (100×100×100). Pip spacing/orientation matches textures; smoothstep falloff for clean edges
- D4/D8/D12/D20 labels: Canvas textures on planes oriented to face normals with optional underline for 6 and 9 on d12/d20

### Style Presets (visual defaults)
Sliders auto-sync to preset values when a style is selected.
- ClassicWhite
   - transmission: 0.0, ior: 1.5, thickness: 0.5, attenuationDistance: 0.8, clearcoat: 0.0, clearcoatRoughness: 0.0
- CasinoRed / MidnightBlue / Emerald
   - transmission: 0.82, ior: 1.3, thickness: 0.5, attenuationDistance: 0.8, clearcoat: 0.1, clearcoatRoughness: 0.2
- Bone
   - transmission: 0.0, ior: 1.5, thickness: 0.5, attenuationDistance: 1.0, clearcoat: 0.0, clearcoatRoughness: 0.0

### GUI Behavior
- Style preset switch resets the physical sliders to the preset and refreshes their UI
- Sliders: transmission, ior, thickness, attenuationDistance, clearcoat, clearcoatRoughness, pipBGDarken
- Debug defaults: Show Center of Mass = off; Physics Wireframe = off; Wireframe ground visible through transparent dice

### Implementation Notes for Contributors
- Use `activeStyle` for preset values; sliders write to `state` and override materials at runtime
- For non-d6 dice, material fallbacks now read clearcoat/clearcoatRoughness from `activeStyle`
- For d6, per-face materials include `transmissionMap` so pips remain opaque while faces transmit light
- Transparent materials: `side: THREE.DoubleSide`; d6 keeps `depthWrite: true` to avoid sorting artifacts

### Performance Considerations
- D6 uses high subdivision only for the geometry with dimples; other dice remain lighter
- Prefer modifying material parameters over rebuilding geometry when possible

## Physics modes: simple vs complex
- Broadphase/solver: Uses Cannon.js SAP broadphase with tuned contact equation stiffness/relaxation and shared materials for consistent friction/restitution.
- Toggle: Debug UI exposes "Simple Physics" (state.useSimplePhysics). Changing it rebuilds physics shapes.
- Simple physics mode (default: on)
   - d6: CANNON.Box(0.5, 0.5, 0.5) for speed and stability.
   - d4/d8/d12/d20: Convex hull built from original unbeveled geometry for fast, solid contacts.
   - Vertex dedup precision: 5 (more rounding) to keep hulls compact.
- Complex physics mode (Simple Physics off)
   - Convex hull derived from the beveled visual geometry for more accurate edge/rounding contacts.
   - Vertex dedup precision: 4 (slightly finer) to keep face count reasonable.
   - Fallback: If convex hull creation fails, falls back to Trimesh of beveled geometry (exact shape but limited dynamic–dynamic collisions; die–die collisions may not register). Used as a last resort to visually match shapes.
- Sleep tuning: Bodies use a slightly higher sleepSpeedLimit to settle faster with beveled contact patches.

Defaults
- useSimplePhysics: true
- friction/restitution: set via shared dice/ground ContactMaterials in scene setup

## Bevel and chamfer system
- Scope
   - d4, d8, d12, d20 support beveled edges; d6 is excluded (it uses a dedicated high-subdivision cube with physical pip dimples instead).
- Controls (GUI: Bevel Debug Sliders; can be hidden via state.showBevelSliders)
   - bevelAmount (0.0–1.0): 1.0 = sharp edges; 0.0 = fully rounded toward spherical.
   - rectangleCount (1–64): number of rectangular bands per edge for curved chamfers.
   - chamferPeakHeight (0.0–2.0): multiplier for arc height; 1.0 reaches the original edge locus.
   - edgeExtend (0.0–0.3): extends beveled strips past endpoints so neighboring chamfers meet cleanly at corners.
   - taperAmount (0.0–1.0): controls tapering at very ends; 1.0 = no taper, 0.0 = fully collapsed.
   - taperStart (0.0–0.5): normalized distance from each edge end where tapering finishes.
- Behavior by die type
   - d12: Curved chamfer along pentagon edges using rectangleCount bands; parameters also support end tapering for cleaner corner transitions.
   - d4/d8/d20: Flat chamfer (single segment) suitable for purely triangular convex polyhedra.
   - d6: No bevel pass; retains crisp cube edges combined with physical pip dimples.
- Implementation details
   - Bevel changes trigger a geometry rebuild; labels are generated from the original, unbeveled geometry to maintain proper alignment and readability.
   - Physics shape selection honors the Simple/Complex physics toggle: original geometry hulls for Simple; beveled geometry hulls for Complex.
   - Precision in convex hull construction is relaxed slightly for beveled meshes to keep vertex counts reasonable.

## Code Conventions
1. **CSS Styling**
   - Styles are inlined in `<style>` tag for performance
   - Uses system font stack for optimal loading
   - Mobile-first responsive design using flexbox
   - Class naming follows functional purpose (e.g., `.btn` for button styles)

2. **JavaScript**
   - ES6 modules for Three.js and related libraries
   - Import maps for module resolution
   - Object-oriented patterns for 3D scene management
   - Direct DOM manipulation for basic UI elements

3. **Three.js Implementation**
   - Scene setup in module-scoped context
   - Responsive canvas sizing
   - GUI controls for interactive parameters
   - Animation loop with requestAnimationFrame

## Development Workflow
1. **Local Development**
   - Use VS Code with Live Server extension for development
   - Debug configuration set up for Chrome debugging
   - No build process required
   - Live reloading for instant feedback

2. **Deployment**
   - Automatically deploys to GitHub Pages on push to main branch
   - Custom domain configuration through CNAME file
   - Relative paths ensure compatibility between local and production

## Testing
- Manual testing of responsive design using browser dev tools
- Visual inspection of layout on different devices/viewports

## Common Tasks
1. **Adding New Pages**
   - Create new .html files in root directory
   - Update navigation links in `index.html`
   - Follow existing HTML structure and style conventions

2. **Updating Styles**
   - Modify inline styles in `<style>` tag of `index.html`
   - Maintain mobile-first approach with flexbox layouts

3. **Content Updates**
   - Edit HTML directly in relevant files
   - Ensure responsive layout is maintained
   - Test changes across different viewport sizes