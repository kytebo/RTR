# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Directory Is

This is a client materials directory for Rent the Runway (RTR), a Quantum Rise client opportunity. It is **not a software development project** — there is no build system, package manager, or test suite.

## Contents

- `RTR Customer _ AI Walkthrough _offline_.html` — A self-contained offline presentation bundling all assets (images, scripts, styles) as base64-encoded data embedded directly in the HTML. At runtime, the bundler script decodes and decompresses these assets into blob URLs and swaps the document root via `DOMParser`.
- `Images/` — Source images (Unsplash photos) used in the presentation.

## Working With the HTML File

**To view:** Open the HTML file directly in a browser (no server needed — it's fully offline-capable).

**To edit content:** The file is a bundled export. Editing raw content inside the base64 blobs is impractical. The canonical workflow is to edit the source presentation and re-export as an offline bundle.

**Bundler mechanics:** The HTML contains two `<script>` tags with custom MIME types:
- `type="__bundler/manifest"` — JSON map of UUID → `{data, mime, compressed}` for each asset
- `type="__bundler/template"` — The HTML template with UUID placeholders where asset URLs should go

The bootstrap script decodes/decompresses assets, creates blob URLs, substitutes UUIDs in the template, and replaces `document.documentElement` with the parsed result. Scripts are re-created via `createElement` to trigger execution (DOMParser-inserted scripts are inert per spec).

**Image slots:** The file has a post-render injection that adds placeholder image slots to observation slides and framework quadrant cards. Look for `.panel.obs.has-image` and the injected `<style>` block near line 144 to find where images are wired in.
