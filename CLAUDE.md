# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A bilingual (English/Spanish) personal blog built with Hugo and the PaperMod theme. Published at https://javierbarbaran.com/. Spanish is the default language; both languages live under `content/es/` and `content/en/` respectively, with the site serving each under `/es/` and `/en/` URL prefixes.

## Commands

```bash
# Start dev server (drafts hidden by default)
hugo server

# Start dev server including draft posts
hugo server -D

# Build the site (outputs to public/)
hugo

# Create a new post (creates it as draft=true)
hugo new content/en/posts/post-name.md
hugo new content/es/posts/post-name.md
```

The `public/` directory is committed to the repository — run `hugo` to regenerate it before committing content changes.

## Content structure

- Posts use **TOML frontmatter** (`+++` delimiters), with `date`, `draft`, and `title` fields
- Each post should exist in both `content/en/posts/` and `content/es/posts/` with matching filenames
- Images go in `static/images/` and are referenced as `/images/filename.ext`
- Set `draft = false` when a post is ready to publish

## Customization points

- `assets/css/extended/custom.css` — CSS overrides layered on top of PaperMod; keep additions here rather than editing the theme
- `hugo.toml` — all site config: languages, menus, params, social icons, search (Fuse.js), and table of contents settings
- `themes/PaperMod/` is a git submodule — do not edit files inside it

## Key config details

- `markup.goldmark.renderer.unsafe = true` allows raw HTML in markdown (used for the avatar `<img>` on the home page)
- Table of contents renders heading levels 2–4, open by default (`TocOpen = true`)
- Search requires the JSON output format, which is already configured in `[outputs]`