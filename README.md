# Media Tracker theme

A Hugo theme for keeping a personal log of movies, series, games and any other
media type. It is built as a Hugo Module on top of
[hugo-blog-awesome](https://github.com/hugo-sid/hugo-blog-awesome).

Features:

- Data-driven media types (`data/media_types.yml`) — add a type without touching templates
- Canonical statuses with translatable labels (`data/statuses.yml`)
- One unified listing with client-side search and multi-select filters
- Stats / dashboard page with rating distribution, per-year breakdowns, platform
  pie charts, and anime / cinema breakdowns
- Series ⇄ season relations shown as cover cards
- Social card metadata (OG / Twitter cards with cover image and personal review)
- RSS feeds with cover thumbnails, including a dedicated feed for finished items
- Collage generator (downloadable PNG, mobile-friendly)

## Media Tracker ecosystem

This theme is one piece of a small ecosystem that turns your media library into a
website. **New here? Don't start with the bare theme — clone the starter site below.**

- 🚀 **[mediatracker-starter](https://github.com/christt105/mediatracker-starter)** —
  **start here.** A ready-to-clone site that already imports this theme; deploy to
  GitHub Pages in minutes. ([live demo](https://christt105.github.io/mediatracker-starter/))
- 🎨 **hugo-mediatracker-theme** — this repo: the Hugo theme that renders the library.
- 📥 **[obsidian-mediatracker-plugin](https://github.com/christt105/obsidian-mediatracker-plugin)** —
  an Obsidian plugin that creates theme-compatible notes from TMDB, TheTVDB, IGDB,
  Steam and Open Library, so you don't have to write frontmatter by hand.

## Usage

Requires Hugo extended and Go (for modules).

1. Initialise your site as a module:

   ```bash
   hugo mod init github.com/you/your-site
   ```

2. Import the theme in your `hugo.toml`:

   ```toml
   [module]
     [[module.imports]]
       path = "github.com/christt105/hugo-mediatracker-theme"
   ```

3. Add content under sections that match `data/media_types.yml` (e.g.
   `content/movies/<title>/index.md`) with frontmatter like:

   ```yaml
   ---
   title: "Example"
   type: movie
   status: finished
   rating: "6"
   date: 2024-01-01
   image: cover.jpg
   ---
   ```

See [mediatracker-starter](https://github.com/christt105/mediatracker-starter)
for a ready-to-clone example, or the
[live site](https://christt105.github.io/MediaTracker/).

## Creating content

Entries are plain Markdown page bundles, so you can author them by hand. For a
faster workflow, the companion
[obsidian-mediatracker-plugin](https://github.com/christt105/obsidian-mediatracker-plugin)
for Obsidian searches TMDB, TheTVDB, IGDB and Steam and writes notes with the
exact frontmatter this theme expects (cover, banner, genres, ids, season
relations and more).

## Content conventions

### Series and seasons

The theme supports two patterns for TV shows:

**Single-season show** — one note, no separate season entries. The show note
carries the status, rating and completion date directly:

```
content/tv/blue-eye-samurai/
└── index.md   ← status: finished, rating: "7", date: 2024-01-15
```

**Multi-season show** — one note for the show plus one note per season.
The show note acts as a hub and its `status` should always be `not_started`
(it has no completion data of its own). Each season note holds the real
progress and rating:

```
content/tv/frieren/
└── index.md        ← status: not_started (hub; no rating or date)

content/seasons/frieren-season-1/
└── index.md        ← status: finished, rating: "10", date: 2024-03-22
                       season_number: 1, series: frieren

content/seasons/frieren-season-2/
└── index.md        ← status: in_progress
                       season_number: 2, series: frieren
```

The `series` field must match the **folder name** of the show entry exactly.
The theme automatically shows the seasons as cover cards on the show page, and
shows the parent show on each season page.

### Detecting anime and cinema in stats

The stats page can show an anime breakdown and a cinema-vs-streaming pie if the
relevant tags are present:

- **Anime**: add `anime` to a content entry's `tags`, or include `Animation` or
  `Anime` in its `genres`.
- **Cinema**: add `cinema` (or `cine`) to a movie entry's `tags`.

Both sections only appear when at least one matching finished item exists.

## Local development

To work on the theme against a site, add a `go.work` (not committed) in the site
root pointing at a local checkout:

```
go 1.24.4
use .
use ../hugo-mediatracker-theme
```
