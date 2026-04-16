# cop4331c

A MkDocs-based static site template for COP 4331C (Processes of Object Oriented Software Development), with Python tooling to generate and publish a documentation site to GitHub Pages.

## Description

This repository provides a MkDocs site template along with helper scripts to configure and deploy it. The main entry point, `generate_template.py`, reads the current git remote to infer the GitHub user and repository name, then substitutes values into `mkdocs.yml` and optionally updates the GitHub Actions deploy workflow to enable submodule support. The `docs/` directory holds assignments, lecture slides, and code demos organized for the course.

Additional utility scripts are included:

- **`overrides/hooks/on_env.py`** — A MkDocs hook that registers custom Jinja2 filters for time formatting (`convert_time`, `iso_time`, `to_local_time`), URL handling (`url_decode`, `get_last_part_URL`), and attachment configuration.
- **`overrides/hooks/on_post_page.py`** — A MkDocs hook that post-processes rendered HTML, replacing broken `ezlinks_not_found` anchor tags with `<span>` elements using `beautifulsoup4`.
- **`overrides/hooks/category.py`** — A CLI script to create a new category folder and `index.md` file inside the MkDocs docs directory.
- **`.github/find_unused_image.py`** — A CLI script to find (and optionally delete) images in the docs assets directory that are not referenced in any Markdown file.

## Requirements

- Python 3.13 or higher
- [uv](https://docs.astral.sh/uv/)

> [!warning]
> As Netlify doesn't support Python past 3.8, the template **can't** be used with Netlify.
> Only GitHub Pages is officially supported.

## Installation

1. Click on "use this template"
2. Create the new repository from the template
3. [Generate a new GH token with the `repo` and `workflow` scope](https://github.com/settings/tokens/new?scopes=repo,workflow)
4. In **Settings > Secrets and variables > Actions**:
   - Click on "New repository secret"
   - Name: `GH_TOKEN`
   - Value: `<your token>`

## Template Generation

1. In Actions, go to "Generate Website" and click on "Run workflow".
2. Fill in the items and click on "Run workflow".
3. Wait for the workflow to finish.

If auto-merging is not enabled:

- Read and validate the pull request, then merge it.
- You can then go into "Publish" and click on "Run workflow" to publish the website.

## Enabling Publishing with GitHub Pages

In **Settings**, go into **Pages** and:

- Set the **source** to "Deploy from a branch"
- Select the `gh-pages` branch, from root

> [!warning]
> You can't publish a private repository as a public page in the free tier of GitHub. But you can use a private submodule as your docs folder and publish it as a public page.
> If so, don't forget to set the `GH_TOKEN` secret and enable submodules in the `deploy.yml` file by setting `FETCH_SUBMODULE: true`.

## Dependencies

Declared in `pyproject.toml`:

| Package | Role |
|---|---|
| `mkdocs` | Static site generator |
| `mkdocs-material` | Material theme |
| `mkdocs-awesome-pages-plugin` | Page ordering |
| `mdx_breakless_lists` | Markdown extension |
| `mkdocs-embed-file-plugin` | File embedding |
| `mkdocs_custom_fences` | Custom code fences |
| `mkdocs-git-revision-date-localized-plugin` | Git-based revision dates |
| `mkdocs-encryptcontent-plugin` | Page encryption |
| `mkdocs-callouts` | Callout blocks |
| `mkdocs-custom-tags-attributes` | Custom tag attributes |
| `mkdocs-meta-descriptions-plugin` | Meta descriptions |
| `mkdocs-glightbox` | Image lightbox |
| `mkdocs-revealjs` | Reveal.js presentations |
| `mkdocs-obsidian-links` | Obsidian-style links |
| `beautifulsoup4` | HTML post-processing |
| `babel` | Locale/date formatting |

Development dependencies: `gitpython>=3.1.43`, `pydantic>=2.10.4`, `pyyaml>=6.0.2`, `rich>=13.9.4`.

## License

GNU General Public License v3.0 — see [LICENSE](LICENSE) for details.