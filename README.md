# Debate Across Arkansas

Website for Debate Across Arkansas, a nonprofit that supports speech and debate in Arkansas. The site is a [Hugo](https://gohugo.io/) project with a small custom theme in `layouts/` and `assets/`. There is no third-party theme to upgrade.

The pages ship with placeholder copy. Each placeholder page is marked on the page itself. Replace that copy with the organization's own words. Do not publish invented programs, people, dates, amounts, or contact details.

## Run locally

Install [Hugo](https://gohugo.io/installation/). This site was built with Hugo 0.152.2. Dart Sass is not required.

From the repository root:

```bash
hugo server
```

Open the URL Hugo prints. With the base URL in `hugo.toml`, that is <http://localhost:1313/debate-across-arkansas/>. Hugo serves the local site on the same subpath the production site uses, so links match GitHub Pages.

To build the site the way GitHub Actions does:

```bash
hugo --gc --minify --baseURL "https://hstings.github.io/debate-across-arkansas/"
```

Hugo writes the finished site to `public/`. That folder is gitignored. Do not commit it.

## Brand color

The light blue is the CSS variable `--brand` in `assets/css/main.css`. Change that one value to recolor the state mark, buttons, and bands.

Two spots cannot read a CSS variable, so they repeat the same hex:

- `static/favicon.svg` (the favicon fill)
- the `theme-color` meta tag in `layouts/partials/head.html`

Update those when you change `--brand`.

The state mark is an SVG outline of Arkansas in `assets/mark.svg`, from Natural Earth public-domain boundaries. The header uses it in the brand color. The same shape is the favicon.

Inter and Newsreader are included under the SIL Open Font License. The license texts are `static/fonts/OFL-Inter.txt` and `static/fonts/OFL-Newsreader.txt`.

## Add a page

Create a Markdown file in `content/`:

```bash
hugo new about-the-board.md
```

`archetypes/default.md` starts the file as placeholder copy. Edit the title, description, and body. The new page is published at a URL based on the filename.

To show it in the header and footer, add a menu entry in `hugo.toml`:

```toml
[[menus.main]]
  name = 'Board'
  pageRef = '/about-the-board'
  weight = 70
```

Set `draft: true` in the front matter to hide a page until it is ready. Remove `placeholder: true` when the copy should be indexed by search engines. While that flag is set, the page template adds `noindex`.

Links written as root paths, such as `[events](/events/)`, are prefixed with the site base path so they keep working on GitHub Pages.

## Add an event

Events are a Hugo section. Each event is a Markdown file in `content/events/`, and the events page lists them.

```bash
hugo new events/spring-meeting.md
```

That command uses `archetypes/events.md`. Write the title, the `summary` (shown on the events list), and the body. Keep `example: false`. The list is ordered by `weight`, lowest first.

`content/events/example-event.md` is a sample entry. The site labels it as an example. Delete that file when you no longer want it on the events page.

The event template does not print a date. Add a date to the template only when a real date should be published.

## Deployment

Pushes to `main` run [`.github/workflows/hugo.yml`](.github/workflows/hugo.yml). The workflow installs Hugo, builds the site with this base URL, and deploys the `public/` folder to GitHub Pages:

`https://hstings.github.io/debate-across-arkansas/`

Pull requests run the same build and do not deploy.

One-time GitHub Pages setting: in the repository, open **Settings > Pages > Build and deployment > Source** and choose **GitHub Actions**.

After the first successful run on `main`, the site is published at the URL above. `baseURL` in `hugo.toml` and `--baseURL` in the workflow both use that address, including the `/debate-across-arkansas/` subpath, so links, styles, fonts, and the favicon resolve on the project site.
