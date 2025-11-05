# AI Agent Instructions for perezjmp3.github.io

## Project Overview
This is a personal portfolio website built with vanilla HTML/CSS/JavaScript, focusing on simplicity and performance. The site is hosted via GitHub Pages.

## Architecture
- Single-page static website with minimal dependencies
- Inline CSS for fast loading and reduced HTTP requests
- Vanilla JavaScript for basic functionality
- Responsive design using system font stack and flexbox layouts

## Key Files and Their Purpose
- `index.html`: Main entry point containing all site content, styles, and scripts
- `CNAME`: GitHub Pages custom domain configuration
- `README.md`: Basic project documentation

## Code Conventions
1. **CSS Styling**
   - Styles are inlined in `<style>` tag for performance
   - Uses system font stack for optimal loading
   - Mobile-first responsive design using flexbox
   - Class naming follows functional purpose (e.g., `.btn` for button styles)

2. **JavaScript**
   - Minimal JavaScript usage, primarily for dynamic year in footer
   - Direct DOM manipulation without frameworks

## Development Workflow
1. **Local Development**
   - Site can be tested locally using any static file server
   - No build process required

2. **Deployment**
   - Automatically deploys to GitHub Pages on push to main branch
   - Custom domain configuration through CNAME file

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