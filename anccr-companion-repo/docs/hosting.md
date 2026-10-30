# Hosting on GitHub Pages

The site is one file. If you see a page that shows only the repository name and a rule, Pages has not found `index.html` at the site root. Check these in order.

1. **Files at the root.** `index.html`, `manifest.webmanifest`, `assets/` and `.nojekyll` must sit at the top level of the repository, not inside an `anccr-companion/` folder. If they are in a folder, move them up (or let the workflow find them — it checks both places).
2. **Source set to GitHub Actions.** Settings → Pages → Build and deployment → Source: *GitHub Actions*. The workflow in `.github/workflows/pages.yml` publishes the folder that holds `index.html`.
3. **`.nojekyll` present.** It stops Pages from running Jekyll over the repository, which is what produces the empty themed page.
4. **Workflow green.** Actions tab → "Deploy to GitHub Pages" → latest run. The "Locate index.html" step prints which folder it published.
5. **Hard refresh.** Pages caches aggressively; use a private window or append `?v=2` to the URL once.

Expected URL: `https://<user>.github.io/<repo>/`
