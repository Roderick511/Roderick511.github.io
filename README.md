# Boyang Wu's Personal Website

A personal website built with Jekyll and [al-folio](https://github.com/alshedivat/al-folio). Routine maintenance involves editing Markdown pages, adding images, and publishing updates. You usually do not need to change the underlying templates.

- Website: [roderick511.github.io](https://roderick511.github.io/)
- Main sections: About, Academics, Career, Updates, and Photography.
- Updates uses one content collection: the homepage shows the latest three entries, the Updates page lists all entries, and each title links to a full article.
- The separate news module, publication features, sample articles, and external blog imports have been removed, along with the homepage's social icons.

## Contents

<!--ts-->

- [Common files](#common-files)
- [Editing personal information](#editing-personal-information)
- [Editing academic and work experience](#editing-academic-and-work-experience)
- [Publishing an update](#publishing-an-update)
- [Adding photography](#adding-photography)
- [Changing pages and appearance](#changing-pages-and-appearance)
- [Local preview](#local-preview)
- [Publishing to GitHub](#publishing-to-github)
- [Source files and generated files](#source-files-and-generated-files)

<!--te-->

## Common files

All paths below are relative to the repository root.

| What to change                                                     | Where to edit                                          |
| ------------------------------------------------------------------ | ------------------------------------------------------ |
| Name, site description, keywords, and domain                       | [`_config.yml`](_config.yml)                           |
| Homepage biography, subtitle, location, and profile image settings | [`_pages/about.md`](_pages/about.md)                   |
| Education, research, and teaching experience                       | [`_pages/Academics.md`](_pages/Academics.md)           |
| Work experience and skills                                         | [`_pages/Career.md`](_pages/Career.md)                 |
| Individual updates                                                 | `_posts/YYYY-MM-DD-title.md`                           |
| Updates page title and description                                 | [`_pages/Updates.md`](_pages/Updates.md)               |
| Photography groups, image order, and album links                   | [`_pages/Photography.md`](_pages/Photography.md)       |
| Profile pictures and other images                                  | `assets/img/`                                          |
| Shared update listing                                              | [`_includes/updates.liquid`](_includes/updates.liquid) |
| Individual article layout                                          | [`_layouts/post.liquid`](_layouts/post.liquid)         |

Git tracks the homepage as `_pages/about.md`. It may appear locally as `About.md`; do not create a second file that differs only in capitalization.

## Editing personal information

Edit `_pages/about.md`. The settings between the opening pair of `---` lines are called **front matter**. The text after the closing `---` is the page body.

- `subtitle`: the introduction below your name; it can include links.
- `profile.image`: the profile image filename, such as `prof_pic.jpg`. Place the image in `assets/img/`.
- `profile.image_circular`: set to `true` for a circular profile image.
- `profile.more_info`: location or other information below the image.
- Page body: replace the template instructions with your biography. Use Markdown for paragraphs, lists, and links.

Edit site-wide information in `_config.yml`, for example:

```yaml
title: Boyang Wu
first_name: Boyang
last_name: Wu
description: "Boyang Wu's personal website."
keywords: Boyang Wu, computer engineering, software development
```

The current personal GitHub Pages site uses this URL and an empty base path. Keep these settings consistent:

```yaml
url: https://Roderick511.github.io
baseurl:
```

Use spaces for YAML indentation. Quote text containing special characters such as a colon followed by a space.

## Editing academic and work experience

Edit `_pages/Academics.md` and `_pages/Career.md` directly. These pages combine Markdown headings with HTML cards:

- To edit an entry, replace the school, company, role, dates, and description in its card.
- To add an entry, copy a complete `<div class="card mt-3 p-3"> ... </div>` block, including all nested elements, and replace its content.
- To reorder entries, move entire card blocks.
- To add an achievement, insert `<li>Your achievement</li>` inside the card's `<ul>` list.

These pages do not currently read from `_data/cv.yml` or `assets/json/resume.json`. Editing those resume files will not update Academics or Career.

## Publishing an update

Write every update in `_posts/`. Create the directory if it does not exist. There is no separate news collection or duplicate homepage entry to maintain.

For example, create `_posts/2026-11-09-website-update.md`:

```markdown
---
layout: post
title: "Website update"
date: 2026-11-09 12:00:00 -0600
description: "I have reorganized my website and will share recent updates here."
---

I have brought all website updates together in one place.

## What I am working on

- Updating my personal introduction
- Organizing projects and photography

You can also write longer articles or include [project links](https://github.com/Roderick511).
```

Replace the example date and text with your actual content before publishing:

- Use the filename pattern `YYYY-MM-DD-short-title.md`, such as `2026-11-09-website-update.md`.
- `title` is the displayed heading; `description` is the summary shown in listings.
- If `description` is omitted, the listing uses a shortened excerpt from the body.
- `date` controls ordering, with the newest entries first. Future-dated entries are not published by default; setting a future date alone does not schedule a deployment.
- The example uses `-0600` for US Central Standard Time. Use `-0500` during daylight saving time.
- Article URLs follow `/updates/year/filename-slug/`. Renaming a file may change its URL.
- To keep an entry unpublished, add `published: false` to its front matter. Remove that line or set it to `true` when ready.

**Adding images:** place the file in `assets/img/updates/`, creating the directory as needed, and reference it in the article:

```liquid
{%
  include figure.liquid
  path="assets/img/updates/my-photo.jpg"
  alt="A description of the photo"
  class="img-fluid rounded"
  zoomable=true
%}
```

The image must exist at the specified path, including matching capitalization. This component uses the site's existing image processing and zoom behavior.

**Changing the homepage limit:** edit these settings in `_pages/about.md`:

```yaml
updates:
  enabled: true
  limit: 3
```

`limit` affects only the homepage. `/updates/` still lists all published entries. When there are no entries, both pages display “No updates yet.”

## Adding photography

1. Place photos in `assets/img/photography/city-name/`, for example `assets/img/photography/Chicago/lake.jpg`.
2. Edit `_pages/Photography.md`. Replace an existing `figure.liquid` image path, or copy a photo block to add another image.
3. To add a city, copy a complete city section and change its heading and image paths.
4. Replace `href="#"` on each `Explore full album` link with the actual album URL. If there is no album, remove that link paragraph.

Compress images before adding them, use short filenames, and provide an `alt` description for each photo. The three existing city groups currently reuse the same three placeholder images; replace them with the appropriate photos.

## Changing pages and appearance

A regular page's front matter controls its title, URL, and navigation order:

```yaml
---
layout: page
title: Photography
permalink: /photography/
nav: true
nav_order: 4
---
```

- `nav: true` includes the page in navigation. Lower `nav_order` values appear first.
- `nav: false` hides only the navigation entry. The page may still be generated and accessible by URL.
- To add a regular page, create a Markdown file in `_pages/` with these settings and your content.
- The homepage is identified by `permalink: /` and normally does not need a `nav` setting.

The Projects page still exists but is hidden from navigation. Replace the sample content in `_projects/` before enabling it.

Routine content changes do not require editing `_layouts/`, `_includes/`, or `_sass/`. For appearance changes, use these locations:

| Setting                      | Location                                             |
| ---------------------------- | ---------------------------------------------------- |
| Maximum content width        | `max_width` in `_config.yml`                         |
| Dark mode                    | `enable_darkmode` in `_config.yml`                   |
| Theme colors                 | `_sass/_themes.scss`                                 |
| Style variables              | `_sass/_variables.scss`                              |
| Shared navigation and footer | `_includes/header.liquid`, `_includes/footer.liquid` |

## Local preview

Local previews use Docker. The repository is mounted at `/srv/jekyll` inside the container, and the preview is available at [localhost:8080](http://localhost:8080/). Start Docker Desktop and run the following commands from the repository root.

### Refreshing the existing preview

The preview previously started for this project serves an existing build without watching for changes. After editing, run:

```bash
docker compose exec -T -e JEKYLL_ENV=production jekyll bundle exec jekyll build
```

Refresh the browser after the build succeeds. Ordinary text and image changes do not require rebuilding the Docker image.

### Starting a development preview with automatic rebuilds

To rebuild automatically when content changes, stop the existing preview service and start a development container in the foreground:

```bash
docker compose stop jekyll
docker compose run --rm --service-ports jekyll sh -c 'bundle install && bundle exec jekyll serve --host 0.0.0.0 --port 8080 --livereload --force_polling'
```

Keep the terminal running. Press `Ctrl+C` to end the preview; the temporary development container is removed automatically, while source files and the Docker image remain. Restart this command after changing `_config.yml`. Do not run two preview servers on port 8080 at the same time.

An existing local image is reused. On a new machine without the image, run `docker compose pull` first. Consider rebuilding the image when dependencies or the Dockerfile change.

### Troubleshooting

- **`service "jekyll" is not running`:** no Compose service is running. Use the development preview command above. That mode rebuilds automatically, so a separate `compose exec` build is unnecessary.
- **Asked to run `bundle install`, or a Git-sourced gem is missing:** the default startup script may remove an untracked `Gemfile.lock`. The development preview command above installs dependencies before starting Jekyll.
- **Content does not appear:** check the filename, front matter delimiters, article date, and `published` setting. Confirm that the build succeeded, then refresh.
- **An image does not load:** check that the file exists and that its path and capitalization match the reference.

## Publishing to GitHub

Saving files locally changes only the local source. Website changes trigger deployment after they are pushed to the repository's `main` branch.

1. Review `git diff` and confirm that it contains only the changes you intend to publish.
2. Before committing, format according to the repository guidelines and complete a local Docker build and page check:

   ```bash
   npm install
   npx prettier . --write
   ```

   Run `npm install` only when formatting tools are missing or dependencies have changed. Formatting the entire repository can affect other files, so review `git diff` again afterward.

3. Stage specific files with `git add`, then commit and push. This example publishes one update; replace the path with your actual file and stage any related images separately:

   ```bash
   git add _posts/2026-11-09-website-update.md
   git commit -m "feat: add website update"
   git push origin main
   ```

4. Wait for the [Deploy site workflow](.github/workflows/deploy.yml) to finish, then check the live site.

The generated website is published to the `gh-pages` branch. Make routine changes to source files on `main`; do not edit `gh-pages` directly or commit `_site/`.

Changes to `README.md` alone do not trigger site deployment because the deployment workflow explicitly excludes it.

`Deploy site` builds and publishes the website, so it must succeed for a push to update the live pages. A failed deployment leaves the previous version online.
Optional CV generation, automatic Markdown table-of-contents updates, upstream Docker image publishing, and link-checking workflows have been removed. Maintain the README contents manually; these removed workflows no longer write generated changes back to `main`.

If a run fails before any steps start, check the run's **Annotations** for account or runner errors. Disabling unrelated checks does not resolve a deployment error or an account restriction. A skipped job simply did not run.

## Source files and generated files

| Type                                      | Location                                           | Maintenance                                                               |
| ----------------------------------------- | -------------------------------------------------- | ------------------------------------------------------------------------- |
| Pages, updates, images, and configuration | `_pages/`, `_posts/`, `assets/img/`, `_config.yml` | Edit and commit these source files                                        |
| Generated website                         | `_site/`                                           | Regenerated by the build; do not edit manually                            |
| Jekyll cache                              | `.jekyll-cache/`, `.jekyll-metadata`               | Managed automatically                                                     |
| Formatting dependencies                   | `node_modules/`                                    | Managed by npm                                                            |
| Ruby dependency lockfile                  | `Gemfile.lock`                                     | Generated by Bundler; currently ignored by this repository's `.gitignore` |
| Docker images, containers, and cache      | Docker Desktop storage                             | Keep out of Git; no cleanup is needed after each content change           |

Older documents such as `CUSTOMIZE.md` and `INSTALL.md` describe the upstream template's full feature set, including features removed from this site. For everyday maintenance, follow this README and the current source files.

Theme: [al-folio](https://github.com/alshedivat/al-folio). See [LICENSE](LICENSE) for the original theme license.
