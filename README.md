# ThriveCare website

The website for **ThriveCare** (thrivecare.ca): medical weight loss & menopause care. Supportive, medically guided and judgement-free.

Built with [Hugo](https://gohugo.io/) (extended) and a small custom theme (`themes/thrivecare`). There's no CSS framework and no build tooling beyond Hugo.

> **Status: draft.** All copy, services, FAQ answers, contact details, the provider bio and the footer disclaimer are **placeholders** marked "Draft", "Placeholder" or "edit me". Replace them before launch, and have any clinical wording and the disclaimer reviewed. Please don't publish medication names, promised results, statistics, prices, credentials or coverage claims until they're confirmed.

## Run it locally

1. Install Hugo **extended**, version 0.146 or newer (the site is built with 0.166.0): <https://gohugo.io/installation/>
2. From this folder, run:

   ```bash
   hugo server
   ```

3. Open the address it prints (for example <http://localhost:1313/rosiewebsite/>). Pages reload automatically as you edit.

To produce the production build in `public/`, run `hugo --minify`.

## How deploys work

`.github/workflows/hugo.yml` uses the official GitHub Pages pattern (`configure-pages`, `upload-pages-artifact`, `deploy-pages`):

- Every push to `main` (or a manual run from the **Actions** tab, "Run workflow") builds the site with Hugo extended and publishes it to GitHub Pages.
- Pages must be enabled with **Settings → Pages → Build and deployment → Source: GitHub Actions**.
- Current address: <https://sebbycorp.github.io/rosiewebsite/>

Note: GitHub Pages for a **private** repository requires a paid GitHub plan (Pro, Team or Enterprise).

## Editing content (no coding needed)

All text lives in Markdown/YAML files. Edit them on GitHub (pencil icon) or locally, and commit to `main` to publish.

| What | File |
| --- | --- |
| Home page: hero, "Care that feels different", "How it works" steps, provider teaser, closing call to action | `content/_index.md` (front matter at the top) |
| Services list (Services page and Home page preview) | `data/services.yaml` (set `featured: true` to show a service on Home) |
| Services page intro and note | `content/services/_index.md` |
| FAQ (Home page) | `data/faq.yaml` |
| About page and **provider bio** | `content/about.md` |
| Contact page text | `content/contact.md` |
| Email, phone, address, hours, booking link, footer disclaimer | `hugo.toml` under `[params]` |
| Navigation menu | `hugo.toml` under `[menus]` |

### Adding your bio and photo

Everything for the "Meet your provider" section is in **`content/about.md`**, in the `founder:` block at the top:

```yaml
founder:
  name: "Jane Example"            # your name
  title: "Your title / role"      # how you'd like to be introduced
  photo: "images/provider.jpg"    # your photo, see below
  photo_alt: "Portrait of ThriveCare's founder"
  bio: |
    First paragraph of your bio.

    Second paragraph (leave a blank line between paragraphs).
```

1. The current photo is `static/images/provider.jpg`. To change it, replace that file (same name) or add a new one to `static/images/`. A square image of at least 720×720 px works best; it is shown as a circle.
2. If you use a new file name, set `photo: "images/<file>"` (no leading slash) and update `photo_alt`.
3. Replace the name, title and bio text. The Home page "Meet your provider" teaser picks up the same name, title and photo automatically; its short intro sentence is `provider.text` in `content/_index.md`.

### Booking link

If you use an online booking tool, set `bookingURL = "https://..."` in `hugo.toml`. All "Book a consultation" buttons will then go there instead of the Contact page.

### Contact form

The form on the Contact page is static markup that opens the visitor's email app (`mailto:`). Nothing is stored on the website. Before launch, consider connecting a privacy-conscious form service (appropriate for Canadian privacy requirements) and updating the form's `action` in `themes/thrivecare/layouts/contact.html`.

### Links and images

Always use relative paths (no leading `/`) so the site works both at `/rosiewebsite/` and at the root of a custom domain. In templates, use `relURL`/`absURL`.

## Switching to https://thrivecare.ca later

1. **Update `baseURL`** in `hugo.toml` to `https://thrivecare.ca/`. (The deploy workflow passes the Pages URL automatically, but keep this in sync for local builds.)
2. **Add `static/CNAME`** containing a single line:

   ```
   thrivecare.ca
   ```

3. **In GitHub**: Settings → Pages → Custom domain → `thrivecare.ca`, save, then tick **Enforce HTTPS** once the certificate is issued. Optionally verify the domain under your account/organisation settings → Pages to prevent takeovers.
4. **DNS records** at your domain registrar:
   - Apex `thrivecare.ca`: four `A` records pointing to `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153` (optionally `AAAA` records `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`).
   - `www`: a `CNAME` record pointing to `sebbycorp.github.io`.
   - Check GitHub's current list: <https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site>
5. Push to `main` and the site redeploys. DNS changes can take up to 24–48 hours.

## Project structure

```
hugo.toml                     site settings, contact details, menu, disclaimer
content/                      page text (Markdown)
data/services.yaml            services list
data/faq.yaml                 FAQ
static/                       files copied as-is (images, favicon, later CNAME)
themes/thrivecare/            layouts (HTML templates) and CSS
.github/workflows/hugo.yml    GitHub Pages deploy
```
