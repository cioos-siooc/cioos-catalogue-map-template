# CIOOS Catalogue Map Template

## Overview

This repository is a thin template to deploy a CIOOS Catalogue Map from configuration and static assets only. At build time, it uses the shared base project (cioos-catalogue-map-base) and produces a static site that is published to GitHub Pages. You customize the resulting map by editing `config.yaml` and by adding logos/images to `public/`.

No application code lives here—deployment is handled entirely by a GitHub Action that pulls the base and builds with your configuration and assets.

## What’s in this repo

- `config.yaml`: The full configuration for your map (catalogue endpoint, map center/zoom, languages, titles/metadata, theme colors, logos, basemaps, favicon, etc.).
- `public/`: Static assets that become available at the site root (e.g., `/Images/...`, `/images/favicon.ico`). Place your logos and any content here.
- `.github/workflows/deploy-map.yaml`: The workflow that builds and publishes the site to GitHub Pages on push.

## How deployment works (GitHub Action)

The workflow `.github/workflows/deploy-map.yaml`:

- Triggers on pushes to `main`.
- Uses GitHub Pages with OIDC (permissions: `id-token: write`, `pages: write`, `contents: read`).
- Deploys via a composite action (`cioos-siooccioos-catalogue-map-base`) that:
	- Consumes your `config.yaml` and files under `public/`.
	- Builds the site using the base project (cioos-catalogue-map-base).
	- Publishes the output to the `github-pages` environment. The final URL is exposed as `steps.deployment.outputs.page_url`.

You don’t need to run a local build. Pushing changes is enough to trigger a new deployment.

## Customize your map

1) Configure behavior and UI in `config.yaml`:

- `catalogue_url`: Base URL of a CIOOS CKAN catalogue API to query.
- `base_query`: Optional default query/filters applied to catalogue searches.
- `map.center` and `map.zoom`: Initial map view.
- `default_language`: Initial UI language (`en`/`fr`).
- `title` and `metadata`: Localized site title/description.
- `theme`: Light/dark colors, primary and accent.
- `favicon`: Path to your favicon (e.g., `/images/favicon.ico`).
- `main_logo` and `bottom_logo`: One or more logo entries with `url`, `mode` (light/dark), optional `lang`, `alt`, and optional sizing/class.
- `basemaps`: List of base layers with localized names, tile URL templates, attribution, and options such as `checked`/`maxZoom`.

2) Add assets under `public/`:

- Place images in folders like `public/Images/...` and reference them in `config.yaml` with absolute paths such as `/Images/cioos-national_EN_W.svg-.png`.
- Ensure paths are case-sensitive and exist (e.g., `favicon` defaults to `/images/favicon.ico`; create `public/images/` and put your file there or update the path).

3) Commit and deploy:

- Commit changes to `main` (production). The workflow will build and publish automatically.
- Check the Actions tab for logs and the Pages environment for the live URL.

## Quick start

- Edit `config.yaml` to point to your catalogue and set titles, theme, logos, and basemaps.
- Add your images/logos under `public/` and update paths in `config.yaml`.
- Push to `main` to publish to GitHub Pages.

## Notes and troubleshooting

- If deployment fails, open the failed run under Actions and review the logs from the “Deploy Map” step.
- Logo/favicons not appearing usually means the path is wrong (case-sensitive) or the file isn’t under `public/`.
- To change when deployments happen, edit the `on.push.branches` list in `.github/workflows/deploy-map.yaml`.
