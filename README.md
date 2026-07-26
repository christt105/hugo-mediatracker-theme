# Media Tracker theme

![Screenshot](https://raw.githubusercontent.com/christt105/hugo-mediatracker-theme/main/images/screenshot.png)

A Hugo theme for tracking movies, series, games and any other media type.
Built as a Hugo Module on top of [hugo-blog-awesome](https://github.com/hugo-sid/hugo-blog-awesome).

Features:

- Data-driven media types via `data/media_types.yml`; adding a new type requires no template changes
- Canonical statuses with translatable labels (`data/statuses.yml`)
- Unified listing with client-side search and multi-select filters
- Stats page with rating distribution, per-year breakdowns, platform charts, and anime/cinema breakdowns
- Series and season relations shown as cover cards
- Social card metadata (OG/Twitter cards with cover image and review)
- RSS feeds with cover thumbnails, including a dedicated feed for finished items
- Collage generator (downloadable PNG, mobile-friendly)

## Media Tracker ecosystem

This theme is part of a small set of tools for turning a media library into a website.
If you are new here, start with the starter site rather than the bare theme.

- **[mediatracker-starter](https://github.com/christt105/mediatracker-starter)**: a ready-to-clone site that already imports this theme and deploys to GitHub Pages in minutes. ([live demo](https://christt105.github.io/mediatracker-starter/))
- **hugo-mediatracker-theme**: this repo, the Hugo theme that renders the library.
- **[obsidian-mediatracker-plugin](https://github.com/christt105/obsidian-mediatracker-plugin)**: an Obsidian plugin that creates theme-compatible notes from TMDB, TheTVDB, IGDB, Steam and Open Library.

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
for a ready-to-clone example, or the [live site](https://christt105.github.io/MediaTracker/).

## Creating content

Entries are plain Markdown page bundles and can be written by hand. The companion
[obsidian-mediatracker-plugin](https://github.com/christt105/obsidian-mediatracker-plugin)
searches TMDB, TheTVDB, IGDB and Steam and writes notes with the frontmatter this
theme expects (cover, banner, genres, IDs, season relations and more).

## Content conventions

### Series and seasons

The theme handles two patterns for TV shows:

**Single-season show**: one note with no separate season entries. The show note
carries the status, rating and completion date directly:

```
content/tv/blue-eye-samurai/
└── index.md   ← status: finished, rating: "7", date: 2024-01-15
```

**Multi-season show**: one note for the show plus one per season.
The show note acts as a hub; its `status` should be `not_started`
since it holds no completion data of its own. Each season note holds the real
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
The theme shows seasons as cover cards on the show page and links back to the
parent show on each season page.

### Anime and cinema stats

The stats page shows an anime breakdown and a cinema-vs-streaming chart when
the relevant tags or genres are present:

- **Anime**: add `anime` to the entry's `tags`, or include `Animation` or `Anime` in `genres`.
- **Cinema**: add `cinema` (or `cine`) to a movie entry's `tags`.

Both sections only appear when at least one matching finished item exists.

## Screenshots

![Main gallery](https://raw.githubusercontent.com/christt105/hugo-mediatracker-theme/main/images/screenshot.png)

![Filter bar and search](https://raw.githubusercontent.com/christt105/hugo-mediatracker-theme/main/images/filter-search.png)

![Statistics page](https://raw.githubusercontent.com/christt105/hugo-mediatracker-theme/main/images/stats.png)

![Collage generator](https://raw.githubusercontent.com/christt105/hugo-mediatracker-theme/main/images/collage.png)

## Frontmatter reference

### Common fields (all types)

| Field | Type | Description |
|-------|------|-------------|
| `title` | string | Entry title |
| `date` | date | Completion date, used for per-year stats and card sorting |
| `type` | string | Media type key from `data/media_types.yml`: `movie`, `tv`, `season`, `videogame`, `book` |
| `status` | string | `finished`, `in_progress`, `not_started`, `paused`, `dropped` |
| `rating` | string | Numeric rating as a string (`"7"`, `"10"`) |
| `image` | string | Cover image filename in the page bundle |
| `banner_image` | string | Wide banner image filename in the page bundle |
| `overview` | string | Synopsis shown on the entry page |
| `release_date` | string | Original release date of the title (`"2024-03-15"`) |
| `genres` | list | Genre strings; `Anime` or `Animation` activates the anime breakdown in stats |
| `tags` | list | Free tags; `cinema` or `cine` activates the cinema vs. streaming chart in stats |
| `rewatches` | list | Repeat viewing dates, one date string per entry (`- 2024-06-15`) |

### Type-specific fields

| Field | Type | Types | Description |
|-------|------|-------|-------------|
| `tmdb_id` | string | `movie`, `tv`, `season` | TMDB ID, links the badge on the entry page |
| `steam_appid` | string | `videogame` | Steam app ID, links the badge |
| `platforms` | list | `videogame` | Platforms played on, shown in the stats pie chart |
| `developer` | string | `videogame` | Game developer |
| `author` | string | `book` | Book author |
| `series` | string | `season` | Exact folder name of the parent TV show entry |
| `season_number` | integer | `season` | Season number, used for ordering on the show page |

### Archetypes

The theme includes archetypes for each built-in content section. Running
`hugo new movies/my-film/index.md` (or the equivalent for `tv`, `seasons`,
`games`, `books`) generates a pre-filled frontmatter for that type. The
`default` archetype includes all fields and works as a fallback for custom types.

## Local development

To work on the theme against a site, add a `go.work` (not committed) in the site
root pointing at a local checkout:

```
go 1.24.4
use .
use ../hugo-mediatracker-theme
```
