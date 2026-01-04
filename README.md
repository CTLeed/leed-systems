# Leed Systems - Landing Page

Modern landing page built with SvelteKit and deployed to GitHub Pages.

## Development

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

## Deployment to GitHub Pages

This site is configured to automatically deploy to GitHub Pages when you push to the main branch.

### Setup Instructions

1. **Create a new repository on GitHub** (or use an existing one)

2. **Enable GitHub Pages**:
   - Go to your repository Settings
   - Navigate to "Pages" in the left sidebar
   - Under "Build and deployment", set Source to "GitHub Actions"

3. **Push your code**:
   ```bash
   cd sveltekit-site
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/leed-systems.git
   git push -u origin main
   ```

4. **Wait for deployment**: The GitHub Actions workflow will automatically build and deploy your site. Check the "Actions" tab to monitor progress.

5. **Visit your site**: Once deployed, your site will be available at:
   `https://YOUR-USERNAME.github.io/leed-systems/`

## Project Structure

```
sveltekit-site/
├── src/
│   ├── routes/
│   │   ├── +layout.ts          # Enables static prerendering
│   │   └── +page.svelte        # Main landing page
│   ├── app.html                # HTML template
│   └── app.d.ts                # TypeScript declarations
├── static/                     # Static assets (images, favicons)
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Pages deployment
└── svelte.config.js            # SvelteKit configuration
```

## Features

- ⚡ Built with SvelteKit 2
- 🎨 Smooth animations and transitions
- 📱 Fully responsive design
- 🌙 Dark theme with gradient accents
- 🚀 Optimized static build for GitHub Pages
- ♿ Accessible markup and ARIA labels
- 📧 Newsletter signup integration

## Customization

To customize the content:

1. Edit `src/routes/+page.svelte` to change text, links, and structure
2. Update colors in the CSS variables (`:global(:root)` section)
3. Replace images in the `static/` folder
4. Modify meta tags in `src/app.html`

## Notes

- The site uses the repository name in the base path (`/leed-systems`)
- To use a custom domain, update `paths.base` in `svelte.config.js`
- The `.nojekyll` file ensures GitHub Pages serves the site correctly
