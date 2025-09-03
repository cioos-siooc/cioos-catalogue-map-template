# Initialize

Thanks for creating this new catalogue map, to be fully completed the following steps are required:

## Overview

This repository is a thin template to deploy a CIOOS Catalogue Map from configuration and static assets only. At build time, it uses the shared base project (cioos-catalogue-map-base) and produces a static site that is published to GitHub Pages. You customize the resulting map by editing `config.yaml` and by adding logos/images to `public/`.

No application code lives here—deployment is handled entirely by a GitHub Action that pulls the base and builds with your configuration and assets.

## What’s in this repo

- `config.yaml`: The full configuration for your map (catalogue endpoint, map center/zoom, languages, titles/metadata, theme colors, logos, basemaps, favicon, etc.).
- `public/`: Static assets that become available at the site root (e.g., `/Images/...`, `/images/favicon.ico`). Place your logos and any content here.
- `.github/workflows/deploy-map.yaml`: The workflow that builds and publishes the site to GitHub Pages on push.

### How deployment works (GitHub Action)

The workflow `.github/workflows/deploy-map.yaml`:

- Triggers on pushes to `main`.
- Uses GitHub Pages with OIDC (permissions: `id-token: write`, `pages: write`, `contents: read`).
- Deploys via a composite action (`cioos-siooccioos-catalogue-map-base`) that:
	- Consumes your `config.yaml` and files under `public/`.
	- Builds the site using the base project (cioos-catalogue-map-base).
	- Publishes the output to the `github-pages` environment. The final URL is exposed as `steps.deployment.outputs.page_url`.

You don’t need to run a local build. Pushing changes is enough to trigger a new deployment.

## Todos

 - [ ] Update README.md
 - [ ] Update the config.yaml page:
   - [ ] `catalogue_url`: Base URL of a CIOOS CKAN catalogue API to query.
   - [ ] `base_query`: Optional default query/filters applied to catalogue searches.
   - [ ] `metadata` titles: titles in browser search.
   - [ ] `map.center` and `map.zoom`: Initial map view.
   - [ ] `theme`:
     - [ ] `primary_color`: Main color use for interface
     - [ ] `secondary_color`: Accent color used on links/buttons, etc.
   - [ ] `favicon`: Path to your favicon (e.g., `/images/favicon.ico`).
 - [ ] About pages:
   - [ ] [public/about.md](../../../public/about.md)
   - [ ] [public/a-propos.md](../../../public/a-propos.md)
 - [ ] Activate Github Pages
 - [ ] Confirm Catalogue Map is now available.
 - [ ] [optional] Redirect Map to external URL