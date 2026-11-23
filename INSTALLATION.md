# Apply the Zensical documentation theme

1. Copy `mkdocs.yml` into the repository root.
2. Copy `.github/workflows/docs.yml` into the matching workflow directory.
3. Replace the existing `docs/index.md` with the supplied version.
4. Copy `docs/assets/stylesheets/extra.css` into the matching directory.
5. Remove the old root `_config.yml` so the repository no longer advertises the retired Jekyll theme.
6. In GitHub, open **Settings → Pages** and set **Build and deployment → Source** to **GitHub Actions**.
7. Commit the files to `main`. The workflow will build and deploy the site.

The existing numbered Markdown documents remain in `docs/` and are linked by the new navigation.

The root-level `index.md` is not used by this configuration because Zensical's documentation source directory is `docs/`. It can be kept as a GitHub-facing README-style page or removed later.

Official Zensical deployment guide: https://zensical.org/docs/publish-your-site/
