
# Initialize a new Catalogue Ma
Thanks for creating a new catalogue map. Follow this checklist to configure, deploy, and verify your site.

## Overview

This template deploys a CIOOS Catalogue Map from configuration and static assets only. At build time, it uses a shared base project (cioos-catalogue-map-base) and publishes a static site to GitHub Pages. Customize via `config.yaml` and assets in `public/`.

Deployment is handled by a GitHub Action that builds with your config and assets—no local build required.

## What’s in this repo

- `config.yaml`: Full map configuration (catalogue endpoint, map center/zoom, languages, titles/metadata, theme colors, logos, basemaps, favicon, etc.).
- `public/`: Static assets served from the site root (e.g., `/Images/...`, `/images/favicon.ico`). Add logos and images here.
- `.github/workflows/deploy-map.yaml`: Workflow that builds and publishes to GitHub Pages on push.

## How deployment works (GitHub Action)

Workflow: `.github/workflows/deploy-map.yaml`

- Triggers on pushes to `main` or manual dispatched via [Gitub Action page](../actions/workflows/deploy-map.yaml) (see `Run workflow` button).
- Uses GitHub Pages with OIDC (permissions: `id-token: write`, `pages: write`, `contents: read`).
- Deploys via a composite action (`cioos-siooc/cioos-catalogue-map-base@main`) that:
  - Consumes your `config.yaml` and assets under `public/`.
  - Builds the site using the shared base project.
  - Publishes to the `github-pages` environment. The Pages URL is exposed as `steps.deployment.outputs.page_url`.

You don’t need to run a local build. Pushing changes triggers deployment.

## Pre-configuration info

Please fill in:

- CKAN Catalogue URL (`catalogue_url`): <!-- e.g., https://catalogue.ogsl.ca -->
- Default base query (`base_query`): <!-- optional filter string -->
- Default language (`default_language`): <!-- en or fr -->
- Map center (`map.center`): <!-- [lat, lon] e.g., [66.485, -62.48] -->
- Map zoom (`map.zoom`): <!-- e.g., 5 -->
- Primary color (`theme.primary_color`): <!-- e.g., #3b82f6 -->
- Accent color (`theme.accent_color`): <!-- e.g., #F6AF3B -->
- Favicon path (`favicon`): <!-- e.g., /images/favicon.ico -->
- Main logo(s) (path + mode/lang + alt): <!-- e.g., /Images/Logo.png -->
- Footer/bottom logo(s): <!-- list paths and variants -->

## Checklist

- [ ] Update `README.md` with site-specific details (optional).
- [ ] Edit `config.yaml`:
  - [ ] `catalogue_url` set to your CKAN catalogue base URL.
  - [ ] `base_query` set or left empty as needed.
  - [ ] `default_language` set.
  - [ ] `title` and `metadata` updated in both languages.
  - [ ] `map.center` and `map.zoom` adjusted.
  - [ ] `theme.light`/`theme.dark` background colors set as desired.
  - [ ] `theme.primary_color` and `theme.accent_color` set.
  - [ ] `favicon` path valid under `public/`.
  - [ ] `main_logo` and `bottom_logo` entries complete with correct paths, `mode` (light/dark), optional `lang`, and `alt` text.
  - [ ] `basemaps` list reviewed; tile URLs, attribution, and `checked`/`maxZoom` set.
- [ ] About pages:
  - [ ] Update `public/about.md` (English) if desired.
  - [ ] Update `public/a-propos.md` (French) if desired.
- [ ] Add/verify assets in `public/`:
  - [ ] Logo/image files added under `public/Images/` (or similar).
  - [ ] Paths in `config.yaml` match file names exactly (case-sensitive).
  - [ ] Favicon available at the configured path (e.g., `public/images/favicon.ico`).
- [ ] Pages configuration:
  - [ ] GitHub Pages is enabled under [Settings → Pages](../settings/page): Select `Source = Github Action`
  - [ ] Build and deployment source is set to "GitHub Actions".
- [ ] Branch strategy:
  - [ ] Use `development` for test deployments if desired.
  - [ ] Merge to `main` for production deployments.
- [ ] Push changes to trigger deployment.

## Post-deployment verification

- [ ] Workflow run completed successfully (Actions tab).
- [ ] Pages URL is available and loads without errors.
- [ ] Map loads and shows expected basemap(s).
- [ ] Dataset search returns expected CKAN results.
- [ ] Logos and favicon render correctly (light/dark modes where applicable).
- [ ] Titles, metadata, and language toggles behave as expected.

Deployment URL: <https://cioos-siooc.github.io/cioos-catalogue-map-template>

## Optional: custom domain / redirect

- [ ] Configure a custom domain in Settings → Pages (adds a `CNAME`).
- [ ] Update DNS (A/AAAA or CNAME) to point to GitHub Pages.
- [ ] Verify HTTPS is enabled and the custom URL serves the site.

## Definition of done

- A successful deployment is visible at the expected URL (GitHub Pages or custom domain).
- Core configuration is set (catalogue URL, map center/zoom, theme, logos, basemaps).
- Assets load correctly and links are valid.