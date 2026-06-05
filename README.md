# Media Tracker theme

A Hugo theme for keeping a personal log of movies, series, games and any other
media type. It is built as a Hugo Module on top of
[hugo-blog-awesome](https://github.com/hugo-sid/hugo-blog-awesome).

Features:

- Data-driven media types (`data/media_types.yml`) — add a type without touching templates
- Canonical statuses with translatable labels (`data/statuses.yml`)
- One unified listing with client-side search and multi-select filters
- A stats / dashboard page
- Series ⇄ season relations shown as cover cards
- Social card metadata, RSS feeds with cover thumbnails

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

## Local development

To work on the theme against a site, add a `go.work` (not committed) in the site
pointing at a local checkout:

```
go 1.24.4
use .
use ../hugo-mediatracker-theme
```
